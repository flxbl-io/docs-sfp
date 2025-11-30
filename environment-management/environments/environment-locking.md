---
icon: ring-diamond
---

# Environment Locking

{% hint style="info" %}
This feature requires **sfp-pro** with sfp-server
{% endhint %}

Environment locking prevents concurrent access to shared environments. When a pipeline or developer locks an environment, others must wait until the lock is released, ensuring deployments don't conflict.

## Why Lock Environments?

Without locking, multiple operations can interfere with each other:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Without Locking (Problem)                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Pipeline A ────────> Deploy v1.0 ─────────┐                   │
│                                             │                   │
│   Pipeline B ────────> Deploy v1.1 ────┐    │                   │
│                                        ▼    ▼                   │
│                                   ┌──────────────┐              │
│                                   │     UAT      │ Conflict!    │
│                                   │   (broken)   │              │
│                                   └──────────────┘              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    With Locking (Solution)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Pipeline A ── Lock ──> Deploy v1.0 ── Unlock ─┐               │
│                                                 │               │
│   Pipeline B ── Wait... ─────────────── Lock ───┼──> Deploy v1.1│
│                                                 ▼               │
│                                           ┌──────────────┐      │
│                                           │     UAT      │      │
│                                           │  (working)   │      │
│                                           └──────────────┘      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## How Locking Works

### Lock Request Flow

1. **Request Lock**: Pipeline requests a lock, gets a ticket ID
2. **Queue Position**: If environment is locked, request is queued
3. **Acquire Lock**: When available, pipeline acquires the lock
4. **Perform Operations**: Deploy, test, etc.
5. **Release Lock**: Unlock when finished

### Lock Properties

| Property        | Description                              |
| --------------- | ---------------------------------------- |
| `ticketId`      | Unique identifier for the lock request   |
| `lockedBy`      | Who holds the lock (user or application) |
| `lockReason`    | Why the lock was acquired                |
| `expiresAt`     | When the lock expires                    |
| `leaseDuration` | How long the lock is valid               |

## Requesting a Lock

### Basic Lock Request

```bash
sfp server environment lock \
  --name UAT \
  --repository myorg/salesforce-app \
  --reason "Deploying release v2.0"
```

Output:

```
Lock requested for environment: UAT
Ticket ID: lock-abc123
Status: queued
Queue Position: 1
Estimated Wait: 15 minutes
```

### With Lease Duration

```bash
sfp server environment lock \
  --name UAT \
  --repository myorg/salesforce-app \
  --reason "Long-running deployment" \
  --lease-duration 60  # 60 minutes
```

## Acquiring the Lock

Once your request reaches the front of the queue:

```bash
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --lock-ticket-id lock-abc123 \
  --authenticate
```

This command:

1. Waits for the lock to be available (polls every 30 seconds)
2. Acquires the lock when available
3. Retrieves credentials
4. Authenticates locally

### Wait for Lock (Automatic)

The `--lock-ticket-id` flag automatically waits for lock acquisition:

```bash
# This will wait up to 60 minutes for the lock
sfp server environment get \
  --name UAT \
  --repository myorg/salesforce-app \
  --lock-ticket-id lock-abc123 \
  --authenticate

# Output during wait:
# ⏳ Still waiting... Position in queue: 2
# ⏳ Still waiting... Position in queue: 1
# ✅ Lock acquired successfully!
```

## Releasing a Lock

### Manual Release

```bash
sfp server environment unlock \
  --name UAT \
  --repository myorg/salesforce-app \
  --lock-ticket-id lock-abc123
```

### Automatic Expiration

Locks automatically expire after the lease duration. This prevents orphaned locks from blocking deployments indefinitely.

## CI/CD Integration

### GitHub Actions with Locking

```yaml
jobs:
  deploy-to-uat:
    runs-on: ubuntu-latest
    env:
      SFP_SERVER_URL: ${{ secrets.SFP_SERVER_URL }}
      SFP_SERVER_TOKEN: ${{ secrets.SFP_SERVER_TOKEN }}
    steps:
      - uses: actions/checkout@v4

      - name: Request Lock
        id: lock
        run: |
          RESULT=$(sfp server environment lock \
            --name UAT \
            --repository ${{ github.repository }} \
            --reason "PR #${{ github.event.number }} deployment" \
            --json)
          echo "ticket_id=$(echo $RESULT | jq -r '.ticketId')" >> $GITHUB_OUTPUT

      - name: Acquire Lock and Authenticate
        run: |
          sfp server environment get \
            --name UAT \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }} \
            --authenticate

      - name: Deploy
        run: |
          sfp install --targetorg UAT --artifactdir ./artifacts

      - name: Release Lock
        if: always()  # Run even if deployment fails
        run: |
          sfp server environment unlock \
            --name UAT \
            --repository ${{ github.repository }} \
            --lock-ticket-id ${{ steps.lock.outputs.ticket_id }}
```

### Handling Lock Timeouts

```yaml
- name: Acquire Lock with Timeout
  timeout-minutes: 30  # CI timeout
  run: |
    sfp server environment get \
      --name UAT \
      --repository ${{ github.repository }} \
      --lock-ticket-id ${{ steps.lock.outputs.ticket_id }} \
      --authenticate
```

## Lock Queue Management

### View Queue Status

```bash
sfp server environment get --name UAT --repository myorg/salesforce-app --json
```

Output:

```json
{
  "name": "UAT",
  "isLocked": true,
  "lockStatus": {
    "isLocked": true,
    "currentLock": {
      "lockedBy": "ci-pipeline-main",
      "lockReason": "Release deployment",
      "expiresAt": "2024-01-15T15:00:00Z",
      "expiresInSeconds": 1200
    },
    "queuedLocks": [
      {
        "position": 1,
        "requestedBy": "ci-pipeline-pr-123",
        "estimatedWaitSeconds": 1200,
        "ticketId": "lock-abc123"
      },
      {
        "position": 2,
        "requestedBy": "developer@company.com",
        "estimatedWaitSeconds": 2400,
        "ticketId": "lock-def456"
      }
    ]
  }
}
```

### Cancel a Lock Request

```bash
# Cancel your pending lock request
sfp server environment unlock \
  --name UAT \
  --repository myorg/salesforce-app \
  --lock-ticket-id lock-abc123
```

## Lock Credentials Security

When you acquire a lock, credentials are only revealed at that point:

```
Request Lock
     │
     │ No credentials exposed
     ▼
Waiting in Queue
     │
     │ No credentials exposed
     ▼
Lock Acquired ◄─── Credentials returned HERE
     │
     │ accessToken or sfdxAuthUrl
     ▼
Perform Operations
     │
     ▼
Release Lock
```

This ensures credentials are only provided to the lock holder.

## Troubleshooting

### Lock Not Releasing

Check if the lock holder is still active:

```bash
sfp server environment get --name UAT --repository myorg/salesforce-app --json
```

If the lock is orphaned (holder crashed), wait for expiration or contact an admin.

### Stuck in Queue

* Check queue position
* Verify your ticket ID is correct
* Check if you have permission to lock

### "Lock already held"

You already hold a lock on this environment:

```bash
# Release existing lock first
sfp server environment unlock --name UAT --repository myorg/salesforce-app --lock-ticket-id existing-ticket
```

### Credentials Not Returned

Ensure you're using `--lock-ticket-id` when fetching:

```bash
# Wrong - won't get credentials
sfp server environment get --name UAT --repository myorg/salesforce-app

# Correct - gets credentials after lock acquisition
sfp server environment get --name UAT --repository myorg/salesforce-app --lock-ticket-id lock-abc123
```

## Related Topics

* [Environments](./) - Environment management overview
* [Server Authentication](../../authentication/server-authentication.md) - Authenticate with sfp-server
* [Accessing Environments](accessing-environments.md) - Practical examples

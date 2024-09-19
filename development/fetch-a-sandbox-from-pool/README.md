---
icon: ring-diamond
---

# Fetch a Sandbox from Pool

|              | sfp-pro      | sfp (community) |
| ------------ | ------------ | --------------- |
| Availability | ✅            | ❌               |
| From         | September 24 |                 |

The `sfp pool sandbox fetch` command is used to fetch an available sandbox from a pool and assign it to a specific issue or pull request.

### Usage

```
sfp pool sandbox fetch --repository <owner/repo> --pool <pool-name> --branch <branch-name> --issue <issue-number> [--devhubalias <alias>] [--wait <minutes>] [--leasefor <minutes>]
```

### Flags

* `--repository, -r`: The repository path (e.g., `owner/repo`). Default is the `GITHUB_REPOSITORY` environment variable.
* `--pool, -p`: The name of the pool to fetch the sandbox from (required).
* `--branch, -b`: The branch of the pool from where the environment is to be fetched (required).
* `--issue, -i`: Issue number to be associated with the sandbox (required).
* `--devhubalias`: The DevHub alias associated with the pool (default: 'devhub').
* `--wait`: Time in minutes to wait for an available sandbox (default: 20).
* `--leasefor`: Time in minutes to lease the sandbox for the current task (default: 15).

### Process

1. Checks for existing sandbox assignments to the issue
2. If no assignment, acquires a lock on the pool
3. Fetches an available sandbox
4. Assigns the sandbox to the specified issue
5. Sets GitHub Actions output variable (if in GitHub Actions environment)
6. Releases the lock on the pool

### Leasing

The `--leasefor` parameter specifies how long the sandbox is reserved for the current task before it can be reassigned within the same issue context. This is different from the overall expiration time of the sandbox in the pool.

### Example

```
sfp pool sandbox fetch --repository myorg/myrepo --pool dev-pool --branch feature/new-feature --issue 1234 --leasefor 60
```

This fetches a sandbox from 'dev-pool' for the branch 'feature/new-feature', assigns it to issue 1234, and leases it for 60 minutes.

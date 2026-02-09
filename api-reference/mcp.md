# MCP (Model Context Protocol)

sfp server exposes an MCP endpoint that lets AI assistants (Claude, GitHub Copilot, Cursor, OpenCode) fetch Jira work items, manage review environments, and query pool configurations — enabling end-to-end AI-assisted Salesforce development.

## Endpoint

```
POST /sfp/api/mcp       (JSON-RPC 2.0 — tool calls, initialize, etc.)
GET  /sfp/api/mcp       (SSE stream for notifications)
DELETE /sfp/api/mcp     (close session)
GET  /sfp/api/mcp/tools (REST — list available tools)
```

| Property | Value |
|----------|-------|
| Transport | Streamable HTTP |
| Protocol | MCP 2025-06-18 |
| Auth | `Authorization: Bearer <application-token>` |
| Content-Type | `application/json` |
| Accept | `application/json, text/event-stream` |

## Quick Start

Follow these steps to connect your AI tool to sfp server:

1.  **Ensure sfp-pro server is running**
    The server must be accessible (e.g., `http://localhost:3029` or `https://sfp.your-domain.com`).

2.  **Create an application token**
    You need a dedicated token for MCP clients.
    ```bash
    # Login first if needed
    sfp server auth login

    # Create token (save the output UUID!)
    sfp server application-token create --name "My MCP Token" --expires-in 90
    ```

3.  **Configure your AI client**
    Add the server URL and token to your client configuration (see [Connecting AI Clients](#connecting-ai-clients)).

4.  **Verify the connection**
    Ask your AI: *"List the available sandbox pools for this repository."*

## Authentication

The MCP endpoint requires authentication via an **Application Token**.

- **Header**: `Authorization: Bearer <application-token>`
- **Role**: Application (recommended), Owner, or Member

### Creating Tokens

{% tabs %}
{% tab title="CLI" %}
```bash
# Create a token valid for 90 days
sfp server application-token create --name "Claude Desktop" --expires-in 90
```
{% endtab %}

{% tab title="API" %}
```bash
# Requires existing JWT token
curl -X POST https://your-server/sfp/api/application-tokens \
  -H "Authorization: Bearer <your-jwt>" \
  -H "Content-Type: application/json" \
  -d '{"name": "Claude Desktop", "expiresIn": 90}'
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Important**: The token is only displayed once upon creation. Save it immediately in a secret manager or CI/CD vault.
{% endhint %}

{% hint style="info" %}
**Security**: Application tokens authenticate as the `Application` role. The MCP endpoint also accepts Owner and Member JWT tokens. Rotate tokens regularly and use short expiration times for CI/CD.
{% endhint %}

## Connecting AI Clients

Configure your AI tool to connect to the sfp MCP server.

{% tabs %}
{% tab title="Claude Desktop" %}
Edit `~/.claude/mcp-config.json` (macOS/Linux) or `%APPDATA%\Claude\mcp-config.json` (Windows).

**Note**: Claude Desktop requires `mcp-remote` to bridge to HTTP servers.

```json
{
  "mcpServers": {
    "sfp": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://your-server/sfp/api/mcp",
        "--header", "Authorization:Bearer YOUR_APP_TOKEN",
        "--transport", "http-only"
      ]
    }
  }
}
```
{% endtab %}

{% tab title="Claude Code (CLI)" %}
Run the add command:

```bash
claude mcp add sfp --transport http https://your-server/sfp/api/mcp \
  --header "Authorization: Bearer YOUR_APP_TOKEN"
```

Or edit `.claude/settings.json`:

```json
{
  "mcpServers": {
    "sfp": {
      "type": "http",
      "url": "https://your-server/sfp/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_APP_TOKEN"
      }
    }
  }
}
```
{% endtab %}

{% tab title="GitHub Copilot" %}
Edit `.vscode/mcp.json` or `~/.vscode/mcp.json`:

```json
{
  "mcpServers": {
    "sfp": {
      "type": "http",
      "url": "https://your-server/sfp/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_APP_TOKEN"
      }
    }
  }
}
```
{% endtab %}

{% tab title="Cursor" %}
Go to **Settings > MCP Servers > Add**:

- **Type**: `Streamable HTTP`
- **URL**: `https://your-server/sfp/api/mcp`
- **Headers**: `Authorization: Bearer YOUR_APP_TOKEN`

Or edit `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "sfp": {
      "type": "http",
      "url": "https://your-server/sfp/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_APP_TOKEN"
      }
    }
  }
}
```
{% endtab %}

{% tab title="OpenCode" %}
Edit `opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "sfp": {
      "type": "remote",
      "url": "https://your-server/sfp/api/mcp",
      "oauth": false,
      "headers": {
        "Authorization": "Bearer {env:SFP_APP_TOKEN}"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Available Tools

### `pools`

List sandbox pool configurations.

- **Action**: `list_pools`
- **Parameters**:
  - `action` (required): Must be `list_pools`
  - `repositoryIdentifier` (optional): Repository in `owner/repo` format
  - `poolType` (optional): `SANDBOX` or `SCRATCH_ORG`
  - `hasAssignmentRules` (optional): Filter by assignment rules (boolean)

**Example response** (via `tools/call`):

```json
{
  "result": {
    "content": [
      {
        "type": "text",
        "text": "Found 2 pool(s) for flxbl-io/sf-core:\n- dev-pool (SANDBOX, 5 available)\n- scratch-ci (SCRATCH_ORG, 3 available)"
      }
    ]
  }
}
```

### `review_envs`

Fetch and manage review environments.

- **Actions**: `fetch_review_env`, `release_review_env`
- **Parameters (fetch)**:
  - `action` (required): `fetch_review_env`
  - `assignmentType` (required): `pr`, `issue`, `user`, or `custom`
  - `assignmentId` (required): ID (PR number, issue key, username)
  - `repositoryIdentifier` (required): Repository in `owner/repo` format
  - `poolTag` (optional): Specific pool tag
  - `branch` (optional): Branch name for pool resolution
  - `domains` (optional): Array of domains
  - `domain` (optional): Single domain
  - `expirationHours` (optional): 1-720
  - `isImmortal` (optional): boolean
  - `metadata` (optional): object
  - `override` (optional): boolean

### `work_items`

Get or list work items from Jira.

- **Actions**: `get_work_item`, `list_work_items`
- **Parameters**:
  - `action` (required): `get_work_item` or `list_work_items`
  - `projectIdentifier` (optional): `owner/repo` (platform derived from config)
  - `id` (optional): Work item ID (for `get_work_item`)
  - `status` (optional): Filter by status
  - `assignee` (optional): Filter by assignee
  - `type` (optional): Filter by type
  - `maxResults` (optional): 1-100 (default 50)

## Verification and Debugging

### Step 1: Verify auth with curl

```bash
# Without token — should return 401
curl -s https://your-server/sfp/api/mcp/tools
# {"statusCode":401,"message":"No authentication token provided"}

# With token — should return tools list
curl -s -H "Authorization: Bearer YOUR_TOKEN" \
  https://your-server/sfp/api/mcp/tools
```

### Step 2: Test MCP protocol

```bash
# Initialize handshake (note: both Accept types required)
curl -s -X POST https://your-server/sfp/api/mcp \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "initialize",
    "params": {
      "protocolVersion": "2025-06-18",
      "capabilities": {},
      "clientInfo": {"name": "test", "version": "1.0"}
    }
  }'
```

Expected response:

```json
{
  "result": {
    "protocolVersion": "2025-06-18",
    "capabilities": { "tools": { "listChanged": true } },
    "serverInfo": { "name": "sfp-pro", "version": "1.0.0" }
  },
  "jsonrpc": "2.0",
  "id": 1
}
```

### Step 3: Test a tool call

```bash
curl -s -X POST https://your-server/sfp/api/mcp \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{
    "jsonrpc": "2.0",
    "id": 2,
    "method": "tools/call",
    "params": {
      "name": "pools",
      "arguments": {
        "action": "list_pools",
        "repositoryIdentifier": "your-org/your-repo"
      }
    }
  }'
```

### MCP Inspector

The [MCP Inspector](https://github.com/modelcontextprotocol/inspector) provides a web UI for testing:

```bash
npx @modelcontextprotocol/inspector \
  --transport streamable-http \
  --url https://your-server/sfp/api/mcp
```

Open `http://localhost:6274` and enter your application token in the **Bearer Token** field.

### Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `401 Unauthorized` | Missing or expired token | Set `Authorization: Bearer <token>` header |
| `Not Acceptable` | Missing Accept header | Include `Accept: application/json, text/event-stream` |
| `Connection refused` | Server not running or port blocked | Check server status with `sfp server status` |
| `isError: true` in response | Tool-level error (e.g., pool not found) | Check the error message; use `list_pools` to discover valid pools |

## Example Workflows

### Implementing a Jira Issue

**User**: "Implement DP-6"

**Agent Actions**:
1.  `work_items` -> `get_work_item(id="DP-6")`
2.  `review_envs` -> `fetch_review_env(assignmentId="DP-6", ...)`
3.  Agent reads code, implements changes, and deploys.

### Pool Discovery

**User**: "What pools are available for flxbl-io/sf-core?"

**Agent Actions**:
1.  `pools` -> `list_pools(repositoryIdentifier="flxbl-io/sf-core")`

## Requirements

- **sfp-pro server**: Running with MCP module enabled.
- **Application Token**: Created via CLI or API.
- **Integrations**: Jira/GitHub configured for work items and pools.

## Related Documentation

- [Integrations](integrations.md)
- [Review Environments](../review-environments/overview.md)
- [Pools](../environment-management/pools/README.md)

# MCP (Model Context Protocol)

Connect your AI tools to sfp using MCP. Claude, GitHub Copilot, Cursor, and OpenCode can fetch Jira work items, manage review environments, and query pool configurations directly from your sfp instance — enabling end-to-end AI-assisted Salesforce development.

## Endpoint

Your MCP endpoint is your sfp instance URL with `/sfp/api/mcp` appended:

```
https://your-company.flxbl.io/sfp/api/mcp
```

| Method | Path | Purpose |
|--------|------|---------|
| `POST` | `/sfp/api/mcp` | JSON-RPC 2.0 — tool calls, initialize |
| `GET` | `/sfp/api/mcp` | SSE stream for notifications |
| `DELETE` | `/sfp/api/mcp` | Close session |
| `GET` | `/sfp/api/mcp/tools` | REST — list available tools |

| Property | Value |
|----------|-------|
| Transport | Streamable HTTP |
| Protocol | MCP 2025-06-18 |
| Auth | `Authorization: Bearer <application-token>` |
| Content-Type | `application/json` |
| Accept | `application/json, text/event-stream` |

## Quick Start

### 1. Create an application token

Generate a dedicated token for your MCP clients:

{% tabs %}
{% tab title="CLI" %}
```bash
# Login to your sfp instance
sfp server auth login

# Create a token (save the output — it is only shown once)
sfp server application-token create --name "Claude Desktop" --expires-in 90
```
{% endtab %}

{% tab title="API" %}
```bash
curl -X POST https://your-company.flxbl.io/sfp/api/application-tokens \
  -H "Authorization: Bearer <your-jwt>" \
  -H "Content-Type: application/json" \
  -d '{"name": "Claude Desktop", "expiresIn": 90}'
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
The token is only displayed once upon creation. Save it immediately in a secret manager or your CI/CD vault.
{% endhint %}

### 2. Configure your AI client

Pick your editor and add the configuration below. Replace the URL with your sfp instance and paste your token.

{% tabs %}
{% tab title="Claude Desktop" %}
Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "sfp": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://your-company.flxbl.io/sfp/api/mcp",
        "--header", "Authorization:Bearer YOUR_APP_TOKEN",
        "--transport", "http-only"
      ]
    }
  }
}
```

{% hint style="info" %}
Claude Desktop requires [mcp-remote](https://www.npmjs.com/package/mcp-remote) to connect to HTTP servers. The `npx -y` command installs it automatically.
{% endhint %}
{% endtab %}

{% tab title="Claude Code (CLI)" %}
```bash
claude mcp add sfp --transport http \
  https://your-company.flxbl.io/sfp/api/mcp \
  --header "Authorization: Bearer YOUR_APP_TOKEN"
```

Or add to `.claude/settings.json`:

```json
{
  "mcpServers": {
    "sfp": {
      "type": "http",
      "url": "https://your-company.flxbl.io/sfp/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_APP_TOKEN"
      }
    }
  }
}
```
{% endtab %}

{% tab title="GitHub Copilot" %}
Add to `.vscode/mcp.json` (per-project) or `~/.vscode/mcp.json` (global):

```json
{
  "mcpServers": {
    "sfp": {
      "type": "http",
      "url": "https://your-company.flxbl.io/sfp/api/mcp",
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
- **URL**: `https://your-company.flxbl.io/sfp/api/mcp`
- **Headers**: `Authorization: Bearer YOUR_APP_TOKEN`

Or add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "sfp": {
      "type": "http",
      "url": "https://your-company.flxbl.io/sfp/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_APP_TOKEN"
      }
    }
  }
}
```
{% endtab %}

{% tab title="OpenCode" %}
Add to `opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "sfp": {
      "type": "remote",
      "url": "https://your-company.flxbl.io/sfp/api/mcp",
      "oauth": false,
      "headers": {
        "Authorization": "Bearer {env:SFP_APP_TOKEN}"
      }
    }
  }
}
```

{% hint style="info" %}
OpenCode supports `{env:VAR}` syntax to read tokens from environment variables, keeping secrets out of config files.
{% endhint %}
{% endtab %}
{% endtabs %}

### 3. Verify

Ask your AI assistant: *"List the available sandbox pools."* If it returns pool data, you're connected.

## Authentication

All MCP requests require a bearer token:

```
Authorization: Bearer <application-token>
```

Application tokens authenticate with the `Application` role. Owner and Member JWT tokens also work. Rotate tokens regularly — use short expiration times (30-90 days) for CI/CD environments.

## Available Tools

### `pools`

List sandbox and scratch org pool configurations.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `action` | Yes | Must be `list_pools` |
| `repositoryIdentifier` | No | Repository in `owner/repo` format |
| `poolType` | No | `SANDBOX` or `SCRATCH_ORG` |
| `hasAssignmentRules` | No | Filter by assignment rules (boolean) |

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

Fetch and release review environments for PRs, issues, or users.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `action` | Yes | `fetch_review_env` or `release_review_env` |
| `assignmentType` | Yes (fetch) | `pr`, `issue`, `user`, or `custom` |
| `assignmentId` | Yes (fetch) | PR number, issue key, or username |
| `repositoryIdentifier` | Yes (fetch) | Repository in `owner/repo` format |
| `poolTag` | No | Specific pool tag |
| `branch` | No | Branch name for pool resolution |
| `domains` | No | Array of domains |
| `domain` | No | Single domain |
| `expirationHours` | No | 1-720 |
| `isImmortal` | No | boolean |
| `metadata` | No | object |
| `override` | No | boolean |

### `work_items`

Get or list work items from your connected issue tracker (Jira, GitHub Issues).

| Parameter | Required | Description |
|-----------|----------|-------------|
| `action` | Yes | `get_work_item` or `list_work_items` |
| `projectIdentifier` | No | `owner/repo` (platform derived from config) |
| `id` | No | Work item ID (for `get_work_item`) |
| `status` | No | Filter by status |
| `assignee` | No | Filter by assignee |
| `type` | No | Filter by type |
| `maxResults` | No | 1-100 (default 50) |

## Debugging

### Verify auth with curl

```bash
# Should return 401
curl -s https://your-company.flxbl.io/sfp/api/mcp/tools

# Should return tool list
curl -s -H "Authorization: Bearer YOUR_TOKEN" \
  https://your-company.flxbl.io/sfp/api/mcp/tools
```

### Test the MCP protocol handshake

```bash
curl -s -X POST https://your-company.flxbl.io/sfp/api/mcp \
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

### MCP Inspector

The [MCP Inspector](https://github.com/modelcontextprotocol/inspector) provides a visual interface for testing tools:

```bash
npx @modelcontextprotocol/inspector \
  --transport streamable-http \
  --url https://your-company.flxbl.io/sfp/api/mcp
```

Open `http://localhost:6274` and enter your application token in the **Bearer Token** field.

### Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `401 Unauthorized` | Missing or expired token | Create a new token with `sfp server application-token create` |
| `Not Acceptable` | Missing Accept header | Include `Accept: application/json, text/event-stream` (both required) |
| `Connection refused` | Instance unreachable from your network | Verify the URL and check that your network can reach your sfp instance |
| `isError: true` in tool response | Tool-level error (e.g., pool not found) | Check the error message; call `list_pools` to discover valid pools |

## Example Workflows

### Implementing a Jira Issue

**You**: *"Implement DP-6"*

**Agent**:
1. `work_items` → `get_work_item(id="DP-6")` — reads acceptance criteria
2. `review_envs` → `fetch_review_env(assignmentId="DP-6", ...)` — gets a sandbox
3. Implements changes, deploys, and validates

### Pool Discovery

**You**: *"What pools are available for flxbl-io/sf-core?"*

**Agent**:
1. `pools` → `list_pools(repositoryIdentifier="flxbl-io/sf-core")`

## Related

- [Review Environments](../review-environments/overview.md)
- [Pools](../environment-management/pools/README.md)

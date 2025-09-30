---
icon: ring-diamond
---

# CI/CD Integration

The project analysis command integrates seamlessly with various CI/CD platforms to provide automated code quality checks and visual feedback through GitHub Checks.

## Automatic Detection

### GitHub Actions (Default)

When running in GitHub Actions, everything works automatically because GitHub Actions provides built-in access to GitHub App tokens:

```yaml
# .github/workflows/pr-analysis.yml
name: PR Analysis

on: pull_request

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Run Project Analysis
        run: sfp project:analyze
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Note**: The `GITHUB_TOKEN` provided by GitHub Actions has the necessary permissions to create checks. This is why it works automatically in GitHub Actions but requires special setup in other CI platforms (see below).

The command automatically:
- ✅ Detects it's running in a PR context
- ✅ Fetches changed files from the PR
- ✅ Creates GitHub Checks with results
- ✅ Adds annotations to files with issues

## Manual Configuration (Other CI Platforms)

If you're using a different CI platform but want to push checks to GitHub, you need to manually provide environment variables.

### Required Environment Variables

#### For PR Context Detection

| Variable | Description | Example |
|----------|-------------|---------|
| `GITHUB_ACTIONS` | Set to `"true"` to enable GitHub mode | `true` |
| `GITHUB_EVENT_NAME` | Event type that triggered the workflow | `pull_request` |
| `GITHUB_REPOSITORY` | Repository in format `owner/repo` | `owner/repo` |
| `GITHUB_EVENT_PATH` | Path to file containing PR event data | `/tmp/pr-event.json` |

#### For Check Creation

| Variable | Description | Example |
|----------|-------------|---------|
| `GITHUB_SHA` | Commit SHA to attach check to | `abc123...` |
| `GITHUB_TOKEN` | GitHub token with `checks:write` permission | `ghs_xxxxx` |
| `GITHUB_RUN_ID` | Optional: Run ID for details URL | `12345` |

#### For Git Diff Operations

| Variable/Flag | Description | Example |
|---------------|-------------|---------|
| `--base-ref` | Base commit/branch for comparison | `main` or `abc123...` |
| `--head-ref` | Head commit/branch for comparison | `HEAD` or `def456...` |

### Event Data Format

The `GITHUB_EVENT_PATH` should point to a JSON file with this structure:

```json
{
  "number": 123,
  "pull_request": {
    "number": 123,
    "base": {
      "sha": "base-commit-sha"
    },
    "head": {
      "sha": "head-commit-sha"
    }
  }
}
```

## Authentication

**IMPORTANT**: Creating GitHub Checks requires a GitHub App installation token, not a regular `GITHUB_TOKEN`.

### Using sfp server (Recommended)

If you have sfp server installed, use it to generate installation tokens:

```bash
# Get installation token for your repository
sfp server repository auth-token \
  --repository owner/repo \
  --sfp-server-url https://your-server-url \
  --email your-email@example.com \
  --json

# Use the returned token
export GITHUB_TOKEN=<token-from-above>
```

The token from sfp server has the required GitHub App permissions:
- `checks:write` - Create/update checks
- `contents:read` - Read repository contents
- `pull_requests:read` - Read PR information

### Without sfp server

If you don't have sfp server, you need to:
1. Create your own GitHub App with the permissions listed above
2. Install it on your repository/organization
3. Generate an installation token using your own tooling
4. Set `GITHUB_TOKEN` to that installation token

**Note**: Regular GitHub Actions `GITHUB_TOKEN` or Personal Access Tokens will NOT work for creating checks.

## Testing Your Configuration

Test your configuration locally:

```bash
# Step 1: Get GitHub App installation token from sfp server
GITHUB_TOKEN=$(sfp server repository auth-token \
  --repository owner/repo \
  --sfp-server-url https://your-server-url \
  --email your-email@example.com \
  --json | jq -r '.token')

# Step 2: Set environment variables
export GITHUB_TOKEN
export GITHUB_ACTIONS=true
export GITHUB_EVENT_NAME=pull_request
export GITHUB_REPOSITORY=owner/repo
export GITHUB_SHA=your-commit-sha
export GITHUB_EVENT_PATH=/tmp/pr-event.json

# Step 3: Create event file
cat > /tmp/pr-event.json <<EOF
{
  "number": 123,
  "pull_request": {
    "number": 123,
    "base": { "sha": "base-sha" },
    "head": { "sha": "head-sha" }
  }
}
EOF

# Step 4: Run analysis
sfp project:analyze \
    --base-ref base-sha \
    --head-ref head-sha
```

Expected output:
```
✅ Detected PR context #123
✅ Retrieved X changed files from PR #123
✅ Creating check for [linter]...
✅ Successfully created GitHub check: https://github.com/owner/repo/pull/123/checks
```

## Troubleshooting

### No PR Context Detected

```
Skipping check creation - not supported in this environment
```

**Solution**: Verify `GITHUB_ACTIONS=true` and `GITHUB_EVENT_NAME=pull_request` are set.

### Missing GitHub Context

```
Cannot create GitHub check: Missing GitHub context information
```

**Solution**: Ensure `GITHUB_REPOSITORY`, `GITHUB_SHA`, and `GITHUB_EVENT_PATH` are set.

### Authentication Failed

```
Failed to get auth token: Neither App credentials nor personal access token found
```

**Solution**: Set `GITHUB_TOKEN` environment variable with a valid token.

### Wrong Line Counts

```
Changes: +0 -0 lines
```

**Solution**: Provide correct `--base-ref` and `--head-ref` flags. In PR contexts, use the actual base/head SHAs, not just `HEAD`.

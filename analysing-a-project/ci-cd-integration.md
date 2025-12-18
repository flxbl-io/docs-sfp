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

## Other CI Platforms

If you're using a CI platform other than GitHub Actions, you can still create GitHub Checks by setting the required environment variables.

### Required Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GITHUB_ACTIONS` | Yes | Set to `"true"` to enable GitHub Check creation |
| `GITHUB_REPOSITORY` | Yes | Repository in `owner/repo` format |
| `GITHUB_SHA` | Yes | The commit SHA to attach the check to (use PR head SHA) |
| `GITHUB_EVENT_NAME` | Yes | Set to `"pull_request"` for PR context |
| `GITHUB_EVENT_PATH` | Yes | Path to JSON file containing PR event data |
| `GITHUB_TOKEN` | Yes | GitHub App installation token (see Authentication below) |
| `GITHUB_RUN_ID` | No | Your CI build/run ID (used for details URL) |

### PR Event Data File

Create a JSON file at the path specified by `GITHUB_EVENT_PATH`:

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

### Command Line Flags

For accurate diff detection, pass these flags:

```bash
sfp project:analyze \
    --base-ref <base-branch-or-sha> \
    --head-ref <head-branch-or-sha>
```

| Flag | Description |
|------|-------------|
| `--base-ref` | Base commit/branch for comparison (PR target) |
| `--head-ref` | Head commit/branch for comparison (PR source) |

## Authentication

Creating GitHub Checks requires a **GitHub App installation token**. Personal Access Tokens (PATs) cannot create checks.

Use sfp server to generate installation tokens:

```bash
sfp server repository auth-token \
  --repository owner/repo \
  --sfp-server-url https://your-server-url \
  --email your-email@example.com \
  --json
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

u# Auto-Enable Trace Logging in CI/CD
*Available from October 2025*

sfp automatically enables trace logging when CI/CD debug mode is detected.

## Supported CI Platforms
- **GitHub Actions**: `RUNNER_DEBUG=1`
- **GitLab CI**: `CI_DEBUG_TRACE=1`
- **Azure DevOps**: `SYSTEM_DEBUG=true`
- **Buildkite**: `BUILDKITE_DEBUG=1`

## Manual Override
```bash
SFP_DEBUG=1 sfp build
```
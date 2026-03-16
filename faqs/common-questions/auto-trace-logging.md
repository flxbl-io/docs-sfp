---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/faqs/common-questions/auto-trace-logging
---

# Auto-Enable Trace Logging in CI/CD

u# Auto-Enable Trace Logging in CI/CD _Available from October 2025_

sfp automatically enables trace logging when CI/CD debug mode is detected.

### Supported CI Platforms

* **GitHub Actions**: `RUNNER_DEBUG=1`
* **GitLab CI**: `CI_DEBUG_TRACE=1`
* **Azure DevOps**: `SYSTEM_DEBUG=true`
* **Buildkite**: `BUILDKITE_DEBUG=1`

### Manual Override

```bash
SFP_DEBUG=1 sfp build
```

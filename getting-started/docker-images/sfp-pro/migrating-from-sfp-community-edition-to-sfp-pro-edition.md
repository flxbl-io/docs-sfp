---
icon: ring-diamond
---

# Migrating from sfp community edition to sfp pro edition

&#x20;**Breaking Changes: sfp community edition to sfp-pro**

| Command                                   | Must Work | Notes                        |
| ----------------------------------------- | --------- | ---------------------------- |
| `sfp build` (alias: `orchestrator:build`) | ✅         | Pro adds `--commit` flag     |
| `sfp install`                             | ✅         | Core functionality unchanged |
| `sfp publish`                             | ✅         | Core functionality unchanged |
| `sfp release`                             | ✅         | Core functionality unchanged |

**Critical: Update Your Pipelines**

Most `orchestrator:` command aliases have been removed in sfp-pro. You must update your pipelines:

| Old Command (sfp-comm)        | New Command (sfp-pro) |
| ----------------------------- | --------------------- |
| `sfp orchestrator:build`      | `sfp build`           |
| `sfp orchestrator:deploy`     | `sfp install`         |
| `sfp orchestrator:release`    | `sfp release`         |
| `sfp orchestrator:validate`   | `sfp validate`        |
| `sfp orchestrator:quickbuild` | `sfp quickbuild`      |

**New/Changed in sfp-pro:**

**Exception:** `sfp orchestrator:publish` still works in sfp-pro.

* **`--commit` flag** added to `build` command for specifying commit hash
* **Server flags** added to most commands (`--server-url`, `--api-key`)
* **Additional commands** in pro: `server:*`, `dev:*`, `project:*`, `sandbox:*`
* **Orchestrator alias**: Both versions support `sfp orchestrator:build` as alias for `sfp build`

**✅ What Stays the Same**

* Core command functionality is unchanged
* All flags from sfp-comm still work
* Command outputs remain compatible

**Migration Notes:**

**🆕 New in sfp-pro**

* All core orchestrator commands (`build`, `install`, `publish`, `release`) work identically
* Existing pipelines using these commands will continue to work
* New flags are optional and backward compatible
* `--commit` flag on `build` command
* Server mode with `--server-url` and `--api-key` flags
* Additional commands: `server:*`, `dev:*`, `project:*`, `sandbox:*`, `config:*`

---
icon: ring-diamond
---

# Project

A **project** in sfp  represents a registered repository (e.g., `flxbl-io/sf-core`). Projects are the foundation for:

* Tracking builds and deployments
* Scoping integrations to specific repositories
* Managing team access and permissions

Before creating project-scoped integrations, you must first register your project:

```bash
# Register a project via CLI
sfp server project register --identifier "your-org/your-repo" --remote-url "https://github.com/your-org/your-repo"
```

Or via API:

```bash
curl -X POST https://your-server/sfp/api/projects \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "identifier": "your-org/your-repo",
    "remoteUrl": "https://github.com/your-org/your-repo"
  }'
```

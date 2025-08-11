# Server

Commands for administrators to manage sfp server

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅          | ❌               |
| From         | August 25   |                 |

## Core Commands

* [`sfp server init`](init.md) - Initialize a new SFP server instance
* [`sfp server start`](start.md) - Start a tenant's services
* [`sfp server stop`](stop.md) - Stop a tenant's services
* [`sfp server status`](status.md) - Check the status of a tenant's services
* [`sfp server update`](update.md) - Update a tenant's services to the latest version
* [`sfp server health`](health.md) - Check the health and connectivity of the remote SFP server

## Monitoring & Operations

* [`sfp server logs`](logs.md) - View logs for a tenant's services
* [`sfp server scale`](scale.md) - Scale worker services for a tenant

## Management Topics

* [`sfp server application-token`](application-token.md) - Manage application tokens in sfp server
* [`sfp server artifacts`](artifacts.md) - Promote unlocked packages with metadata publishing to SFP Server (experimental)
* [`sfp server auth`](auth.md) - Allows to authenticate to a sfp server
* [`sfp server builds`](builds.md) - List recent builds from the SFP server sorted by commit time
* [`sfp server doc-store`](doc-store.md) - Manage document store in sfp server
* [`sfp server environment`](environment.md) - Manage environments in sfp server
* [`sfp server key-value`](key-value.md) - Manage key-value store in sfp server
* [`sfp server org`](org.md) - Manage salesforce orgs in sfp server
* [`sfp server pool`](pool.md) - Manage scratch org and sandbox pools in sfp server
* [`sfp server project`](project.md) - Manage projects in sfp server
* [`sfp server releasedefinition`](releasedefinition.md) - Generate a release definition based on artifacts using git tags and publish to server
* [`sfp server repository`](repository.md) - Manage repository authentication in sfp server
* [`sfp server review-envs`](review-envs.md) - Manage review environments in sfp server
* [`sfp server user`](user.md) - Manage users in sfp server
* [`sfp server webhook`](webhook.md) - Manage webhooks in sfp server
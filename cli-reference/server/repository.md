# Repository

## `sfp server repository`

Manage repository authentication in sfp server

### Description

The repository commands provide functionality for managing Git repository connections and authentication for the SFP server.

### Available Commands

* Add repository authentication
* List configured repositories
* Update repository credentials
* Test repository connectivity
* Remove repository configurations

### Supported Providers

- GitHub
- GitLab
- Bitbucket
- Azure DevOps
- Generic Git repositories

### Authentication Methods

- Personal Access Tokens (PAT)
- OAuth tokens
- SSH keys
- App installations (GitHub Apps)

### Common Use Cases

- Configure repository access for automated builds
- Manage repository webhooks
- Track repository activity
- Rotate access credentials

> **Note**: Detailed command documentation coming soon.

> **Security**: Repository credentials are encrypted and stored securely. Use tokens with minimal required permissions.
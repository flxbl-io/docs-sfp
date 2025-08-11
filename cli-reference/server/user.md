# User

## `sfp server user`

Manage users in sfp server

### Description

The user commands provide functionality for managing user accounts, permissions, and access control within the SFP server.

### Available Commands

* Create user accounts
* List users
* Update user information
* Manage user roles and permissions
* Deactivate/activate users
* Delete user accounts

### User Roles

- **Owner**: Full administrative access
- **Admin**: Manage projects and environments
- **Developer**: Deploy and manage packages
- **Viewer**: Read-only access

### Authentication Integration

Users can authenticate via:
- Email/password
- OAuth providers (Google, GitHub, etc.)
- SAML/SSO
- Application tokens

### Common Use Cases

- Onboard team members
- Manage access permissions
- Audit user activity
- Rotate credentials
- Enforce security policies

> **Note**: Detailed command documentation coming soon.

> **Security**: Follow the principle of least privilege when assigning user roles.
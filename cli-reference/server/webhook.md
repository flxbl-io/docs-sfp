# Webhook

## `sfp server webhook`

Manage webhooks in sfp server

### Description

The webhook commands provide functionality for configuring and managing webhooks that notify external systems about events occurring within the SFP server.

### Available Commands

* Create webhooks
* List configured webhooks
* Update webhook configuration
* Test webhook connectivity
* Enable/disable webhooks
* Delete webhooks

### Supported Events

- Build started/completed/failed
- Deployment started/completed/failed
- Release created/deployed
- Environment locked/unlocked
- Pool status changes
- Error notifications

### Webhook Formats

- Generic JSON
- Slack
- Microsoft Teams
- Discord
- Custom templates

### Common Use Cases

- Send notifications to Slack/Teams
- Trigger external CI/CD pipelines
- Update external tracking systems
- Audit logging to external systems
- Real-time monitoring and alerting

### Example Webhook Payload

```json
{
  "event": "deployment.completed",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "environment": "production",
    "release": "v2.3.0",
    "status": "success",
    "duration": 932,
    "packages": ["core-1.2.3", "ui-2.0.1"]
  }
}
```

> **Note**: Detailed command documentation coming soon.

> **Security**: Webhook URLs should use HTTPS and include authentication tokens in headers when possible.
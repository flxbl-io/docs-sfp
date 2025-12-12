# Configuring Collectors

sfp supports multiple metrics collectors to send build, deploy, validate, and release metrics to your observability platform of choice.

## Available Collectors

| Collector                          | Description                                                  | sfp-pro | sfp (community) |
| ---------------------------------- | ------------------------------------------------------------ | ------- | --------------- |
| [sfp Server](sfp-server.md)        | Built-in Victoria Metrics with query CLI                     | ✅       | ❌               |
| [Datadog](datadog.md)              | Native integration with Datadog                              | ✅       | ✅               |
| [Splunk](splunk.md)                | Native integration with Splunk                               | ✅       | ✅               |
| [New Relic](new-relic.md)          | Native integration with New Relic                            | ✅       | ✅               |
| [StatsD](statsd.md)                | Forward to any StatsD-compatible collector                   | ✅       | ✅               |

## Multiple Collectors

sfp can send metrics to multiple collectors simultaneously. Simply configure the environment variables for each collector you want to use:

```bash
# Send metrics to both sfp server and Datadog
export SFP_SERVER_URL=http://your-sfp-server:3029
export SFPOWERSCRIPTS_DATADOG=true
export SFPOWERSCRIPTS_DATADOG_API_KEY=your-api-key

sfp build  # Metrics sent to both destinations
```

## Default Behavior

### sfp Server (Default for sfp-pro)

When `SFP_SERVER_URL` is configured, all sfp commands automatically emit metrics to the sfp server. No additional configuration is required - simply set the server URL and authenticate:

```bash
sfp config set server-url http://your-sfp-server:3029
sfp config set server-email your-email@example.com
sfp server auth login
```

Once configured, any command (`build`, `deploy`, `release`, `validate`, etc.) will automatically send metrics to the server.

### External Collectors (Datadog, Splunk, New Relic, StatsD)

Use external collectors when you need to:
- Integrate with an existing observability platform
- Correlate sfp metrics with other system metrics
- Use advanced alerting and dashboard features
- Meet compliance requirements for specific platforms

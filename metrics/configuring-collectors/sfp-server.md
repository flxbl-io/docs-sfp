---
description: >-
  Configure sfp to send metrics to sfp server with built-in Victoria Metrics for
  centralized metrics storage and querying.
icon: ring-diamond
---

# sfp Server

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | January 26  |                 |

sfp-pro includes a built-in metrics collector powered by Victoria Metrics. This is the default metrics destination for sfp-pro users - once configured, all sfp commands automatically emit metrics to the server.

## Overview

When `SFP_SERVER_URL` is configured, all sfp commands (`build`, `deploy`, `release`, `validate`, etc.) automatically send metrics to your sfp server instance where they are stored in Victoria Metrics. No additional flags or configuration is needed.

You can then query and visualize these metrics using:

* The `sfp metrics:query` command for ad-hoc queries
* The `sfp metrics:display` command for summary dashboards
* Direct Victoria Metrics API access for advanced use cases

## Prerequisites

* sfp server running (see [Server Setup](../../cli-reference/server/README.md))
* Server authentication configured (see [Server Authentication](../../auth-management/server-authentication.md))

## Configuration

### Using sfp config

Set the server URL and authentication credentials globally:

```bash
# Set server URL
sfp config set server-url http://your-sfp-server:3029

# Set your email for authentication
sfp config set server-email your-email@example.com

# Authenticate with the server
sfp server auth login
```

### Using Environment Variables

Alternatively, set environment variables in your CI/CD pipeline:

```bash
export SFP_SERVER_URL=http://your-sfp-server:3029
export SFP_SERVER_TOKEN=your-application-token
```

### Using Flags

Pass credentials directly to commands:

```bash
sfp build --sfp-server-url http://your-sfp-server:3029 --email your-email@example.com
```

## How It Works

Once `SFP_SERVER_URL` is set, metrics flow automatically:

1. **Automatic Collection**: Every sfp command emits metrics during execution
2. **Batching**: Metrics are batched for efficient network usage
3. **Ingestion**: Batched metrics are sent to the server's `/sfp/api/metrics/ingest` endpoint
4. **Storage**: sfp server stores metrics in Victoria Metrics
5. **Querying**: Use `metrics:query` or `metrics:display` to retrieve data

No changes to your existing workflows are required - just configure the server URL and metrics will start flowing.

## Authentication Requirements

### Metrics Ingestion

Metrics ingestion requires an **application token** (used by CI/CD pipelines). When authenticated with a user token, the server silently accepts the request but does not store the metrics. This design ensures:

* CI/CD pipelines with application tokens can emit metrics
* Local development with user tokens doesn't fail or require special handling
* Metrics data comes only from automated pipelines, ensuring consistency

To create an application token for your CI/CD pipeline, see [Application Tokens](../../auth-management/application-tokens.md).

### Metrics Querying

Both user tokens and application tokens can query metrics using `metrics:query` and `metrics:display` commands.

## Querying Metrics

### Using metrics:query

Query metrics using simple names or MetricsQL expressions:

```bash
# Query by metric name (auto-prefixes with sfpowerscripts.)
sfp metrics:query -n build.duration --range 1h

# Query with filters
sfp metrics:query -n package.created --package salescore --range 24h

# Query with wildcards
sfp metrics:query -n "build.*" --range 7d

# Advanced MetricsQL query
sfp metrics:query -q "sum(sfpowerscripts.build.succeeded.packages)" --range 30d
```

### Using metrics:display

View a summary dashboard of key metrics:

```bash
# Display all categories
sfp metrics:display

# Display specific category
sfp metrics:display --category build

# Display with custom time range
sfp metrics:display --category deploy --days 7
```

### Available Range Options

| Range | Description   |
| ----- | ------------- |
| `5m`  | 5 minutes     |
| `15m` | 15 minutes    |
| `30m` | 30 minutes    |
| `1h`  | 1 hour        |
| `3h`  | 3 hours       |
| `6h`  | 6 hours       |
| `12h` | 12 hours      |
| `24h` | 24 hours      |
| `2d`  | 2 days        |
| `7d`  | 7 days        |
| `30d` | 30 days       |

## Metric Categories

The display command provides summaries for these categories:

| Category   | Description                          |
| ---------- | ------------------------------------ |
| `build`    | Build operations and package counts  |
| `deploy`   | Deployment success rates and timing  |
| `validate` | Validation execution statistics      |
| `release`  | Release operations and package stats |
| `prepare`  | Pool preparation metrics             |

## JSON Output

Both commands support JSON output for programmatic access:

```bash
# Query with JSON output
sfp metrics:query -n build.duration --json

# Display with JSON output
sfp metrics:display --category all --json
```

## Combining with Other Collectors

sfp server metrics can be used alongside other collectors. When `SFP_SERVER_URL` is configured, metrics are sent to the server in addition to any other configured destinations (DataDog, NewRelic, etc.).

```bash
# Metrics sent to both sfp server AND DataDog
export SFP_SERVER_URL=http://your-sfp-server:3029
export SFPOWERSCRIPTS_DATADOG=true
export SFPOWERSCRIPTS_DATADOG_API_KEY=your-api-key

sfp build  # Metrics go to both destinations
```

## Troubleshooting

### No Metrics Appearing

1. **Verify you're using an application token**: User tokens are silently accepted but metrics are not stored. CI/CD pipelines must use application tokens for metrics ingestion.
2. Verify server is running: `sfp server status`
3. Check authentication: `sfp server auth whoami`
4. Ensure server URL is correct: `sfp config list | grep server-url`

### Query Returns No Data

1. Verify the time range covers when metrics were sent
2. Check metric name spelling (use `sfp metrics:query --label __name__` to list all metrics)
3. Ensure the metric prefix is correct (`sfpowerscripts.` is auto-added for `-n` flag)

### Connection Errors

1. Check network connectivity to the server
2. Verify firewall rules allow traffic on port 3029
3. Check server logs: `sfp server logs`

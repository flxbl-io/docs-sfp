---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/cli-reference/advanced/metrics
---

# Metrics

## `sfp metrics`

Commands for reporting and querying metrics from sfp supported metric providers.

* `sfp metrics report`
* `sfp metrics query`
* `sfp metrics display`

***

## `sfp metrics report`

Report a custom metric to any sfp supported metric provider.

```
USAGE
  $ sfp metrics report -m <value> -t gauge|counter|timer [-v <value>] [-g <value>]
    [--sfp-server-url <value>] [-e <value>] [--application-token <value>]
    [--loglevel trace|debug|info|warn|error|fatal]

FLAGS
  -g, --tags=<value>              Tags for metric (JSON format)
  -m, --metric=<value>            (required) Metric name to publish
  -t, --type=<option>             (required) Type of metric
                                  <options: gauge|counter|timer>
  -v, --value=<value>             Value of metric
  -e, --email=<value>             Email for server authentication
      --sfp-server-url=<value>    URL of sfp server
      --application-token=<value> Application token for server authentication
      --loglevel=<option>         [default: info] Logging level
                                  <options: trace|debug|info|warn|error|fatal>

DESCRIPTION
  Report a custom metric to any sfp supported metric provider. Supports StatsD, DataDog,
  NewRelic, Splunk, and sfp server.

EXAMPLES
  $ sfp metrics:report -m <metric> -t <type> -v <value>

  $ sfp metrics:report -m build.duration -t timer -v 12000 --sfp-server-url http://localhost:3029 -e dev@flxbl.io

  $ sfp metrics:report -m custom.counter -t counter --tags '{"team":"platform"}'
```

***

## `sfp metrics query`

Query metrics from sfp server using simple metric names or MetricsQL expressions.

```
USAGE
  $ sfp metrics query [-n <value> | -q <value> | -l <value>] [-r <value>]
    [--package <value>] [--org <value>] [--branch <value>]
    [--start <value>] [--end <value>] [--step <value>]
    [--sfp-server-url <value>] [-e <value>] [--application-token <value>]
    [--json] [--loglevel trace|debug|info|warn|error|fatal]

FLAGS
  -n, --name=<value>              Metric name to query (e.g., build.duration, package.created)
                                  Supports wildcards like "build.*"
  -q, --query=<value>             Raw MetricsQL/PromQL query expression for advanced queries
  -l, --label=<value>             Get distinct values for a label (e.g., __name__, package)
  -r, --range=<value>             [default: 1h] Time range to query
                                  Options: 5m, 15m, 30m, 1h, 3h, 6h, 12h, 24h, 2d, 7d, 30d
      --package=<value>           Filter by package name (used with --name)
      --org=<value>               Filter by target org (used with --name)
      --branch=<value>            Filter by branch name (used with --name)
      --start=<value>             Range start (RFC3339 or unix seconds). Overrides --range
      --end=<value>               Range end (RFC3339 or unix seconds). Overrides --range
      --step=<value>              Step/resolution for range queries (e.g., 30s, 1m, 1h)
  -e, --email=<value>             Email for server authentication
      --sfp-server-url=<value>    URL of sfp server
      --application-token=<value> Application token for server authentication
      --json                      Format output as JSON
      --loglevel=<option>         [default: info] Logging level
                                  <options: trace|debug|info|warn|error|fatal>

DESCRIPTION
  Query metrics stored in sfp server's Victoria Metrics database. Use simple metric names
  with -n flag (auto-prefixes with sfpowerscripts.) or raw MetricsQL with -q flag.

EXAMPLES
  # Query build duration for the last hour
  $ sfp metrics:query -n build.duration

  # Query with wildcard and extended range
  $ sfp metrics:query -n "build.*" --range 24h

  # Query with package filter
  $ sfp metrics:query -n package.created --package salescore --range 7d

  # Advanced MetricsQL query with time range
  $ sfp metrics:query -q "sum(sfpowerscripts_build_completed)" --start 2024-01-01T00:00:00Z --end 2024-01-02T00:00:00Z --step 1h

  # List all available metric names
  $ sfp metrics:query --label __name__

  # JSON output for scripting
  $ sfp metrics:query -n build.duration --json
```

### Query Types

#### Simple Name Query (`-n`)

Use for straightforward metric queries. The `sfpowerscripts.` prefix is automatically added:

```bash
sfp metrics:query -n build.duration      # Queries sfpowerscripts.build.duration
sfp metrics:query -n "deploy.*"          # Queries all deploy metrics
```

#### Raw Query (`-q`)

Use for advanced MetricsQL expressions:

```bash
sfp metrics:query -q "sum(sfpowerscripts.build.succeeded.packages) by (branch)"
sfp metrics:query -q "rate(sfpowerscripts.deploy.failed[1h])"
```

#### Label Query (`-l`)

List distinct values for a label:

```bash
sfp metrics:query --label __name__       # List all metric names
sfp metrics:query --label package        # List all package names
sfp metrics:query --label branch         # List all branch names
```

***

## `sfp metrics display`

Display a summary dashboard of key metrics organized by category.

```
USAGE
  $ sfp metrics display [-c <value>] [-d <value>]
    [--sfp-server-url <value>] [-e <value>] [--application-token <value>]
    [--json] [--loglevel trace|debug|info|warn|error|fatal]

FLAGS
  -c, --category=<option>         [default: all] Metric category to display
                                  <options: build|deploy|validate|release|prepare|all>
  -d, --days=<value>              [default: 30] Number of days to query (1-365)
  -e, --email=<value>             Email for server authentication
      --sfp-server-url=<value>    URL of sfp server
      --application-token=<value> Application token for server authentication
      --json                      Format output as JSON
      --loglevel=<option>         [default: info] Logging level
                                  <options: trace|debug|info|warn|error|fatal>

DESCRIPTION
  Display a summary dashboard of sfp metrics organized by category. Shows counts,
  durations, and calculates success rates for build, deploy, validate, release,
  and prepare operations.

EXAMPLES
  # Display all categories for last 30 days
  $ sfp metrics:display

  # Display build metrics only
  $ sfp metrics:display --category build

  # Display deploy metrics for last 7 days
  $ sfp metrics:display --category deploy --days 7

  # Display all metrics for last 90 days as JSON
  $ sfp metrics:display --category all --days 90 --json
```

### Categories

| Category   | Metrics Shown                                                           |
| ---------- | ----------------------------------------------------------------------- |
| `build`    | Builds Scheduled, Packages Built, Packages Failed, Avg Duration         |
| `deploy`   | Deployments, Successful/Failed Deploys, Packages Deployed, Avg Duration |
| `validate` | Validations, Successful/Failed Validations, Avg Duration                |
| `release`  | Releases, Successful/Failed Releases, Packages Released, Avg Duration   |
| `prepare`  | Orgs Succeeded, Orgs Failed, Avg Duration                               |

### Sample Output

```
Metrics Summary (Last 30 days: 11/12/2024 - 12/12/2024)

🔨 Build Metrics

┌──────────────────────────────┬─────────────────────────┐
│ Metric                       │ Value                   │
├──────────────────────────────┼─────────────────────────┤
│ Builds Scheduled             │ 156                     │
│ Packages Built               │ 1,234                   │
│ Packages Failed              │ 12                      │
│ Avg Build Duration           │ 4.2 min                 │
└──────────────────────────────┴─────────────────────────┘

🚀 Deploy Metrics

┌──────────────────────────────┬─────────────────────────┐
│ Metric                       │ Value                   │
├──────────────────────────────┼─────────────────────────┤
│ Deployments                  │ 89                      │
│ Successful Deploys           │ 85                      │
│ Failed Deploys               │ 4                       │
│ Packages Deployed            │ 678                     │
│ Avg Deploy Duration          │ 12.3 min                │
└──────────────────────────────┴─────────────────────────┘

📊 Quick Stats

  • Build Success Rate: 99.0%
  • Deploy Success Rate: 95.5%
```

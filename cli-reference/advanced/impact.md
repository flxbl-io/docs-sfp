# Impact

## `@flxbl-io/sfp impact`

Utilities to help you understand the impact of various components in sfp

* `@flxbl-io/sfp impact package`
* `@flxbl-io/sfp impact releaseconfig`

### `@flxbl-io/sfp impact package`

Figures out impacted packages of a project, due to a change from the last known tags

```
USAGE
  $ @flxbl-io/sfp impact package --basebranch <value> [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  --basebranch=<value>  (required) The base branch on which the git tags should be used
  --loglevel=<option>   [default: info] logging level for this command invocation
                        <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Figures out impacted packages of a project, due to a change from the last known tags
```

_See code:_ [_src/commands/impact/package.ts_](https://github.com/flxbl-io/sfp/blob/v37.0.1/src/commands/impact/package.ts)

### `@flxbl-io/sfp impact releaseconfig`

Figures out impacted release configurations of a project, due to a change,from the last known tags

```
USAGE
  $ @flxbl-io/sfp impact releaseconfig --basebranch <value> [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL] [--branch <value>] [--releaseconfig <value>]
    [--explicitDependencyCheck] [--filterBy <value>] [--filterByChangesInBranch]

FLAGS
  --basebranch=<value>       (required) The base branch on which the git tags should be used from
  --branch=<value>           The branch on which the comparison is carried out
  --explicitDependencyCheck  Activate to consider dependencyOn attribut while handling impact
  --filterBy=<value>         Filter by a specific release config name
  --filterByChangesInBranch  Filter packages by changes with the provided branches as opposed to tags
  --loglevel=<option>        [default: info] logging level for this command invocation
                             <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>
  --releaseconfig=<value>    [default: config] Path to the directory containing release defns

DESCRIPTION
  Figures out impacted release configurations of a project, due to a change,from the last known tags
```

_See code:_ [_src/commands/impact/releaseconfig.ts_](https://github.com/flxbl-io/sfp/blob/v37.0.1/src/commands/impact/releaseconfig.ts)

# Flow

## `@flxblio/sfp flow`

Manage flows in your org

* `@flxblio/sfp flow activate`
* `@flxblio/sfp flow cleanup`
* `@flxblio/sfp flow deactivate`

### `@flxblio/sfp flow activate`

Activate the flow on a target org

```
USAGE
  $ @flxblio/sfp flow activate -u <value> [-f <value>] [-p <value>] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -f, --developername=<value>    The developer name of the flow
  -p, --namespaceprefix=<value>  Use to specify a specific namespace prefix
  -u, --targetorg=<value>        (required) Username or alias of the target org.
      --loglevel=<option>        [default: info] logging level for this command invocation
                                 <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Activate the flow on a target org
```

_See code:_ [_src/commands/flow/activate.ts_](https://github.com/flxbl-io/sfp)

### `@flxblio/sfp flow cleanup`

Cleanup inactive flows on a target org

```
USAGE
  $ @flxblio/sfp flow cleanup -u <value> [-f <value>] [-p <value>] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -f, --developername=<value>    The developer name of the flow
  -p, --namespaceprefix=<value>  Use to specify a specific namespace prefix
  -u, --targetorg=<value>        (required) Username or alias of the target org.
      --loglevel=<option>        [default: info] logging level for this command invocation
                                 <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Cleanup inactive flows on a target org
```

_See code:_ [_src/commands/flow/cleanup.ts_](https://github.com/flxbl-io/sfp)

### `@flxblio/sfp flow deactivate`

Deactivate the flow on a target org

```
USAGE
  $ @flxblio/sfp flow deactivate -u <value> [-f <value>] [-p <value>] [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL]

FLAGS
  -f, --developername=<value>    The developer name of the flow
  -p, --namespaceprefix=<value>  Use to specify a specific namespace prefix
  -u, --targetorg=<value>        (required) Username or alias of the target org.
      --loglevel=<option>        [default: info] logging level for this command invocation
                                 <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>

DESCRIPTION
  Deactivate the flow on a target org
```

_See code:_ [_src/commands/flow/deactivate.ts_](https://github.com/flxbl-io/sfp)

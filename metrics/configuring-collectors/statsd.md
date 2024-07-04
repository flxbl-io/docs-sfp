# StatsD

> statsD is a  network daemon that runs on the [Node.js](http://nodejs.org/) platform and listens for statistics, like counters and timers, sent over [UDP](http://en.wikipedia.org/wiki/User\_Datagram\_Protocol) or [TCP](http://en.wikipedia.org/wiki/Transmission\_Control\_Protocol) and sends aggregates to one or more pluggable backend services (e.g., [Graphite](http://graphite.readthedocs.org/)).\
> [https://github.com/statsd/statsd](https://github.com/statsd/statsd)

sfp supports emitting events to a statsD collector. You can activate the same by enabling the following environment variables

| Environment Variable             | Type    |                                                                                                                                               |
| -------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| SFPOWERSCRIPTS\_STATSD           | boolean | Activate sending metrics to a StatsD collector (such as a Datadog Agent) by setting this key to true                                          |
| SFPOWERSCRIPTS\_STATSD\_PORT     | string  | The port to send stats to, if not set,  it is automatically set to 8125                                                                       |
| SFPOWERSCRIPTS\_STATSD\_HOST     | string  | The host to send stats to, if not set, it is send to 127.0.0.1                                                                                |
| SFPOWERSCRIPTS\_STATSD\_PROTOCOL | string  | :Use `tcp` option for TCP protocol, or `uds` for the Unix Domain Socket protocol or `stream` for the raw stream. Defaults to `udp` otherwise. |

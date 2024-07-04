# New Relic

sfp is built with a metrics emitter that can emit metrics to NewRelic. In order to forward metrics, sfp requires two environment variables to be set

| Environment Variable              | Type    |                                                                 |
| --------------------------------- | ------- | --------------------------------------------------------------- |
| `SFPOWERSCRIPTS_NEWRELIC`         | boolean | Activate sending metrics to datadog by setting this key to true |
| `SFPOWERSCRIPTS_NEWRELIC_API_KEY` | secret  | Provide the **key** from the new "**Ingest - Licence"**         |

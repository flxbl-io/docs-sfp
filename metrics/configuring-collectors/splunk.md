# Splunk

sfp is built with a metrics emitter that can emit metrics to Splunk. In order to forward metrics, sfp requires three environment variables to be set

| Environment Variable            | Type    |                                                                           |
| ------------------------------- | ------- | ------------------------------------------------------------------------- |
| `SFPOWERSCRIPTS_SPLUNK`         | boolean | Activate sending metrics to splunk by setting this key to true            |
| `SFPOWERSCRIPTS_SPLUNK_HOST`    | string  | Provide the full **Splunk Event Collector Url** from your splunk instance |
| `SFPOWERSCRIPTS_SPLUNK_API_KEY` | secret  | Provide a **HEC-Token** from your splunk instance.                        |

Once few metrics are flown in, you can design the dashboard accordingly

# Datadog

sfp is built with a metrics emitter that can emit metrics to Datadog natively. In order to forward metrics, sfp requires three environment variables to be set

| Environment Variable            | Type    |                                                                                                                                                                                                                                                   |
| ------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SFPOWERSCRIPTS_DATADOG`        | boolean | Activate sending metrics to datadog by setting this key to true                                                                                                                                                                                   |
| `SFPOWERSCRIPTS_DATADOG_HOST`   | string  | Defaults to datadoghq.com. If you are on alternate site, please provide the site name from [https://docs.datadoghq.com/getting\_started/site/#access-the-datadog-site](https://docs.datadoghq.com/getting\_started/site/#access-the-datadog-site) |
| `SFPOWERSCRIPTS_SPLUNK_API_KEY` | secret  | Provide a DataDog [API\_KEY](https://docs.datadoghq.com/account\_management/api-app-keys/)  as a secret                                                                                                                                           |

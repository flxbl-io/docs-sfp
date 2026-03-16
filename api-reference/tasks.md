---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/api-reference/tasks
---

# Tasks

{% openapi-operation spec="sfp-server-api" path="/sfp/api/tasks/{operationId}/status" method="get" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/tasks/{operationId}/result" method="get" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/tasks/{operationId}" method="delete" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/tasks" method="post" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/tasks/{operationId}/stop-recurring" method="post" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/tasks/{operationId}/recurring-instances" method="get" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-schemas spec="sfp-server-api" schemas="TaskRetryConfig,TaskRecurrenceConfigDto,TaskResponseDto,TaskErrorDto,TaskStatusDto,TaskResultDto,SubmitTaskDto,CancelTaskResponseDto,StopRecurringTaskResponseDto" grouped="true" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-schemas %}

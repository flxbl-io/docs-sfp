---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/api-reference/authentication
---

# Authentication

{% openapi-operation spec="sfp-server-api" path="/sfp/api/auth/admin/login" method="post" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/auth/continue" method="get" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/auth/callback" method="post" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-schemas spec="sfp-server-api" schemas="AuthCallbackDto,AuthCallbackResponse,AdminLoginDto,AdminLoginResponse" grouped="true" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-schemas %}

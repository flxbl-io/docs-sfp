---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/api-reference/team
---

# Team

{% openapi-operation spec="sfp-server-api" path="/sfp/api/teams/{slug}" method="delete" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/teams/{slug}/members/{email}" method="delete" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/teams/{slug}/members/{email}/role" method="put" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/teams" method="post" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/teams/{slug}/members" method="get" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-operation spec="sfp-server-api" path="/sfp/api/teams/{slug}/members" method="post" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-operation %}

{% openapi-schemas spec="sfp-server-api" schemas="TeamMembershipDto,TeamDto,AddTeamMemberDto,AddTeamMemberResponse,ListTeamMembersResponseDto,CreateTeamDto,CreateTeamResponse,DeleteTeamResponse,RemoveTeamMemberResponse" grouped="true" %}
[OpenAPI sfp-server-api](https://flxbl-staging.flxbl.io/sfp/apidocs-json)
{% endopenapi-schemas %}

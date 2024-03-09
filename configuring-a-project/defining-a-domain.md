# Defining a domain

A domain is defined by a release configuration.  In order to define a domain,  you need to create a new [release config ](release-config.md)yaml file in your repository

A simple release config can be defined as shown below

<pre class="language-yaml"><code class="lang-yaml">// Sample release config &#x3C;business_domain.yaml>
releaseName: <a data-footnote-ref href="#user-content-fn-1">&#x3C;business_domain>  </a> # --> The name of the domain
pool: <a data-footnote-ref href="#user-content-fn-2">&#x3C;sandbox/scratch org pools></a>
excludeAllPackageDependencies: true
includeOnlyArtifacts:   # --> Insert packages
  - <a data-footnote-ref href="#user-content-fn-3">&#x3C;pkg1></a>
releasedefinitionProperties:
  promotePackagesBeforeDeploymentToOrg: prod
</code></pre>

The resulting file could be stored in your repository and utilized by the commands such as build, release etc.



[^1]: Replae with domain

[^2]: Pools  of  orgs where  the change should be validated

[^3]: Insert your packages here

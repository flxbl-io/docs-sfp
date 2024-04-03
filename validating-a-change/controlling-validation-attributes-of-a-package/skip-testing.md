# Skip Testing

| Attribute   | Type    | Description                                              | Package Types Applicable                                                               |
| ----------- | ------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| skipTesting | boolean | Skip trigger of apex tests while validating or deploying | <ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul> |

One can utilize this attribute on a package definition is sfdx-project.json to skipTesting while a change is validated. Use this option with caution, as the package when it gets deployed ([depending on the package behaviour](../../concepts/supported-package-types/))  might trigger testing while being deployed to production



<pre class="language-json"><code class="lang-json">// Demonstrating how to do use skipTesting
{
  "packageDirectories": [
    {
      "path": "core-crm",
      "package": "core-crm",
      "versionDescription": "Package containing core schema and classes",
      "versionNumber": "4.7.0.NEXT",
      <a data-footnote-ref href="#user-content-fn-1">"skipTesting": true</a>
    },
     ...
   ]
}
</code></pre>

[^1]: Add skipTesting to skip testing while validating a package

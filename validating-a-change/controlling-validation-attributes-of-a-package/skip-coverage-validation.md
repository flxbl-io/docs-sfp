# Skip Coverage Validation



| Attribute                  | Type    | Description                                      | Package Types Applicable                                                                 |
| -------------------------- | ------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| **skipCoverageValidation** | boolean | Skip apex test coverage validation of a package  | <ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff  </li></ul> |

sfp during validation checks the apex test coverage of a package depending on the [package type](../../concepts/supported-package-types/).  This is beneficial so that you dont run into any coverage issues while deploying into higher environments or building an unlocked package. \


However, there could be situations where the test coverage calculation is flaky , sfp provides you with an option to turn the coverage validation off.

<pre class="language-json"><code class="lang-json">// Demonstrating how to do use skipCoverageValidation
{
  "packageDirectories": [
    {
      "path": "core-crm",
      "package": "core-crm",
      "versionDescription": "Package containing core schema and classes",
      "versionNumber": "4.7.0.NEXT",
      <a data-footnote-ref href="#user-content-fn-1">"skipCoverageValidation": true</a>
    },
     ...
   ]
}
</code></pre>

[^1]: Skip coverage validation of a package

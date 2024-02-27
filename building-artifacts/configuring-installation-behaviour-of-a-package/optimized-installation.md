# Optimized Installation

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>isOptimizedDeployment</td><td>boolean</td><td>Detects test classes in a source package automatically and utilise it to deploy the provided package</td><td><p></p><ul><li>source</li><li>diff</li></ul></td></tr></tbody></table>

\
\
sfp cli optimally deploys artifacts to the target organisation by reducing the time spent on running Apex tests where possible. This section explains how this optimisation works for different package types.

<table><thead><tr><th>Package Type</th><th width="258">Apex Test Execution during  installation</th><th>Coverage Requirement</th></tr></thead><tbody><tr><td>Unlocked Package</td><td>No</td><td>75% for the contents of a package validated during build</td></tr><tr><td>Org Dependent Unlocked Package</td><td>No</td><td>No </td></tr><tr><td>Source Package</td><td>Yes </td><td>Yes, either each classes need to have individual 75% coverage or use the entire org's apex test coverage</td></tr><tr><td>Diff Package</td><td>Yes</td><td>Yes, either each classes need to have individual 75% coverage or use the entire org's coverage</td></tr><tr><td>Data Package</td><td>No</td><td>No</td></tr></tbody></table>



To ensure salesforce deployment requirements for source packages,  each Apex class within the source package must achieve a minimum of 75% test coverage. This coverage must be verified individually for each class. It's imperative that the test classes responsible for this coverage are included within the same package.

During the artifact preparation phase, sfp cli will automatically identify Apex test classes included within the package. These identified test classes will then be utilised at the time of installation to verify that the test coverage requirement is met for each Apex class, ensuring compliance with Salesforce's deployment standards. \
\
When there are circumstances, where individual coverage cannot be achieved by the apex classes within a package,  one can disable 'optimized deployment' feature by using the attribute mentioned below.&#x20;

{% hint style="info" %}
This option is only applicable to source/diff packages.  Disabling this option will trigger the entire local tests in the org and can lead to considerable delays
{% endhint %}

<pre><code>// Demonstrating how to disable optimized deployment
{
  "packageDirectories": [
    {
      "path": "core-crm",
      "package": "core-crm",
      "versionDescription": "Package containing core schema and classes",
      "versionNumber": "4.7.0.NEXT",
      "<a data-footnote-ref href="#user-content-fn-1">isOptimizedDeployment</a>": false
    },
     ...
   ]
}
</code></pre>



[^1]: Use isOptimizedDeployment to enable/disable individual coverage vs org coverage


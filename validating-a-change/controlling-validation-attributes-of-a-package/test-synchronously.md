# Test Synchronously

| Attribute           | Type    | Description                                                | Package Types Applicable                                                               |
| ------------------- | ------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **testSynchronous** | boolean | Ensure all tests of the package is triggered synchronously | <ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul> |



sfp by default attempts to trigger all the apex tests in a package in asynchronous mode during validation.  This is to ensure that you have a highly performant apex test classes which provide you fast feedback. &#x20;

{% hint style="info" %}
During installation of packages, test cases are triggered synchronously for source and diff packages.  Unlocked and Org-depednendent packages do not trigger any apex test during installation
{% endhint %}

If you have inherited an existing code base or your unit tests have lot of DML statemements,  you will encounter test failuers when a tests in package is triggered asynchronously.  You can instruct sfp to trigger the tests of the package in a synchronous mode by setting '`testSynchnronous` as true\
\


<pre class="language-json"><code class="lang-json">// Demonstrating how to do use testSynchronous
{
  "packageDirectories": [
    {
      "path": "core-crm",
      "package": "core-crm",
      "versionDescription": "Package containing core schema and classes",
      "versionNumber": "4.7.0.NEXT",
      <a data-footnote-ref href="#user-content-fn-1">"testSynchronous": true</a>
    },
     ...
   ]
}
</code></pre>

[^1]: Add testSynchronous to trigger apex testing in synchronous mode


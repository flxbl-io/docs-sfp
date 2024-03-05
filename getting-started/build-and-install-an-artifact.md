# Build & Install an artifact

In the earlier chapter, we looked at how we configured the project directory for  sfp,  Now lets run down some commands

### Build an artifact for a package

Open up a terminal within your  Salesforce project directory and   enter the following command

<pre class="language-asciidoc"><code class="lang-asciidoc">➜ <a data-footnote-ref href="#user-content-fn-1">sfp build -v </a>&#x3C;DevHubAlias/DevHubUsername>
</code></pre>

You will see the logs with details of your packaage creation, for instance here is a sample output\


<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

### Install the artifact to a target org

Ensure you have authenticated your salesforce cli to a sandbox.  If you  havent, please find the instructions [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_auth\_web\_flow.htm)&#x20;

Once you have the authenticated to the sandbox, you could execute the installation command as below

```
➜ sfp install -u <TargetOrgAlias/TargetOrgUsername>
```

\


<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption><p>Output from installation command</p></figcaption></figure>

{% hint style="info" %}
Depending on the  type of packages,[ sfp will issue the equivalent test classes](../concepts/supported-package-types/) with in the package directory and it could result in failures during installation,  Please fix the issues in your code and repeat till you get a succesfull installation.   If your packaged doesn't have sufficient test coverage, you may need to use the all tests in the org to get your package installed. Refer [here](../building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation.md)
{% endhint %}



[^1]: Execute the build command

# Skip Install on Certain Orgs

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>skipDeployOnOrgs</td><td>array</td><td>Skips installation of an artifact on a target org</td><td><p></p><ul><li>org-dependent unlocked</li><li>unlocked</li><li>data</li><li>source</li><li>diff</li></ul></td></tr></tbody></table>



sfp cli when encounters the attribute `skipDeployOnOrgs` on a package, the generated artifact during installation is checked against the alias or the username passed onto the installation command. If the username or the alias is matched, the artifact installation is skipped



<pre><code><strong>// Demonstrating how to do use skipDeployOnOrgs
</strong>{
  "packageDirectories": [
    {
      "path": "core-crm",
      "package": "core-crm",
      "versionDescription": "Package containing core schema and classes",
      "versionNumber": "4.7.0.NEXT",
      "<a data-footnote-ref href="#user-content-fn-1">skipDeployOnOrgs</a>":[
       "qa",
       "user@colaorg.com.au.sit
      ]
    },
     ...
   ]
}
</code></pre>

In the above example, if the alias to installation command is qa, the artifact for core-crm will get skipped during intallation. The same can also be applied for username of an org as well

[^1]: use skipDeployOnOrgs to skip installation  to specific orgs

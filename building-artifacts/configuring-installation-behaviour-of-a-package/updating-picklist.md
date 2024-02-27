# Updating Picklist

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>enablePicklist</td><td>boolean</td><td>Enable  picklist upgrade for unlocked packages</td><td><p></p><ul><li>unlocked</li><li>org dependent unlocked</li></ul><p></p></td></tr></tbody></table>



Salesforce inherently does not support the deployment of picklist values as part of unlocked package upgrades. This limitation has led to the need for workarounds, traditionally solved by either replicating the picklist in the source package or manually adding the changes in the target organization. Both approaches add a burden of extra maintenance work. This issue has been documented and recognised as a known problem, which can be reviewed in detail [here](https://issues.salesforce.com/issue/a028c00000qPzYUAA0).



To ensure picklist consistency between local environments and the target Salesforce org, the following pre-deployment steps outline the process for managing picklists within unlocked packages:

1. **Retrieval and Comparison**: Initially, picklists defined within the packages marked for deployment will be retrieved from the target org. These retrieved values are then compared with the local versions of the picklists.
2. **Update on Change Detection**: Should any discrepancies be identified between the local and retrieved picklists, the differing values will be updated in the target org using the Salesforce Tooling API.
3. **Handling New Picklists**: During the retrieval phase, picklists that are present locally but absent in the target org are identified as newly created. These new picklists are deployed using the standard deployment process, as Salesforce permits the deployment of new picklists from unlocked packages.
4. **Picklist Enabled Package Identification**: Throughout the build process, the sfp cli examines the contents of each unlocked package artifact. A package is marked as 'picklist enabled' if it contains one or more picklist(s).

<pre><code>// Demonstrating how to disable optimized deployment
{
  "packageDirectories": [
    {
      "path": "core-crm",
      "package": "core-crm",
      "versionDescription": "Package containing core schema and classes",
      "versionNumber": "4.7.0.NEXT",
      "<a data-footnote-ref href="#user-content-fn-1">enablePicklist</a>": false
    },
     ...
   ]
}
</code></pre>

[^1]: Use enablePicklistAttribute to determine picklist update

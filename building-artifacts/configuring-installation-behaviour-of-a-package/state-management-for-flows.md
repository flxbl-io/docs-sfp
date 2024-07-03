# State management for Flows

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>enableFlowActivation</td><td>boolean</td><td>Enable Flows automatically in Production</td><td><p></p><ul><li>source</li><li>diff</li><li>unlocked</li></ul><p></p></td></tr></tbody></table>

While installing a source/diff package, flows by default are deployed as 'Inactive' in the production org. One can deploy flow as 'Active' using the steps mentioned [here](https://help.salesforce.com/s/articleView?id=sf.flow\_distribute\_deploy\_active.htm\&language=en\_US\&type=5), however, this requires the flow to meet test coverage requirement.

Also making a flow inactive, is convoluted, find the detailed article provided by [Gearset](https://gearset.com/blog/deactivate-flows-within-your-data-deployments/)

sfp has automation built in that attempts to align the status of a particular flow version as kept inside the package. It automatically activates the latest version of a Flow in the target org (assuming the package has the latest version). If an earlier version of package is deployed, it skips the activation.

At the same time, if the package contains a Flow with status Obsolete/InvalidDraft/Draft, all the versions of the flow would be rendered inactive.\


{% hint style="info" %}
This feature is enabled by default, if you would like to disable the feature, set enableFlowActivation to false on your package descriptor
{% endhint %}



```json
// Demonstrating package by disabling flow activation
{
  "packageDirectories": [
      {    
      "path": "src/order-management",
      "package": "order-management",
      "versionNumber": "2.0.10.NEXT",
      "enableFlowActivation" : false
    }
```

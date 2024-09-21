# Aliasfy Packages



<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>aliasfy</td><td>boolean</td><td>Enable  deployment of contents of a folder that matches the alias of the environment</td><td><p></p><ul><li>source</li></ul><p></p></td></tr></tbody></table>

{% hint style="info" %}
Aliasified packages are available only for Source
{% endhint %}

Aliasify enables deployment of a subfolder in a source package that matches the target org. For example, you have a source package as listed below. \


<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

\
During Installation, only the metadata contents of the folder that matches the alias gets deployed. If the alias is not found, sfp will fallback to the '**default**' folder. If the default folder is not found, an error will be displayed saying default folder or alias is missing.\


```json
// Demonstrating package directory with aliasfy
{
  "packageDirectories": [
      {    
      "path": "src/src-env-specific-alias-post",
      "package": "src-env-specific-alias-post",
      "versionNumber": "2.0.10.NEXT",
      "aliasfy" : true
    }
```

{% hint style="info" %}
**Default** folder are only deployed to sandboxes
{% endhint %}

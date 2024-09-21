# Aliasfy Packages V2

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>aliasfyV2</td><td>boolean</td><td>Enable  deployment of contents of a folder that matches the alias of the environment</td><td><p></p><ul><li>source</li></ul><p></p></td></tr></tbody></table>

{% hint style="info" %}
Aliasified packages are available only for Source packages
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
**aliasifyV2** introduces default subfolder <mark style="color:blue;">**content inheritance**</mark>. During package build, the default folder's content is inherited by subfolders, with subfolders able to override inherited content. This reduces metadata duplication across subfolders.
{% endhint %}

<div>

<figure><img src="../../.gitbook/assets/Screenshot 2024-09-17 at 12.58.56.png" alt="" width="188"><figcaption><p>Package Content (before build)</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/Screenshot 2024-09-17 at 12.57.20.png" alt="" width="188"><figcaption><p>Artifact Content (after build)</p></figcaption></figure>

</div>

Note in the image above that the `AccountNumberDefault` field is replicated in each subdirectory.



{% hint style="info" %}
The **default** folder is deployed to any type of environment (different from the previous aliasfy version)
{% endhint %}

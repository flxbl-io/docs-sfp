---
icon: ring-diamond
---

# Aliasfy Packages - Merge Mode

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>aliasfyMergeMode</td><td>boolean</td><td>Enable  deployment of contents of a folder that matches the alias of the environment  using merge</td><td><p></p><ul><li>source</li></ul><p></p></td></tr></tbody></table>

|              | sfp-pro      | sfp (community) |
| ------------ | ------------ | --------------- |
| Availability | ✅            | ❌               |
| From         | September 24 |                 |



**aliasfyMergeMode**  adds an additional mode for deploying aliasified packages with  <mark style="color:blue;">**content inheritance**</mark>. During package build, the default folder's content is merged with subfolders that matches the org with alias name,  along with subfolders able to override inherited content. This reduces metadata duplication  while using aliasifed packages.\


```json
// Demonstrating package directory with aliasfy merge mode
{
  "packageDirectories": [
      {    
      "path": "src/src-env-specific-alias-post",
      "package": "src-env-specific-alias-post",
      "versionNumber": "2.0.10.NEXT",
      "aliasfy" : true
      "aliasfyMergeMode" : true
    }
```

<div>

<figure><img src="../../../.gitbook/assets/Screenshot 2024-09-17 at 12.58.56.png" alt="" width="188"><figcaption><p>Package Content (before build)</p></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Screenshot 2024-09-17 at 12.57.20.png" alt="" width="188"><figcaption><p>Artifact Content (after build)</p></figcaption></figure>

</div>

Note in the image above that the `AccountNumberDefault` field is replicated in each deployment directory that matches the alias.

{% hint style="info" %}
The **default** folder is deployed to any type of environment, including production unlike when only used with aliasfy mode
{% endhint %}

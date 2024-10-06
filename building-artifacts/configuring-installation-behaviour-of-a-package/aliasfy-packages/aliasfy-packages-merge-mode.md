---
icon: ring-diamond
---

# Aliasfy Packages - Merge Mode

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>mergeMode</td><td>boolean</td><td>Enable  deployment of contents of a folder that matches the alias of the environment  using merge</td><td><p></p><ul><li>source</li></ul><p></p></td></tr></tbody></table>

|              | sfp-pro      | sfp (community) |
| ------------ | ------------ | --------------- |
| Availability | ✅            | ❌               |
| From         | September 24 |                 |



**mergeMode**  adds an additional mode for deploying aliasified packages with  <mark style="color:blue;">**content inheritance**</mark>. During package build, the default folder's content is merged with subfolders that matches the org with alias name,  along with subfolders able to override inherited content. This reduces metadata duplication  while using aliasifed packages.\


```json
// Demonstrating package directory with aliasfy merge mode
{
  "packageDirectories": [
      {    
      "path": "src/src-env-specific-alias-post",
      "package": "src-env-specific-alias-post",
      "versionNumber": "2.0.10.NEXT",
      "aliasfy" : {
          "mergeMode": true
      }
    }
```

<div>

<figure><img src="../../../.gitbook/assets/Screenshot 2024-09-17 at 12.58.56.png" alt="" width="188"><figcaption><p>Package Content (before build)</p></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Screenshot 2024-09-17 at 12.57.20.png" alt="" width="188"><figcaption><p>Artifact Content (after build)</p></figcaption></figure>

</div>

Note in the image above that the `AccountNumberDefault__c` field is replicated in each deployment directory that matches the alias.

{% hint style="info" %}
The **default** folder is deployed to any type of environment, including production unlike when only used with aliasfy mode
{% endhint %}

The table below describes the behavior of each command when merge mode is enabled/disabled.

<table><thead><tr><th width="152" data-type="checkbox">Merge Mode</th><th width="174" data-type="checkbox">Default available?</th><th width="167">Build</th><th width="248">Install</th><th width="133">Push</th><th>Pull</th></tr></thead><tbody><tr><td>true</td><td>true</td><td><strong>Default</strong> metadata is merged into  each subfolder</td><td>Deploys subfolder that matches enviroment alias name.<br>If there is no match, then <strong>default</strong> subfolder is deployed <br>(Including production)</td><td>Only <strong>default</strong> is pushed</td><td>Only <strong>default</strong> is pulled</td></tr><tr><td>true</td><td>false</td><td><strong>Default</strong> merging not available</td><td>Deploys subfolder that matches enviroment alias name.<br>If there is no match, then <strong>default</strong> subfolder is deployed <br>(Including production)</td><td></td><td></td></tr><tr><td>false</td><td>true</td><td><strong>Default</strong> merging not available</td><td>Deploys subfolder that matches enviroment alias name.<br>If there is no match, deploys default<br>(Only sandboxes)</td><td>Only <strong>default</strong> is pushed</td><td>Only <strong>default</strong> is pushed</td></tr><tr><td>false</td><td>false</td><td><strong>Default</strong> merging not available</td><td>Deploys subfolder that matches enviroment alias name.<br>If there is no match, fails</td><td>Nothing to push</td><td>Nothing to pull</td></tr></tbody></table>



{% hint style="info" %}
When merge mode is enabled, it also supports push/pull commands, the **default** subfolder is always used during this process, make sure it is not force ignored.
{% endhint %}

Before merge mode, the whole package was "force ignored." With merge mode, you have the option to allow push/pull from aliasfy packages by not ignoring the **default** subfolder.

<table><thead><tr><th width="374">Non-merge mode</th><th>Merge Mode</th></tr></thead><tbody><tr><td><pre class="language-gitignore"><code class="lang-gitignore"># .forceIgnore (aliasfy non-merge mode)
src-env-specific-alias-post
</code></pre></td><td><pre class="language-gitignore"><code class="lang-gitignore"># .forceIgnore (aliasfy merge mode)
src-env-specific-alias-post/dev
src-env-specific-alias-post/test
src-env-specific-alias-post/prod
</code></pre></td></tr></tbody></table>


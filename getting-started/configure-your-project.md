# Configure your project

### 1. Ensure you have enabled and authenticated to DevHub

Ensure you have [enabled DevHub in your  production org ](setup-salesforce-org.md)or created a developer org where DevHub is enabled

### 2.  Authenticate your Salesforce CLI to DevHub

Ensure you have authenticated your local development environment to DevHub, if you are not familiar, you can follow the instructions provided [here](https://trailhead.salesforce.com/content/learn/projects/quick-start-salesforce-dx/set-up-your-salesforce-dx-environment)

### 3 .  Add additional attributes to your Salesforce DX Project 

{% hint style="info" %}
sfp cli only works with a Salesforce DX project, with source format as described [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_source\_file\_format.htm) . If your project is not in the source format, you would need to convert into source format using the [options provided by salesforce cli. ](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_ws\_create\_from\_existing.htm)
{% endhint %}

Navigate to your sfdx-project.json, locate a package directory , For eg  given the below sfdx-project.json

```
{ 
"packageDirectories" : [ 
    { "path": "force-app", "default": true}, 
    { "path" : "unpackaged" }, 
    { "path" : "utils" } 
  ],
"namespace": "", 
"sfdcLoginUrl" : "https://login.salesforce.com", 
"sourceApiVersion": "58.0"
}
```

the package directories are `force-app , unpackaged and utils` , Add the following additional  attributes to the sfdx=project.json\


<pre class="language-jsonp"><code class="lang-jsonp">{
   "packageDirectories" : [ 
    {
       <a data-footnote-ref href="#user-content-fn-1">"package": "force-app",</a>
       <a data-footnote-ref href="#user-content-fn-2">"versionNumber": "1.0.6.NEXT",</a>
       "path": "force-app",
       "default": true
     }, 
    { "path" : "unpackaged" },  // You can repeat the same for additonal directories
    { "path" : "utils" }  // You can repeat the same for additonal directories
  ],
"namespace": "", 
"sfdcLoginUrl" : "https://login.salesforce.com", 
"sourceApiVersion": "58.0"
}
</code></pre>

Thats the minimal configuration required to run sfp on a project,  Move on to the next chapter to execute sfp commands in this directory.  If you are looking for advanced configurations, please head to  this [link](configure-your-project.md).

[^1]: Add an additional package attribute

[^2]: Add an additional versionNumber attribute


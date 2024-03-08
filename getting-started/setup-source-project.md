# Configure Your Project

### A. Ensure you have enabled and authenticated to DevHub

Ensure you have [enabled DevHub](setup-salesforce-org.md) in your  production org or created a developer org.

### B. Authenticate your Salesforce CLI to DevHub

Ensure you have authenticated your local development environment to your DevHub.  If you are not familiar with the process, you can follow the instructions provided by Salesforce [here](https://trailhead.salesforce.com/content/learn/projects/quick-start-salesforce-dx/set-up-your-salesforce-dx-environment).

### C. Add additional attributes to your Salesforce DX Project 

{% hint style="info" %}
sfp cli only works with a Salesforce DX project, with source format as described [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_source\_file\_format.htm) . If your project is not source format, you would need to convert into source format using the [options provided by salesforce cli. ](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_ws\_create\_from\_existing.htm)
{% endhint %}

1. Navigate to your sfdx-project.json file and locate the `packageDirectories` property. &#x20;

```json
{ 
"packageDirectories" : [ 
    { "path": "force-app", "default": true}, 
    { "path" : "unpackaged" }, 
    { "path" : "utils" } 
  ],
"namespace": "", 
"sfdcLoginUrl" : "https://login.salesforce.com", 
"sourceApiVersion": "60.0"
}
```

In the above example, the package directories are

* `force-app`
* `unpackaged`
* `utils`&#x20;

3. Add the following additional attributes to the sfdx-project.json and save.

* `package`
* `versionNumber`

```json
{
   "packageDirectories" : [ 
    {
       "package": "force-app",
       "versionNumber": "1.0.0.NEXT",
       "path": "force-app",
       "default": true
     }, 
    { "path" : "unpackaged" },  // You can repeat the same for additonal directories
    { "path" : "utils" }  // You can repeat the same for additonal directories
   ],
"namespace": "", 
"sfdcLoginUrl" : "https://login.salesforce.com", 
"sourceApiVersion": "60.0"
}
```

Thats the minimal configuration required to run sfp on a project.

Move on to the next chapter to execute sfp commands in this directory. &#x20;

{% hint style="info" %}
For detailed configurations on this sfdx-project.json schema for sfp, click [here](https://github.com/flxbl-io/sfp/blob/main/packages/sfp-cli/resources/schemas/sfdx-project.schema.json).
{% endhint %}

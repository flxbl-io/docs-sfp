# Handling dependencies

prepare command has inbuilt capability to orchestrate installation of external package dependencies. Package dependencies are defined in the sfdx-project.json. More information on defining package dependencies can be found in the Salesforce [docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev2gp\_config\_file.htm).

```javascript
{
  "packageDirectories": [
    {
      "path": "util",
      "default": true,
      "package": "Expense-Manager-Util",
      "versionName": "Winter ‘22",
      "versionDescription": "Welcome to Winter 2022 Release of Expense Manager Util Package",
      "versionNumber": "4.7.0.NEXT"
    },
    {
      "path": "exp-core",
      "default": false,
      "package": "ExpenseManager",
      "versionName": "v 3.2",
      "versionDescription": "Winter 2022 Release",
      "versionNumber": "3.2.0.NEXT",
      "dependencies": [
        {
          "package": "ExpenseManager-Util",
          "versionNumber": "4.7.0.LATEST"
        },
          {
          "package": "TriggerFramework",
          "versionNumber": "1.7.0.LATEST"
        },
        {
          "package": "External Apex Library - 1.0.0.4"
        }
      ]
    }
  ],
  "sourceApiVersion": "59.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

Let's unpack the concepts utilizing the above example:

* There are two unlocked packages
  * Expense Manager - Util is an unlocked package in your DevHub, identifiable by **0H** in the packageAlias
  * Expense Manager - another unlocked package which is dependent on ' Expense Manager - Util', 'TriggerFramework' and 'External Apex Library - 1.0.0.4'
* External Apex Library is an external dependency, It could be a managed package or any unlocked package released on a different Dev Hub. All external package dependencies have to be defined with a **04t** ID, which can be determined from the installation URL from AppExchange or by contacting your vendor.
* sfpowerscripts parses sfdx-project.json and does the following in order
  * Skips Expense manager - Util as it doesn't have any dependencies
  * For Expense manager
    * Checks whether any of the package is part of the same repo, in this example 'Expense Manager-Util' is part of the same repository and will not be installed as a dependency
    * Installs the latest version of TriggerFramework ( with major, minor and patch versions matching 1.7.0) to the scratch org
    * Install the 'External Apex Library - 1.0.0.4' by utilizing the 04t id provided in the packageAliases

If any of the managed package has keys, it can be provided as an argument to the prepare command. Check the command's flags for more information.

### Key Support for Managed Packages

The format for the 'keys' parameter is a string of key-value pairs separated by spaces - where the key is the name of the package, the value is the protection key of the package, and the key-value pair itself is delimited by a colon .

e.g. `--keys "packageA:12345 packageB:pw356 packageC:pw777"`

{% hint style="warning" %}
The time taken by this command depends on how many managed packages and your packages that need to be installed. Please note, if you are triggering this command in a CI server, ensure proper time outs are provided for this task, as most cloud-based CI providers have time limits on how long a single task can be run. Explore [multi-stage prepare jobs](https://github.com/dxatscale/sfpowerscripts/blob/main/decision%20records/prepare/002-prepare-daisy-chaining.md) in case this is a blocker for the team.
{% endhint %}

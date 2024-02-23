# Dependency management

sfp features commands and functionality that helps in dealing with complexity of defining dependency of unlocked packages. These functionalities are designed considering aspects of a #flxbl  project, such as the predominant use of mono repositories in the context of a non-ISV scenarios.

Package dependencies are defined in the sfdx-project.json (Project Manifest). More information on defining package dependencies can be found in the Salesforce [docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev2gp\_config\_file.htm).

```javascript
{
  "packageDirectories": [
    {
      "path": "util",
      "package": "Expense-Manager-Util",
      "versionName": "Winter ‘25",
      "versionDescription": "Welcome to Winter 2025 Release of Expense Manager Util Package",
      "versionNumber": "4.7.0.NEXT"
    },
    {
      "path": "exp-core",
      "default": false,
      "package": "ExpenseManager",
      "versionName": "v 3.2",
      "versionDescription": "Winter 2025 Release",
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
    },
    {
      "path": "src/exp-core-config",
      "package": "Expense-Manager-Org-Config",
      "type" : "source",
      "versionName": "Winter ‘25",
      "versionDescription": "Welcome to Winter 2025 Release of Expense Manager Util Package",
      "versionNumber": "4.7.0.NEXT"
    },
  ],
  "sourceApiVersion": "47.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

Let's unpack the concepts utilizing the above example:

* There are two unlocked packages and one source package
  * Expense Manager - Util is an unlocked package in your DevHub, identifiable by 0H in the packageAlias
  * Expense Manager - another unlocked package which is dependent on ' Expense Manager - Util', 'TriggerFramework' and 'External Apex Library - 1.0.0.4'
  * Expense Manager Org Config -  a source package, lets assume has some reports and dashboards
* External Apex Library is an external dependency, it could be a managed package or any unlocked package released on a different Dev Hub. All external package dependencies must be defined with a 04t ID, which can be determined from the installation URL from AppExchange or by contacting your vendor.
* Source Packages/Org Dependent Unlocked / Data packages have an implied dependency as defined by the order of installation, as in it assumes any depedent metadata is already available in the target org before the installation of components within the package

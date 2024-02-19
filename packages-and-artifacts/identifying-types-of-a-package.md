# Identifying types of a package

This section details identifies on how sfp analyzes and classifies different package types by using the information in sfdx-project.json

* **Unlocked Packages** are identified if a matching alias with a package version ID is found and verified through the DevHub. For example, the package named "Expense-Manager-Util" is found to be an Unlocked package upon correlation with its alias "Expense Manager - Util" and subsequent verification.
* **Source Packages** are assumed if no matching alias is found in `packageAliases`. These packages are typically used for source code that is not meant to be packaged and released as a managed or unlocked package.
* The presence of an additional `type` attribute within a package directory will further inform sfp of the specific nature of the package. For instance, types such as "data" for data packages or "diff" for diff packages

The sfdx-project.json file outlines various specifications for Salesforce DX projects, including the definition and management of different types of Salesforce packages. From the sample provided, sfp (Salesforce Package Builder) analyzes the "package" attribute within each `packageDirectories` entry, correlating with `packageAliases` to identify package IDs, thereby determining the package's type as 2GP (Second Generation Packaging).

Consider the following sfdx-project.json&#x20;

```
// Sample sfdx-project.json
{
  "packageDirectories": [
    {
      "path": "util",
      "default": true,
      "package": "Expense-Manager-Util",
      "versionName": "Winter ‘20",
      "versionDescription": "Welcome to Winter 2020 Release of Expense Manager Util Package",
      "versionNumber": "4.7.0.NEXT"
    },
    {
      "path": "exp-core",
      "default": false,
      "package": "ExpenseManager",
      "versionName": "v 3.2",
      "versionDescription": "Winter 2020 Release",
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
      "path": "exp-core-config",
      "package": "expense-manager-config",
      "versionNumber": "1.0.1.NEXT",
      "versionDescription": "This source package extends expense manager unlocked package",
    },
    {
      "path": "expense-manager-test-data",
      "package": "exppense-manager-test-data",
      "type":"data",
      "versionNumber": "1.0.1.NEXT",
      "versionDescription": "This source package extends expense manager unlocked package",
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

#### Understanding Package Type Determination using  the sample`sfdx-project.json`

The `sfdx-project.json` sample can be used to determine how  sfp processes and categorizes packages within a Salesforce DX project. The determination process for each package type, based on the attributes defined in the `packageDirectories`, unfolds as follows:

* **Unlocked Packages:** For a package to be identified as an Unlocked package, sfp looks for a correlation between the `package` name defined in `packageDirectories` and an alias within `packageAliases`. In the provided example, the "Expense-Manager-Util" under the `util` path is matched with its alias "Expense Manager - Util", subsequently confirmed through the DevHub with its package version ID, categorizing it as an Unlocked package.
* **Source Packages:** If a package does not have a corresponding alias in `packageAliases`, it is treated as a Source package. These packages are typically utilized for organizing source code not intended for release. For instance, packages specified in paths like "exp-core-config" and "expense-manager-test-data" would default to Source packages if no matching aliases are found.
* **Specialized Package Types:** The explicit declaration of a `type` attribute within a package directory allows for the differentiation into more specialized package types. For example, the specification of `"type": "data"` explicitly marks a package as a Data package, targeting specific use cases different from typical code packages.


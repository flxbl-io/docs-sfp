---
description: All packages start out as directory in your repo!
---

# Creating a package

A package is a collection of metadata grouped together in a directory, and defined by an entry in your sfdx-project.json ( Project Manifest). &#x20;

```
// A sample sfdx-project.json with a packag
{
  "packageDirectories": [
    {
      "path": "src-env-specific-pre",
      "package": "env-specific-pre",
      "versionNumber": "4.7.0.NEXT",
    },
     ...
   ]
}
```

Each package in the context of sfp need to have the following attributes as the absolute minimum\


| Attribute          | Required |                                                                  |
| ------------------ | -------- | ---------------------------------------------------------------- |
| path               | yes      | Path the the directory that contains the contents of the package |
| package            | yes      | The name of the package                                          |
| versionNumber      | yes      | The version number of the package                                |
| versionDescription | no       | Description for a particular version of the package              |

sfp will not consider any entries in your sfdx-project.json for its operations if  it is missing 'package' or 'versionNumber' attribute,\
\
By default, sfp treats all entries in sfdx-project.json as [Source Packages](supported-package-types/source-packages.md).  If you need to create an unlocked or an org depednent unlocked package, you need to proceed to create  the packages using the follwing steps detailed below

### Creating an Unlocked Package

#### Creating an Unlocked Package with SF CLI

To create an unlocked package using the Salesforce CLI, follow the steps below:

1. **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
2.  **Create a Package**: If the package does not already exist, create it with the command:

    ```
    sf package:create --name <package_name> --packagetype Unlocked  --nonamespace -o <alias_for_org>
    ```

    Replace `<package_name>` with the name of your unlocked package,   with the package directory specified in your `sfdx-project.json`, and `<alias_for_org>` with the alias for your Salesforce org.
3. Ensure that an entry for your package is created in your packageAliases section.  Please commit the updated sfdx-project.json to your version control, before proceeding with sfp commands

### Creating an Org Dependent Unlocked Package

#### Creating an Org Dependent Unlocked Package with SF CLI

To create an unlocked package using the Salesforce CLI, follow the steps below:

1. **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
2.  **Create a Package**: If the package does not already exist, create it with the command:

    ```
    sf package:create --name <package_name> --packagetype Unlocked  --org-dependent      --nonamespace -o <alias_for_org>
    ```

    Replace `<package_name>` with the name of your unlocked package,   with the package directory specified in your `sfdx-project.json`, and `<alias_for_org>` with the alias for your Salesforce org.
3. Ensure that an entry for your package is created in your packageAliases section.  Please commit the updated sfdx-project.json to your version control, before proceeding with sfp commands

### Creating a data package

* **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
* Ensure your package directory is populated with an  export-json and the required CSV files. Read on [here](supported-package-types/data-packages.md) to learn more about data packages
*   **Add an additional attribute of  "type":"data"**\
    \


    ```
        {
        "path": "path--to--package",
        "package": "name--of-the-package", 
        "versionNumber": "X.Y.Z.[NEXT/BUILDNUMBER]",
        "type": "data", // required for data packages
        }
    ```

### Creating a diff package

* **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
* Ensure your package directory is populated with an  export-json and the required CSV files. Read on [here](supported-package-types/diff-package.md) to learn more about diff packages
*   **Add an additional attribute of  "type":"diff"**\
    \


    ```
        {
        "path": "path--to--package",
        "package": "name--of-the-package", 
        "versionNumber": "X.Y.Z.[NEXT/BUILDNUMBER]",
        "type": "diff", // required for data packages
        }
    ```
* Ensure a  new record is created in SfpowerscriptsArtifact2\_\_c object in your devhub, with the details such as the name of the package, the initial version number, the baseline commit id

---
description: Unlocked/Org Dependent Unlocked Packages
---

# Unlocked Packages

There is a huge amount of documentation on unlocked packages.  Here are list of curated links that can help you get started on your unlocked package journey

_**The Basics**_

* [Package Development Model](https://trailhead.salesforce.com/content/learn/modules/sfdx\_dev\_model)
* [Unlocked Package for Customers](https://trailhead.salesforce.com/content/learn/modules/unlocked-packages-for-customers)
* [Successfully Creating Unlocked Package](https://www.youtube.com/watch?v=xJNmHOtIgO0)
* [Salesforce Developer Guide to Unlocked Package](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_unlocked\_pkg\_intro.htm)
* [Unlocked Package FAQ](https://sfdc-db-gmail.github.io/unlocked-packages/faq-unlocked-pkgs.html)

_**Advanced Materials**_

* [Anti Patterns in Package Dependency Design](https://medium.com/salesforce-architects/5-anti-patterns-in-package-dependency-design-and-how-to-avoid-them-87bb50331cb8)

The following sections deals with more of the operational aspects when dealing with unlocked packages

## Unlocked Package and Test Coverage

Unlocked Packages, excluding Org-Dependent unlocked packages have mandatory test coverage requirements. Each package should have minimum of 75% coverage requirement. A validated build (or [build command ](https://dxatscale.gitbook.io/sfpowerscripts/commands/build-and-quickbuild)in sfp) validates the coverage of package during the build phase. To enable the feedback earlier in the process, sfp  provide you functionality to validate test coverage of a package such as  during the Pull Request Validation process.

## Managing Version Numbers of Unlocked Package

For unlocked packages, we ask users to follow a [semantic](https://semver.org) versioning of packages.

{% hint style="info" %}
Please note Salesforce packages do not support the concept of PreRelease/BuildMetadata. The last segment of a version number is a build number. We recommend to utilize the auto increment functionality provided by Salesforce rather than rolling out your own build number substitution ( Use 'NEXT' while describing the build version of the package and 'LATEST' to the build number where the package is used as a dependency)
{% endhint %}

Note that an unlocked package must be [promoted](https://sfpowerscripts.dxatscale.io/commands/command-glossary#sfdx-sfpowerscripts-orchestrator-promote) before it can be installed to a production org, and either the major, minor or patch (not build) version **must** be higher than the last version of this package which was promoted. These version number changes should be made in the `sfdx-project.json` file before the final package build and promotion.

## Deprecating Components from an Unlocked Package

Unlocked packages provide traceability in the org by locking down the metadata components to the package that introduces it. This feature which is the main benefit of unlocked package can also create issues when you want to refactor components from one package to another. Let's look at some scenarios and common strategies that need to be applied

For a project that has two packages.

* **Package A** and **Package B**
* **Package B** is dependent on **Package A.**

### **Scenario 1:**

* Remove a component from **Package A**, provided the component has no dependency
* **Solution:** Create a new version of **Package A** with the metadata component being removed and install the package.

### **Scenario 2:**

* Move a metadata component from **Package A** to **Package B**
* **Solution:** This scenario is pretty straight forward, one can remove the metadata component from **Package A** and move that to **Package B**. When a new version of **Package A** gets installed, the following things happen:
  * If the deployment of the unlocked package is set to mixed, and no other metadata component is dependent on the component, the component gets [deleted](https://help.salesforce.com/articleView?id=sf.fields\_managing\_deleted\_fields.htm\&type=5).
  * On the subsequent install of **Package B**, **Package B** restores the field and takes ownership of the component.

### **Scenario 3:**

* Move a metadata component from **Package B** to **Package A**, where the component currently has other dependencies in **Package B**
* **Solution:** In this scenario, one can move the component to **Package A** and get the packages built. However during deployment to an org, **Package A** will fail with an error this component exists in **Package B**. To mitigate this one should do the following:
  * Deploy a version of **Package B** which removes the lock on the metadata component using deprecate mode. Some times this needs extensive refactoring to other components to break the dependencies. So evaluate whether the approach will work.
  * If not, you can go to the **UI** (**Setup > Packaging > Installed Packages > \<Name of Package> > View Components and Remove**) and remove the lock for a package.

## Managing Package Dependencies

Package dependencies are defined in the sfdx-project.json. More information on defining package dependencies can be found in the Salesforce [docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev2gp\_config\_file.htm).

```javascript
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
    }
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

* There are two unlocked packages
  * Expense Manager - Util is an unlocked package in your DevHub, identifiable by 0H in the packageAlias
  * Expense Manager - another unlocked package which is dependent on ' Expense Manager - Util', 'TriggerFramework' and 'External Apex Library - 1.0.0.4'
* External Apex Library is an external dependency, it could be a managed package or any unlocked package released on a different Dev Hub. All external package dependencies must be defined with a 04t ID, which can be determined from the installation URL from AppExchange or by contacting your vendor.

## Build Options with Unlocked Packages

Unlocked packages have two build modes, one with [skip dependency check](https://github.com/dxatscale/dxatscale-guide/blob/april-22/development-practices/types-of-packaging/broken-reference/README.md) and one without. A package being built without skipping dependency check cant be deployed into production and can usually take a long time to build. sfp cli tries to build packages in parallel understanding your dependency, however some of your packages could spend a significant time in validation.

During these situations, we ask you to consider whether the time taken to build all validated packages on an average is within your build budget, If not, here are your options

* [Move to org dependent package](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_unlocked\_pkg\_org\_dependent.htm): Org-dependent unlocked packages are a variant of unlocked packages. Org-dependent packages do not validate the dependencies of a package and will be faster. However please note that all the org's where the earlier unlocked package was installed, had to be deprecated and the component locks removed, before the new org-dependent unlocked package is installed.
* [Move to source-package:](https://github.com/dxatscale/dxatscale-guide/blob/april-22/development-practices/types-of-packaging/broken-reference/README.md) Use it as the least resort, source packages have a fairly loose lifecycle management.

## Handling metadata that is not supported by unlocked packages

Create a [source package](https://github.com/dxatscale/dxatscale-guide/blob/april-22/development-practices/types-of-packaging/broken-reference/README.md) and move the metadata and any associated dependencies over to that particular package.

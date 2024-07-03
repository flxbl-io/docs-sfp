# Source Packages

Source Packages is an sfp cli feature that mimics the behaviour of native unlocked packages.

Source Packages are metadata deployments from a Salesforce perspective, it is a group of components that are deployed to an org. Unlocked packages are a **First Class** Salesforce deployment construct, where the lifecycle is governed by the org, such as deleting/deprecating metadata and validating versions.

We always recommend using unlocked packages over source packages whenever you can. As a matter of preference, this is our priority of package types

1. [Unlocked Packages](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_unlocked\_pkg\_intro.htm)
2. [Unlocked Packages (Org-Dependent)](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_unlocked\_pkg\_org\_dependent.htm)
3. Source Packages

Source Packages are typically used for application-config (configuring an application delivered by a managed package such as changes to help text/description of fields ) or when you come across these constraints

* [Metadata not supported by Unlocked Packages](https://developer.salesforce.com/docs/metadata-coverage)
* Facing bugs while deploying the metadata using unlocked packages
* Unlocked Package validation takes too long (still we recommend go org-dependent)
* Dealing with metadata that is global or org-specific in nature (such as queues, profiles, etc or composite UI layouts, which doesn't make sense to be packaged using unlocked package)
* Development teams who are starting to adopt package-based development and want to organize their metadata

## **A Salesforce Org composed only of source packages**

A Salesforce Org can be composed only with source packages, however the lack of dependency validation as in unlocked packages, lack of automated destruction of changes can make it a bit challenging. As salesforce org is not aware of the source package,  there is no component lock ,  so another source package with same metadata component can alwasy overwrite a metadata component deployed by another package. For these, reasons, we always recommend you  prefer unlocked package or its variant org dependent unlocked packages.

An example of a common metadata component that typically gets overridden is [Custom Labels](https://developer.salesforce.com/docs/atlas.en-us.api\_meta.meta/api\_meta/meta\_customlabels.htm) which can span across multiple packages.



## Dependency Management for source packages

**Source packages can depend on other unlocked packages or managed packages**, however dependencies of source packages are validated during deployment time, ie., source packages assume that dependent metadata is already there in your org before the metadata in the source package is being deployed. That being said, for purposes of development in scratch org, you could add '**unlocked/managed package'** dependencies to a source package, so sfp cli commands like prepare and validate (in sfpowerscripts:orchestrator) will install the dependencies to the target org before proceedint to install source package

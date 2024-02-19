# Package vs Artifacts

In Salesforce, a **package** is a container that groups together metadata and code in order to facilitate the deployment and distribution of customizations and apps. Salesforce supports different types of packages, such as:

* **Unlocked Packages:** These are more modular and flexible than traditional managed packages, allowing developers to group and release customizations independently. They are version-controlled, upgradeable, and can include dependencies on other packages.
* **Org-Dependent Unlocked Packages:** Similar to unlocked packages but with a dependency on the metadata of the target org, making them less portable but useful for specific org customizations.
* **Managed Packages**: Managed packages are a type of Salesforce package primarily used by ISVs (Independent Software Vendors) for distributing and selling applications on the Salesforce AppExchange. They are fully encapsulated, which means the underlying code and metadata are not accessible or editable by the installing organization. Managed packages support versioning, dependency management, and can enforce licensing. They are ideal for creating applications that need to be securely distributed and updated across multiple Salesforce orgs without exposing proprietary code.

sfp  auguments the above formal salesforce package types with additional package types such as below

* **Source Packages:** These are not formal packages in Salesforce but refer to a collection of metadata and code retrieved from a Salesforce org or version control that can be deployed to an org but aren't versioned or managed as a single entity.
* **Diff Packages:** These are not a formal type of Salesforce package but refer to packages created by determining the differences (diff) between two sets of metadata, often used for deploying specific changes.

In the context of **sfp**, an **artifact** represents a more enhanced concept compared to a Salesforce package. While it is based on a Salesforce package (of any type mentioned above), an artifact in sfp includes additional attributes and metadata that describe the package version, dependencies, installation behavior, and other context-specific information relevant to the CI/CD process. Artifacts in sfp are designed to be more than just a bundle of code and metadata; they encapsulate the package along with its CI/CD lifecycle information, making them more aligned with DevOps practices.

Key differences between Salesforce packages and sfp artifacts include:

* **Versioning and Dependencies:** While Salesforce packages support versioning, sfp artifacts enrich this with detailed dependency tracking, ensuring that the CI/CD pipeline respects the order of package installations based on dependencies.
* **Installation Behavior:** Artifacts in sfp carries additional metadata that defines custom installation behaviors, such as pre- and post-installation scripts or conditional installation steps, which are not inherently a part of Salesforce packages.
* **CI/CD Integration:** Artifacts in sfp are specifically designed to fit into a CI/CD pipeline, such as supporting storing in an artifact registory, version tracking, and release management that are essential for automated deployments but are outside the scope of Salesforce packages themselves.


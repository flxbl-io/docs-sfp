# API Version

When encountering an issue where the `build` command builds an artifact that is incompatible with the target Salesforce environment, it's important to understand the role of the `sourceApiVersion` in the `sfdx-project.json` file and other factors that might influence the API version used during the build.

### Understanding the sourceApiVersion

The `sourceApiVersion` in your `sfdx-project.json` file is intended to specify the default API version for your Salesforce DX project. This version is used when creating scratch orgs and can influence the API version used for package builds. However, if your build results in a package versioned at 59.0 while your `sourceApiVersion` is set to v55.0, this need to be resolved

### Resolving the Version Mismatch

To resolve the version mismatch and ensure your package is built with a compatible API version for your  sandbox, consider the following steps:

* **Review and Align Configuration Files**: Double-check your `sfdx-project.json` and any other configuration files involved in the build process. Make sure the `sourceApiVersion` is consistently set to the intended version across all configurations.
* **Check CLI and Plugin Versions**: Ensure `sfp` cli is up-to-date. Review the documentation or release notes for these tools to understand their behavior regarding API version selection.
* **Specify API Version in Build Command**: If possible, use command-line options to explicitly specify the API version when issuing the build command. For example, some CLI commands allow you to set the API version directly as an argument.

By understanding the factors that influence the API version used during the package build process and taking proactive steps to align your project's configuration with your target environment, you can avoid version compatibility issues and ensure smooth deployments.

\

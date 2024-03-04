# Overview

**sfp** is a purpose-built cli-based tool for modular Salesforce development and release management. **sfp** aims to streamline and automate the build, test, and deployment processes of Salesforce metadata,  code, and data. It extends **sf cli** functionalities, focusing on artifact-driven development to enhance DevOps practices within Salesforce projects.

### Key Aspects of sfp:

* **Artifact-Centric Approach**: **sfp** packages Salesforce code and metadata into artifacts and deployment details, ensuring consistent deployments and simplified version management across environments.
* **Best-in-Class Mono Repo Support**: Offers robust support for mono repositories, facilitating streamlined development, integration, and collaboration.&#x20;
* **Support for Multiple Package Types**: **sfp** accommodates various Salesforce package types with streamlined commands, enabling modular development, independent versioning, and flexible deployment strategies.
* **Orchestrate Across Entire Lifecycle: sfp** provides an extensive set of functionality across the entire lifecycle of your Salesforce development.
* **End-to-End Observability**: **sfp** is built with comprehensive metrics emitted on every command, providing unparalleled visibility into your ALM process.



<div data-full-width="true">

<figure><img src=".gitbook/assets/concept (2).png" alt=""><figcaption></figcaption></figure>

</div>

## Commands

**sfp** incorporates a suite of commands to aid in your end-to-end development cycle for Salesforce. Starting with the core commands, you can perform basic workflows to build and deploy artifacts (locally to start and to an NPM artifact repository after) across environments through the command line. As you get comfortable with the core commands, you can utilize more advanced commands and flags in your CI/CD platform to drive a complete release process, leveraging release definitions, change logs, metrics, and more. &#x20;

**sfp** is constantly evolving and driven by the passionate community that has embraced our work methods. Over the years, we have introduced utility commands to solve pain points specific to the Salesforce Platform. The commands have been successfully tested and used on large enterprise-scale implementations. As we continue to grow the toolset, we hope to introduce more commands to address the future wave of challenges.&#x20;

Below is a high-level snapshot of the main topics and commands of sfp.

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption><p>sfp command groupings</p></figcaption></figure>

##

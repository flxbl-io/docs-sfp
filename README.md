# Overview

**sfp** is a purpose-built CLI tool for modular Salesforce development and release management. **sfp** streamlines and automates the build, test, and deployment processes of Salesforce metadata, code, and data. It extends **sf cli** functionalities, focusing on artifact-driven development to support #flxbl Salesforce project development.

**sfp** is available in two editions:
- **sfp (Community Edition)**: Open-source CLI with core build, deploy, and orchestration capabilities
- **sfp-pro**: Enterprise edition with additional features including a centralized server, environment management, authentication services, and team collaboration tools

## Key Aspects of sfp:

* **Built with codified process**: sfp is derived from extensive experience in modular Salesforce implementations. By embracing the #FLXBL framework, it streamlines the process of creating a well-architected, composable Salesforce Org, eliminating time-consuming efforts usually spent on re-inventing fundamental processes.
* **Artifact-Centric Approach**: **sfp** packages Salesforce code and metadata into artifacts with deployment details, ensuring consistent deployments and simplified version management across environments.
* **Best-in-Class Mono Repo Support**: Offers robust support for mono repositories, facilitating streamlined development, integration, and collaboration.
* **Support for Multiple Package Types**: **sfp** accommodates various Salesforce package types with streamlined commands, enabling modular development, independent versioning, and flexible deployment strategies.
* **Orchestrate Across Entire Lifecycle**: **sfp** provides an extensive set of functionality across the entire lifecycle of your Salesforce development.
* **End-to-End Observability**: **sfp** is built with comprehensive metrics emitted on every command, providing unparalleled visibility into your ALM process.
* **Centralized Server (sfp-pro)**: A backend server that provides environment management, authentication, webhooks, and API access for team-based workflows.

<div data-full-width="false">

<figure><img src=".gitbook/assets/concept (2).png" alt=""><figcaption><p>A Salesforce Project (in a git repository)</p></figcaption></figure>

</div>

## Commands

**sfp** incorporates a suite of commands to aid in your end-to-end development cycle for Salesforce. Starting with the core commands, you can perform basic workflows to build and deploy artifacts across environments through the command line. As you get comfortable with the core commands, you can utilize more advanced commands and flags in your CI/CD platform to drive a complete release process, leveraging release definitions, changelogs, metrics, and more.

**sfp** is constantly evolving and driven by the passionate community that has embraced our work methods. Over the years, we have introduced utility commands to solve pain points specific to the Salesforce Platform. The commands have been successfully tested and used on large-scale enterprise implementations.

Below is a high-level snapshot of the main command categories in sfp.

| Core | Advanced | Server (sfp-pro) | Utilities |
|------|----------|------------------|-----------|
| quickbuild | validate | init | profile |
| build | pool | start / stop / status | apextests |
| install | release | health / logs / scale | dependency |
| publish | releasedefinition | auth | impact |
| artifacts | changelog | environment | repo |
|  | metrics | org | flow |
|  |  | pool | |
|  |  | webhook | |
|  |  | repository | |
|  |  | review-envs | |
|  |  | key-value | |
|  |  | doc-store | |

##

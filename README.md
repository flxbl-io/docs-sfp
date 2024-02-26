# Overview

**sfp** is an purpose built  cli based tool specifically designed for modular Salesforce development and release management.   **sfp** is aimed at streamlining and automating the build, test, and deployment processes of Salesforce metadata,  code and data. It extends **sf cli** functionalities, focusing on artifact-driven development to enhance DevOps practices within Salesforce projects.

### Key Aspects of sfp:

* **Artifact-Centric Approach**: **sfp** packages Salesforce code and metadata into artifacts, along with deployment details, ensuring consistent deployments and simplified version management across environments.
* **Best-in-Class Mono Repo Support**: Offers robust support for mono repositories, facilitating streamlined development, integration, and collaboration&#x20;
* **Support for Multiple Package Types**: **sfp** accommodates various Salesforce package types with streamlined commands, enabling modular development, independent versioning, and flexible deployment strategies.
* **Orchestrate Across Entire Lifecycle:  sfp** provides an extensive set of functionality across the entire lifecycle of your Salesforce development.
* **End-to-End Observability**:  sfp is built with comprehensive metrics that are emitted on every commands providing unparalleled visibility into your ALM process.

## Commands

sfp is comprised of a suite of commands to aid in your end to end development cycle for Salesforce.  Starting with the core commands, you are able to perform  basic work flows to to build and deploy artifacts (locally to start, and to a NPM artifact repository after) across environments through the command line.  As you progress in your understanding of the core commands, you can utilized more advanced commands and flags in your CI/CD platform of choice to drive a more complete release process leveraging release definitions, change logs, metrics and much more. &#x20;

sfp is constantly evolving and being driven by the passionate community that has embraced our ways of working.  We have introduced key utility commands over the years to solve pain points specific to the Salesforce Platform.  The commands have been successfully tested and used on large enterprise-scale implementations.  As we continue to grow the toolset, we hope to introduced more commands to address the future wave of challenges.&#x20;

Below is a high level snapshot of the key topics and commands of sfp.

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption><p>sfp commands</p></figcaption></figure>

## Basic Flow

The following diagram depicts the basic flow of the development and test process, building artifacts, and deploying to target environments.

Once you have mastered the basic workflow, you can progress to publishing artifacts to a NPM Repository that will store immutable, versions of the metadata and code needed to drive the release of the components to your targeted Salesforce environments.

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption><p>sfp Develop, Test and Release Flow</p></figcaption></figure>

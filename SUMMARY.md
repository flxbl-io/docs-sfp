# Table of contents

* [ToDo List](README.md)
* [Overview](overview.md)
* [Concepts to Know](concepts-to-know.md)
* [SF CLI vs. SFP](sf-cli-vs.-sfp.md)

## Getting Started

* [Pre-Requisites](getting-started/pre-requisites.md)
* [Install sfp](getting-started/install-sfp.md)
* [Create Salesforce Orgs](getting-started/create-salesforce-orgs.md)
* [Fork Sample Repository](getting-started/fork-sample-repository.md)
* [NPM Registry Setup](getting-started/npm-registry-setup.md)
* [Building Artifacts](getting-started/building-artifacts.md)
* [Deploying Artifacts](getting-started/deploying-artifacts.md)
* [Publishing Artifacts](getting-started/publishing-artifacts.md)

## Deep-Dive

* [Supported package types](deep-dive/supported-package-types/README.md)
  * [Unlocked Packages](deep-dive/supported-package-types/unlocked-packages.md)
  * [Source Packages](deep-dive/supported-package-types/source-packages.md)
  * [Diff Package](deep-dive/supported-package-types/diff-package.md)
  * [Data Packages](deep-dive/supported-package-types/data-packages.md)
* [Creating a package](deep-dive/creating-a-package.md)
* [Artifacts](deep-dive/artifacts.md)
* [Package vs Artifacts](deep-dive/package-vs-artifacts.md)
* [Release Config](deep-dive/release-config.md)
* [Building Artifacts](deep-dive/build-artifacts.md)
  * [Identifying types of a package](deep-dive/identifying-types-of-a-package.md)
  * [Determining whether an artifact need to be built](deep-dive/building-artifacts/determining-whether-an-artifact-need-to-be-built.md)
  * [Configuring installation behaviour of an artifact](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/README.md)
    * [Always deploy an artifact](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/always-deploy-an-artifact.md)
    * [Optimized Installation](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/optimized-installation.md)
    * [Pre/Post Deployment Script](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/pre-post-deployment-script.md)
    * [PermissionSet Assignment](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/permissionset-assignment.md)
    * [Entitlement Deployment Helper](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/entitlement-deployment-helper.md)
    * [Declarative Field History Tracking](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/declarative-field-history-tracking.md)
    * [Reconciling Profiles](deep-dive/building-artifacts/configuring-installation-behaviour-of-an-artifact/reconciling-profiles.md)
  * [Controlling aspects of the build](deep-dive/building-artifacts/controlling-aspects-of-the-build.md)
  * [Limiting artifacts to be built](deep-dive/building-artifacts/limiting-artifacts-to-be-built.md)
  * [Building an artifact individually](deep-dive/building-artifacts/building-an-artifact-individually.md)
* [Deploying Artifacts](deep-dive/deploying-artifacts/README.md)
  * [Controlling Aspects of Deployment](deep-dive/deploying-artifacts/controlling-aspects-of-deployment.md)
  * [BuiltIn Deployment Helpers](deep-dive/deploying-artifacts/builtin-deployment-helpers/README.md)
    * [PermissionSet Group Awaiter](deep-dive/deploying-artifacts/builtin-deployment-helpers/permissionset-group-awaiter.md)
    * [Applying picklists](deep-dive/deploying-artifacts/builtin-deployment-helpers/applying-picklists.md)
* [Publish Artifact](deep-dive/publish-artifact.md)
* [Fetching Artifacts](deep-dive/fetching-artifacts.md)
* [Advanced Topics](deep-dive/advanced-topics/README.md)
  * [Release](deep-dive/advanced-topics/release/README.md)
    * [Release Definitions](deep-dive/advanced-topics/release/release-definitions.md)
    * [Change Logs](deep-dive/advanced-topics/release/change-logs.md)
  * [Validate](deep-dive/advanced-topics/validate.md)
  * [Metrics](deep-dive/advanced-topics/metrics.md)
  * [Pools](deep-dive/advanced-topics/pools.md)

## Command Guide

* [Core](command-guide/core/README.md)
  * [Orchestrator](command-guide/core/orchestrator.md)
  * [Artifacts](command-guide/core/artifacts.md)
  * [Package](command-guide/core/package.md)
* [Release](command-guide/release/README.md)
  * [Release Definition](command-guide/release/release-definition.md)
  * [Change Log](command-guide/release/change-log.md)
* [Utilities](command-guide/utilities/README.md)
  * [Apex Tests](command-guide/utilities/apex-tests.md)
  * [Dependency](command-guide/utilities/dependency.md)

## FAQs

* [General](faqs/general.md)
* [Concepts](faqs/concepts.md)
* [Technology Stack](faqs/technology-stack.md)
* [Common Errors](faqs/common-errors.md)

***

* [References](references.md)
* [Page](page.md)

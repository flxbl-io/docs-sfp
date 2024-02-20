# Table of contents

* [Overview](README.md)
* [SF CLI vs. SFP](sf-cli-vs.-sfp.md)

## Getting Started

* [Pre-Requisites](getting-started/pre-requisites.md)
* [Install sfp](getting-started/install-sfp.md)
* [Create Salesforce Orgs](getting-started/create-salesforce-orgs.md)
* [NPM Registry Setup](getting-started/npm-registry-setup.md)
* [Fork Sample Repository](getting-started/fork-sample-repository.md)
* [Basic lifecycle](getting-started/basic-lifecycle.md)

## CONCEPTS

* [Overview](concepts/overview.md)
* [Domains](concepts/domains.md)
* [Packages](concepts/packages.md)
* [Supported package types](concepts/supported-package-types/README.md)
  * [Unlocked Packages](concepts/supported-package-types/unlocked-packages.md)
  * [Org Dependent Unlocked Packages](concepts/supported-package-types/org-dependent-unlocked-packages.md)
  * [Source Packages](concepts/supported-package-types/source-packages.md)
  * [Diff Package](concepts/supported-package-types/diff-package.md)
  * [Data Packages](concepts/supported-package-types/data-packages.md)
* [Artifacts](concepts/artifacts.md)
* [Package vs Artifacts](concepts/package-vs-artifacts.md)
* [Creating a package](concepts/creating-a-package.md)
* [Identifying types of a package](concepts/identifying-types-of-a-package.md)
* [Release Config](concepts/release-config.md)

## BUILDing artifacts

* [Overview](building-artifacts/overview.md)
* [Determining whether an artifact need to be built](building-artifacts/determining-whether-an-artifact-need-to-be-built.md)
* [Controlling aspects of the build](building-artifacts/controlling-aspects-of-the-build.md)
* [Building an artifact individually](building-artifacts/building-an-artifact-individually.md)
* [Configuring installation behaviour of an artifact](building-artifacts/configuring-installation-behaviour-of-an-artifact/README.md)
  * [Always deploy an artifact](building-artifacts/configuring-installation-behaviour-of-an-artifact/always-deploy-an-artifact.md)
  * [Optimized Installation](building-artifacts/configuring-installation-behaviour-of-an-artifact/optimized-installation.md)
  * [Pre/Post Deployment Script](building-artifacts/configuring-installation-behaviour-of-an-artifact/pre-post-deployment-script.md)
  * [Reconciling Profiles](building-artifacts/configuring-installation-behaviour-of-an-artifact/reconciling-profiles.md)
  * [PermissionSet Assignment](building-artifacts/configuring-installation-behaviour-of-an-artifact/permissionset-assignment.md)
  * [Updating Picklist](building-artifacts/configuring-installation-behaviour-of-an-artifact/updating-picklist.md)
  * [Entitlement Deployment Helper](building-artifacts/configuring-installation-behaviour-of-an-artifact/entitlement-deployment-helper.md)
  * [Field History & Feed  Tracking](building-artifacts/configuring-installation-behaviour-of-an-artifact/field-history-and-feed-tracking.md)
* [Limiting artifacts to be built](building-artifacts/limiting-artifacts-to-be-built.md)

## Deploying artifacts

* [Overview](deploying-artifacts/overview.md)
* [Controlling Aspects of Deployment](deploying-artifacts/controlling-aspects-of-deployment.md)
* [Applying attributes of an artifact](deploying-artifacts/applying-attributes-of-an-artifact.md)
* [BuiltIn Deployment Helpers](deploying-artifacts/builtin-deployment-helpers/README.md)
  * [PermissionSet Group Awaiter](deploying-artifacts/builtin-deployment-helpers/permissionset-group-awaiter.md)

## publishing and fetching artifacts

* [Publish Artifact](publishing-and-fetching-artifacts/publish-artifact.md)
* [Fetching Artifacts](publishing-and-fetching-artifacts/fetching-artifacts.md)

## release management

* [Overview](release-management/overview.md)
* [Release Definitions](release-management/release-definitions.md)
* [Release](release-management/release/README.md)
  * [Change Logs](release-management/release/change-logs.md)

## Validating Artifacts

* [Overview](validating-artifacts/overview.md)

## Preparing environments

* [Overview](preparing-environments/overview.md)

## Metrics

* [Available metrics](metrics/available-metrics.md)

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

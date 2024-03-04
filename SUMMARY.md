# Table of contents

* [Overview](README.md)
* [SF CLI vs. SFP](sf-cli-vs.-sfp.md)

## Getting Started

* [Pre-Requisites](getting-started/pre-requisites.md)
* [Install sfp](getting-started/install-sfp.md)
* [Setup Salesforce Org](getting-started/setup-salesforce-org.md)
* [NPM Registry Setup](getting-started/npm-registry-setup.md)
* [Fork Sample Repository](getting-started/fork-sample-repository.md)

## CONCEPTS

* [Overview](concepts/overview.md)
* [Domains](concepts/domains.md)
* [Packages](concepts/packages.md)
* [Supported package types](concepts/supported-package-types/README.md)
  * [Unlocked Packages](concepts/supported-package-types/unlocked-packages.md)
  * [Org-Dependent Unlocked Packages](concepts/supported-package-types/org-dependent-unlocked-packages.md)
  * [Source Packages](concepts/supported-package-types/source-packages.md)
  * [Diff Package](concepts/supported-package-types/diff-package.md)
  * [Data Packages](concepts/supported-package-types/data-packages.md)
* [Artifacts](concepts/artifacts.md)
* [Package vs Artifacts](concepts/package-vs-artifacts.md)
* [Identifying types of a package](concepts/identifying-types-of-a-package.md)
* [Dependency management](concepts/dependency-management.md)
* [Transitive Dependency Resolution](concepts/transitive-dependency-resolution.md)

## configuing a project

* [Project structure](configuing-a-project/project-structure.md)
* [Defining a domain](configuing-a-project/defining-a-domain.md)
* [Creating a package](configuing-a-project/creating-a-package.md)

## BUILDing artifacts

* [Overview](building-artifacts/overview.md)
* [Determining whether an artifact need to be built](building-artifacts/determining-whether-an-artifact-need-to-be-built.md)
* [Controlling aspects of the build command](building-artifacts/controlling-aspects-of-the-build-command.md)
* [Building a domain](building-artifacts/building-a-domain.md)
* [Building an artifact for package individually](building-artifacts/building-an-artifact-for-package-individually.md)
* [Configuring installation behaviour of a package](building-artifacts/configuring-installation-behaviour-of-a-package/README.md)
  * [Always deploy a package](building-artifacts/configuring-installation-behaviour-of-a-package/always-deploy-a-package.md)
  * [Skip Deploy on Certain Orgs](building-artifacts/configuring-installation-behaviour-of-a-package/skip-deploy-on-certain-orgs.md)
  * [Optimized Installation](building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation.md)
  * [Pre/Post Deployment Script](building-artifacts/configuring-installation-behaviour-of-a-package/pre-post-deployment-script.md)
  * [Reconciling Profiles](building-artifacts/configuring-installation-behaviour-of-a-package/reconciling-profiles.md)
  * [PermissionSet Assignment](building-artifacts/configuring-installation-behaviour-of-a-package/permissionset-assignment.md)
  * [Updating Picklist](building-artifacts/configuring-installation-behaviour-of-a-package/updating-picklist.md)
  * [Entitlement Deployment Helper](building-artifacts/configuring-installation-behaviour-of-a-package/entitlement-deployment-helper.md)
  * [Field History & Feed  Tracking](building-artifacts/configuring-installation-behaviour-of-a-package/field-history-and-feed-tracking.md)
  * [State management for Flows](building-artifacts/configuring-installation-behaviour-of-a-package/state-management-for-flows.md)
* [Release Config](concepts/release-config.md)
* [Limiting artifacts to be built](building-artifacts/limiting-artifacts-to-be-built.md)

## Installing an artifact

* [Overview](installing-an-artifact/overview.md)
* [Controlling Aspects of Deployment](installing-an-artifact/controlling-aspects-of-deployment.md)
* [Applying attributes of an artifact](installing-an-artifact/applying-attributes-of-an-artifact.md)
* [BuiltIn Deployment Helpers](installing-an-artifact/builtin-deployment-helpers/README.md)
  * [PermissionSet Group Awaiter](installing-an-artifact/builtin-deployment-helpers/permissionset-group-awaiter.md)

## publishing and fetching artifacts

* [Publish Artifact](publishing-and-fetching-artifacts/publish-artifact.md)
* [Fetching Artifacts](publishing-and-fetching-artifacts/fetching-artifacts.md)

## Releasing artifacts

* [Overview](releasing-artifacts/overview.md)
* [Release Definitions](releasing-artifacts/release-definitions.md)
* [Generating a release definition](releasing-artifacts/generating-a-release-definition.md)
* [Generating a changelog](releasing-artifacts/generating-a-changelog.md)

## Validating a change

* [Overview](validating-a-change/overview.md)
* [Different types of validation](validating-a-change/different-types-of-validation.md)
* [Limiting Validation by Domain](validating-a-change/limiting-validation-by-domain.md)

## POOLS

* [Overview](pools/overview.md)
* [Defining a pool](pools/defining-a-pool.md)
* [Pool Operations](pools/pool-operations/README.md)
  * [Preparing pools](pools/pool-operations/preparing-pools/README.md)
    * [Handling dependencies](pools/pool-operations/preparing-pools/handling-dependencies.md)
  * [List Scratch Orgs in a pool](pools/pool-operations/list-scratch-orgs-in-a-pool.md)
  * [Fetch a scratch org](pools/pool-operations/fetch-a-scratch-org.md)
  * [Delete Pools](pools/pool-operations/delete-pools.md)

## Command Guide

* [Core](command-guide/core/README.md)
  * [Build](command-guide/core/build.md)
  * [Quickbuild](command-guide/core/quickbuild.md)
  * [Publish](command-guide/core/publish.md)
  * [Install](command-guide/core/install.md)
  * [Release](command-guide/core/release.md)
* [Advanced](command-guide/advanced/README.md)
  * [Validate](command-guide/advanced/validate.md)
  * [Artifacts](command-guide/advanced/artifacts.md)
  * [Changelog](command-guide/advanced/changelog.md)
  * [Impact](command-guide/advanced/impact.md)
  * [Pool](command-guide/advanced/pool.md)
  * [Metrics](command-guide/advanced/metrics.md)
  * [Repo](command-guide/advanced/repo.md)
* [Utilities](command-guide/utilities/README.md)
  * [Apex Tests](command-guide/utilities/apex-tests.md)
  * [Flow](command-guide/utilities/flow.md)
  * [Dependency](command-guide/utilities/dependency.md)
  * [Profile](command-guide/utilities/profile.md)

## FAQs

* [Common Errors](faqs/common-errors.md)

***

* [References](references.md)

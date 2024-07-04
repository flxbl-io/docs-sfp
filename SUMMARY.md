# Table of contents

* [Overview](README.md)

## Getting Started

* [Pre-Requisites](getting-started/pre-requisites.md)
* [Install sfp](getting-started/install-sfp.md)
* [Configure Your Project](getting-started/setup-source-project.md)
* [Build & Install an Artifact](getting-started/build-and-install-an-artifact.md)
* [Congratulations!](getting-started/congratulations.md)

## Transitioning from DX@Scale

* [CLI](transitioning-from-dx-scale/cli.md)
* [Unlocked Packages](transitioning-from-dx-scale/unlocked-packages.md)
* [Commands](transitioning-from-dx-scale/commands.md)
* [Docker Images](transitioning-from-dx-scale/docker-images.md)
* [Docs](transitioning-from-dx-scale/docs.md)

## CONCEPTS

* [Overview](concepts/overview.md)
* [SF CLI vs. SFP](sf-cli-vs.-sfp.md)
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
* [Destructive Changes](concepts/destructive-changes.md)

## configuring a project

* [Project structure](configuring-a-project/project-structure.md)
* [Setup Salesforce Org](getting-started/setup-salesforce-org.md)
* [Creating a package](configuring-a-project/creating-a-package.md)
* [Defining a domain](configuring-a-project/defining-a-domain.md)
* [Release Config](configuring-a-project/release-config.md)

## BUILDING ARTIFACTS

* [Overview](building-artifacts/overview.md)
* [Determining whether an artifact need to be built](building-artifacts/determining-whether-an-artifact-need-to-be-built.md)
* [Building a domain](building-artifacts/building-a-domain.md)
* [Building an artifact for package individually](building-artifacts/building-an-artifact-for-package-individually.md)
* [Limiting artifacts to be built](building-artifacts/limiting-artifacts-to-be-built.md)
* [Controlling aspects of the build command](building-artifacts/controlling-aspects-of-the-build-command/README.md)
  * [Ignoring packages from being built](building-artifacts/controlling-aspects-of-the-build-command/ignoring-packages-from-being-built.md)
  * [Building a collection of packages together](building-artifacts/controlling-aspects-of-the-build-command/building-a-collection-of-packages-together.md)
  * [Selective ignoring of components from being built](building-artifacts/controlling-aspects-of-the-build-command/selective-ignoring-of-components-from-being-built.md)
  * [Use of multiple config file in build command](building-artifacts/controlling-aspects-of-the-build-command/use-of-multiple-config-file-in-build-command.md)
* [Configuring installation behaviour of a package](building-artifacts/configuring-installation-behaviour-of-a-package/README.md)
  * [Always deploy a package](building-artifacts/configuring-installation-behaviour-of-a-package/always-deploy-a-package.md)
  * [Skip Install on Certain Orgs](building-artifacts/configuring-installation-behaviour-of-a-package/skip-install-on-certain-orgs.md)
  * [Optimized Installation](building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation.md)
  * [Pre/Post Deployment Script](building-artifacts/configuring-installation-behaviour-of-a-package/pre-post-deployment-script.md)
  * [Reconciling Profiles](building-artifacts/configuring-installation-behaviour-of-a-package/reconciling-profiles.md)
  * [PermissionSet Assignment](building-artifacts/configuring-installation-behaviour-of-a-package/permissionset-assignment.md)
  * [Updating Picklist](building-artifacts/configuring-installation-behaviour-of-a-package/updating-picklist.md)
  * [Entitlement Deployment Helper](building-artifacts/configuring-installation-behaviour-of-a-package/entitlement-deployment-helper.md)
  * [Field History & Feed  Tracking](building-artifacts/configuring-installation-behaviour-of-a-package/field-history-and-feed-tracking.md)
  * [Aliasfy Packages](building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages.md)
  * [State management for Flows](building-artifacts/configuring-installation-behaviour-of-a-package/state-management-for-flows.md)

## Installing an artifact

* [Overview](installing-an-artifact/overview.md)
* [Controlling Aspects of Installation](installing-an-artifact/controlling-aspects-of-installation.md)
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
* [Controlling validation attributes of a package](validating-a-change/controlling-validation-attributes-of-a-package/README.md)
  * [Skip Testing](validating-a-change/controlling-validation-attributes-of-a-package/skip-testing.md)
  * [Skip Coverage Validation](validating-a-change/controlling-validation-attributes-of-a-package/skip-coverage-validation.md)
  * [Test Synchronously](validating-a-change/controlling-validation-attributes-of-a-package/test-synchronously.md)

## POOLS

* [Overview](pools/overview.md)
* [Setting up your Salesforce Org for Scratch Org Pools](pools/setting-up-your-salesforce-org-for-scratch-org-pools.md)
* [Defining a pool](pools/defining-a-pool.md)
* [Pool Operations](pools/pool-operations/README.md)
  * [Preparing pools](pools/pool-operations/preparing-pools/README.md)
    * [Handling dependencies](pools/pool-operations/preparing-pools/handling-dependencies.md)
  * [List Scratch Orgs in a pool](pools/pool-operations/list-scratch-orgs-in-a-pool.md)
  * [Fetch a scratch org](pools/pool-operations/fetch-a-scratch-org.md)
  * [Delete Pools](pools/pool-operations/delete-pools.md)

## Metrics

* [Available Metrics](metrics/available-metrics.md)
* [Custom Metrics](metrics/custom-metrics.md)
* [Configuring Collectors](metrics/configuring-collectors/README.md)
  * [Datadog](metrics/configuring-collectors/datadog.md)
  * [Splunk](metrics/configuring-collectors/splunk.md)
  * [New Relic](metrics/configuring-collectors/new-relic.md)
  * [StatsD](metrics/configuring-collectors/statsd.md)

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
* [Common Questions](faqs/common-questions/README.md)
  * [Dealing with Long Build Times in Salesforce](faqs/common-questions/dealing-with-long-build-times-in-salesforce.md)
  * [Standard ValueSets and unlocked packages](faqs/common-questions/standard-valuesets-and-unlocked-packages.md)
  * [Common Issues encountered with aliasfied packages](faqs/common-questions/common-issues-encountered-with-aliasfied-packages.md)
  * [Org Shapes](faqs/common-questions/org-shapes.md)
  * [API Version](faqs/common-questions/api-version.md)

***

* [References](references.md)

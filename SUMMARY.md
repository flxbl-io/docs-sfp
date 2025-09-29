# Table of contents

* [Overview](README.md)

## Getting Started

* [Pre-Requisites](getting-started/pre-requisites.md)
* [Install sfp](getting-started/install-sfp/README.md)
  * [Install sfp community edition](getting-started/install-sfp/install-sfp-community-edition.md)
  * [Install sfp-pro](getting-started/install-sfp/install-sfp-pro.md)
* [Configure Your Project](getting-started/setup-source-project.md)
* [Build & Install an Artifact](getting-started/build-and-install-an-artifact.md)
* [Configuring LLM Providers](getting-started/configuring-llm-providers.md)
* [Congratulations!](getting-started/congratulations.md)
* [Docker Images](getting-started/docker-images/README.md)
  * [sfp-pro](getting-started/docker-images/sfp-pro/README.md)
    * [Automated Image Synchronization to Your Registry](getting-started/docker-images/sfp-pro/migrating-to-sfp-pro.md)
    * [Migrating from sfp community edition to sfp pro edition](getting-started/docker-images/sfp-pro/migrating-from-sfp-community-edition-to-sfp-pro-edition.md)

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

## Development

* [Development Workflow](development/development-workflow.md)
* [Development Environment](development/development-environment.md)
* [Pull Changes from your org](development/pull-changes-from-your-org.md)
* [Push Changes to your org](development/push-changes-to-your-org.md)
* [String Replacements](development/string-replacements.md)
* [Creating a package](configuring-a-project/creating-a-package.md)
* [Defining a domain](configuring-a-project/defining-a-domain.md)
  * [Release Config](configuring-a-project/release-config.md)
* [Dependency Management](development/dependency-management/README.md)
  * [Expand Dependencies](development/dependency-management/expand-dependencies.md)
  * [Shrink Dependencies](development/dependency-management/shrink-dependencies.md)
  * [Explain Dependencies](development/dependency-management/explain-dependencies.md)

## Analysing a Project

* [Overview](analysing-a-project/overview.md)
* [Duplicate Check](analysing-a-project/duplicate-check.md)
* [Compliance Check](analysing-a-project/compliance-check.md)
* [AI-Powered PR Linter](analysing-a-project/ai-pr-linter.md)
* [AI Assisted Insight Report](analysing-a-project/ai-powered-report.md)

## Validating a change

* [Overview](validating-a-change/overview.md)
* [Different types of validation](validating-a-change/different-types-of-validation.md)
* [AI-Assisted Error Analysis](validating-a-change/ai-assisted-error-analysis.md)
* [Limiting Validation by Domain](validating-a-change/limiting-validation-by-domain.md)
* [Validation Scripts](validating-a-change/validation-scripts.md)
* [Controlling validation attributes of a package](validating-a-change/controlling-validation-attributes-of-a-package/README.md)
  * [Skip Testing](validating-a-change/controlling-validation-attributes-of-a-package/skip-testing.md)
  * [Skip Coverage Validation](validating-a-change/controlling-validation-attributes-of-a-package/skip-coverage-validation.md)
  * [Test Synchronously](validating-a-change/controlling-validation-attributes-of-a-package/test-synchronously.md)

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
  * [Field History & Feed Tracking](building-artifacts/configuring-installation-behaviour-of-a-package/field-history-and-feed-tracking.md)
  * [Aliasfy Packages](building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/README.md)
    * [Aliasfy Packages - Merge Mode](building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/aliasfy-packages-merge-mode.md)
  * [String Replacements](building-artifacts/configuring-installation-behaviour-of-a-package/string-replacements.md)
  * [State management for Flows](building-artifacts/configuring-installation-behaviour-of-a-package/state-management-for-flows.md)

## Installing an artifact

* [Overview](installing-an-artifact/overview.md)
* [Controlling Aspects of Installation](installing-an-artifact/controlling-aspects-of-installation.md)
* [Applying attributes of an artifact](installing-an-artifact/applying-attributes-of-an-artifact.md)
* [String Replacements During Install](installing-an-artifact/string-replacements-during-install.md)
* [BuiltIn Deployment Helpers](installing-an-artifact/builtin-deployment-helpers/README.md)
  * [PermissionSet Group Awaiter](installing-an-artifact/builtin-deployment-helpers/permissionset-group-awaiter.md)

## publishing and fetching artifacts

* [Publish Artifact](publishing-and-fetching-artifacts/publish-artifact.md)
* [Fetching Artifacts](publishing-and-fetching-artifacts/fetching-artifacts.md)

## Releasing artifacts

* [Overview](releasing-artifacts/overview.md)
* [Release Definitions](releasing-artifacts/release-definitions.md)
* [Generating a release definition](releasing-artifacts/generating-a-release-definition.md)
* [Patching Releases](releasing-artifacts/patching-releases.md)
* [Generating a changelog](releasing-artifacts/generating-a-changelog.md)

## Environment Management

* [Pools](environment-management/pools/README.md)
  * [Scratch Org Pools](environment-management/pools/scratch-org-pools/README.md)
    * [Defining a pool](pools/defining-a-pool.md)
    * [Setting up your Salesforce Org for Scratch Org Pools](pools/setting-up-your-salesforce-org-for-scratch-org-pools.md)
    * [Pool Operations](pools/pool-operations/README.md)
      * [Preparing pools](pools/pool-operations/preparing-pools/README.md)
        * [Handling dependencies](pools/pool-operations/preparing-pools/handling-dependencies.md)
      * [List Scratch Orgs in a pool](pools/pool-operations/list-scratch-orgs-in-a-pool.md)
      * [Fetch a scratch org](pools/pool-operations/fetch-a-scratch-org.md)
      * [Delete Pools](pools/pool-operations/delete-pools.md)
  * [Sandbox Pools](environment-management/pools/sandbox-pools/README.md)
    * [Sandbox Pool Initialization](environment-management/pools/sandbox-pools/sandbox-pool-initialization.md)
    * [Fetch a Sandbox from Pool](environment-management/pools/sandbox-pools/fetch-a-sandbox-from-pool.md)
    * [Monitor Sandbox Pools](environment-management/pools/sandbox-pools/monitor-sandbox-pools.md)
* [Review Environments](review-environments/overview.md)
  * [Commands](review-environments/commands/README.md)
    * [Fetch a Review Environment](review-environments/fetch-a-review-environment.md)
    * [Check Review Environment Status](review-environments/check-review-environment-status.md)
    * [Extend a Review Environment](review-environments/commands/extend-a-review-environment.md)
    * [Transition Review Environment Status](review-environments/commands/transition-review-environment-status.md)
    * [Unassign a Review Environment](review-environments/commands/unassign-a-review-environment.md)
  * [Considerations](review-environments/considerations.md)
* [Sandbox](environment-management/sandbox/README.md)
  * [Create Sandbox](environment-management/sandbox/create-sandbox.md)
  * [Delete Sandbox](environment-management/sandbox/delete-sandbox.md)
  * [List Sandbox](environment-management/sandbox/list-sandbox.md)
  * [Login to Sandbox](environment-management/sandbox/login-to-sandbox.md)
  * [Update Sandbox](environment-management/sandbox/update-sandbox.md)

***

* [API Reference](api-reference/README.md)
  * [Health](api-reference/health.md)
  * [Authentication](api-reference/authentication.md)
  * [Token](api-reference/token.md)
  * [Salesforce](api-reference/salesforce.md)
  * [Team](api-reference/team.md)
  * [Users](api-reference/users.md)
  * [Tasks](api-reference/tasks.md)
  * [Key Value](api-reference/key-value.md)
  * [Repository](api-reference/repository.md)
  * [WebHooks](api-reference/webhooks.md)

## Metrics

* [Available Metrics](metrics/available-metrics.md)
* [Custom Metrics](metrics/custom-metrics.md)
* [Configuring Collectors](metrics/configuring-collectors/README.md)
  * [Datadog](metrics/configuring-collectors/datadog.md)
  * [Splunk](metrics/configuring-collectors/splunk.md)
  * [New Relic](metrics/configuring-collectors/new-relic.md)
  * [StatsD](metrics/configuring-collectors/statsd.md)

## Helpers

* [Managing Shared Resources](helpers/managing-shared-resources.md)

## CLI REFERENCE

* [Core](cli-reference/core/README.md)
  * [Build](cli-reference/core/build.md)
  * [Quickbuild](cli-reference/core/quickbuild.md)
  * [Publish](cli-reference/core/publish.md)
  * [Install](cli-reference/core/install.md)
  * [Release](cli-reference/core/release.md)
* [Advanced](cli-reference/advanced/README.md)
  * [Validate](cli-reference/advanced/validate.md)
  * [Artifacts](cli-reference/advanced/artifacts.md)
  * [Changelog](cli-reference/advanced/changelog.md)
  * [Impact](cli-reference/advanced/impact.md)
  * [Pool](cli-reference/advanced/pool.md)
  * [Metrics](cli-reference/advanced/metrics.md)
  * [Repo](cli-reference/advanced/repo.md)
  * [Release Definition](cli-reference/advanced/releasedefinition.md)
* [Server](cli-reference/server/README.md)
  * [Init](cli-reference/server/init.md)
  * [Start](cli-reference/server/start.md)
  * [Stop](cli-reference/server/stop.md)
  * [Status](cli-reference/server/status.md)
  * [Update](cli-reference/server/update.md)
  * [Health](cli-reference/server/health.md)
  * [Logs](cli-reference/server/logs.md)
  * [Scale](cli-reference/server/scale.md)
  * [Application Token](cli-reference/server/application-token.md)
  * [Artifacts](cli-reference/server/artifacts.md)
  * [Auth](cli-reference/server/auth.md)
  * [Builds](cli-reference/server/builds.md)
  * [Doc Store](cli-reference/server/doc-store.md)
  * [Environment](cli-reference/server/environment.md)
  * [Key-Value](cli-reference/server/key-value.md)
  * [Org](cli-reference/server/org.md)
  * [Pool](cli-reference/server/pool.md)
  * [Project](cli-reference/server/project.md)
  * [Release Definition](cli-reference/server/releasedefinition.md)
  * [Repository](cli-reference/server/repository.md)
  * [Review Environments](cli-reference/server/review-envs.md)
  * [User](cli-reference/server/user.md)
  * [Webhook](cli-reference/server/webhook.md)
* [Utilities](cli-reference/utilities/README.md)
  * [Apex Tests](cli-reference/utilities/apex-tests.md)
  * [Flow](cli-reference/utilities/flow.md)
  * [Dependency](cli-reference/utilities/dependency.md)
  * [Profile](cli-reference/utilities/profile.md)

## FAQs

* [Common Errors](faqs/common-errors/README.md)
  * [Org Shapes](faqs/common-errors/org-shapes.md)
  * [Troubleshooting Unlocked Packages Build Failure Due to Code Coverage](faqs/common-errors/troubleshooting-unlocked-packages-build-failure-due-to-code-coverage.md)
* [Common Questions](faqs/common-questions/README.md)
  * [Auto-Enable Trace Logging in CI/CD](faqs/common-questions/auto-trace-logging.md)
  * [Email Templates Deployment: Classic vs Lightning](faqs/common-questions/email-templates-deployment-classic-vs-lightning.md)
  * [Dealing with Long Build Times in Salesforce](faqs/common-questions/dealing-with-long-build-times-in-salesforce.md)
  * [Standard ValueSets and unlocked packages](faqs/common-questions/standard-valuesets-and-unlocked-packages.md)
  * [Common Issues encountered with aliasfied packages](faqs/common-questions/common-issues-encountered-with-aliasfied-packages.md)
  * [API Version](faqs/common-questions/api-version.md)
  * [Understanding alwaysDeploy and skipIfAlreadyInstalled in Deployment Pipelines](faqs/common-questions/understanding-alwaysdeploy-and-skipifalreadyinstalled-in-deployment-pipelines.md)
* [sfp versioning and upgrade Process](faqs/sfp-versioning-and-upgrade-process.md)

***

* [References](references.md)
* [Legal](legal/README.md)
  * [Terms of Service for sfp](legal/terms-of-service-for-sfp.md)
  * [Terms of Service for 'sfp-pro' Software](legal/terms-of-service-for-sfp-pro-software.md)

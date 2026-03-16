---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/concepts/sf-cli-vs.-sfp
---

# SF CLI vs. SFP

## Salesforce CLI

The Salesforce CLI is a command-line interface that simplifies development and build automation when working with your Salesforce org. There are numerous of commands that the sf cli provides natively that is beyond the scope of this site and can be found on the official Salesforce Documentation Site.

From a build, test, and deployment perspective, the following diagram depicts the bare minimum commands necessary to get up and running in setting up your sf project, retrieving and deploying code to the target environments.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption><p>sf cli deployments</p></figcaption></figure>

## SFP

sfp is built on the [Open CLI Framework](https://oclif.io/) and leverages the same core Salesforce node libraries and APIs as the [@salesforce/cli](https://www.npmjs.com/package/@salesforce/cli). sfp is a standalone CLI — it is not a Salesforce CLI plugin. sfp releases are independently managed, and core npm libraries are updated as needed to ensure no breaking changes are introduced.

sfp works in conjunction with the **sfp server** to provide a complete Salesforce DevOps platform. Many sfp commands communicate with the server for operations like environment management, credential retrieval, pool orchestration, and task execution. The server handles long-running operations asynchronously, allowing the CLI to remain responsive.

The diagram below depicts the basic flow of the development and test process, building artifacts, and deploying to target environments.

Once you have mastered the basic workflow, you can progress to publishing artifacts to a NPM Repository that will store immutable, versions of the metadata and code used to drive the release of your packages across Salesforce Environments.

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>sfp Develop, Test, and Release Workflow</p></figcaption></figure>

## References

The list below is a curated list of core sf cli and Salesforce DX developer guides for your reference.

* SF CLI
  * [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
  * [Salesforce CLI Release Notes](https://github.com/forcedotcom/cli/blob/main/releasenotes/README.md)
  * [Salesforce CLI Status Page](https://github.com/salesforcecli/status)
  * [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
  * [Developing sf Plugins](https://github.com/salesforcecli/cli/wiki/Quick-Introduction-to-Developing-sf-Plugins)
  * [@salesforce/cli NPM Repository](https://www.npmjs.com/package/@salesforce/cli)

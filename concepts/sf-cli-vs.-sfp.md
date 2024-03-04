# SF CLI vs. SFP

## Salesforce CLI&#x20;

The Salesforce CLI is a command-line interface that simplifies development and build automation when working with your Salesforce org. There are numerous of commands that the sf cli provides natively that is beyond the scope of this site and can be found on the official Salesforce Documentation Site. &#x20;

From a build, test, and deployment perspective, the following diagram depicts the bare minimum commands necessary to get up and running in setting up your sf project, retrieving and deploying code to the target environments. &#x20;

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption><p>sf cli deployments</p></figcaption></figure>

## SFP

sfp is based on the [The Open CLI Framework ](https://oclif.io/)for building a command line interface (CLI) in [Node.js](https://nodejs.org/api/cli.html).   Instead of being a typical Salesforce CLI plugin, sfp is standalone and leverages the same core libraries and APIs as the [@salesforce/cli](https://www.npmjs.com/package/@salesforce/cli).  sfp releases are independently managed and as core npm libraries are stable, we will update them as needed to ensure no breaking changes are introduced. &#x20;

The diagram below depicts the basic flow of the development and test process, building artifacts, and deploying to target environments. &#x20;

Once you have mastered the basic workflow, you can progress to publishing artifacts to a NPM Repository that will store immutable, versions of the metadata and code used to drive the release of your packages across Salesforce Environments.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>sfp Develop, Test, and Release Workflow</p></figcaption></figure>

## References

The list below is a curated list of core sf cli and Salesforce DX developer guides for your reference.&#x20;

* SF CLI
  * [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_setup.meta/sfdx\_setup/sfdx\_setup\_intro.htm)
  * [Salesforce CLI Release Notes](https://github.com/forcedotcom/cli/blob/main/releasenotes/README.md)
  * [Salesforce CLI Status Page](https://github.com/salesforcecli/status)
  * [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_intro.htm)
  * [Developing sf Plugins](https://github.com/salesforcecli/cli/wiki/Quick-Introduction-to-Developing-sf-Plugins)
  * [@salesforce/cli NPM Repository](https://www.npmjs.com/package/@salesforce/cli)
* sfp
  * [@flxblio/sfp NPM Repository](https://www.npmjs.com/package/@flxblio/sfp)
  * [GitHub Repository](https://github.com/flxbl-io/sfp)

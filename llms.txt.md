# LLMs.txt

## https://docs.flxbl.io/sfp llms-full.txt

### Salesforce Development Tool

**sfp** is a purpose-built cli-based tool for modular Salesforce development and release management. **sfp** aims to streamline and automate the build, test, and deployment processes of Salesforce metadata, code, and data. It extends **sf cli** functionalities, focusing on artifact-driven development to support #flxbl salesforce project development

### [Direct link to heading](https://docs.flxbl.io/sfp#key-aspects-of-sfp) Key Aspects of sfp:

* **Built with codified process**: sfp cli is derived from extensive experience in modular Salesforce implementations. By embracing the #FLXBL framework, it aims to streamline the process of creating a well-architected, composable Salesforce Org. sfp cli eliminates the need for time-consuming efforts usually spent on re-inventing fundamental processes, helping you achieve your objectives faster
* **Artifact-Centric Approach**: **sfp** packages Salesforce code and metadata into artifacts and deployment details, ensuring consistent deployments and simplified version management across environments.
* **Best-in-Class Mono Repo Support**: Offers robust support for mono repositories, facilitating streamlined development, integration, and collaboration.
* **Support for Multiple Package Types**: **sfp** accommodates various Salesforce package types with streamlined commands, enabling modular development, independent versioning, and flexible deployment strategies.
* **Orchestrate Across Entire Lifecycle: sfp** provides an extensive set of functionality across the entire lifecycle of your Salesforce development.
* **End-to-End Observability**: **sfp** is built with comprehensive metrics emitted on every command, providing unparalleled visibility into your ALM process.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F25NBdGeewEf4AOzWdrWP%252Fconcept.png%3Falt%3Dmedia%26token%3D52ba493c-594b-4f35-91ea-7ba2efa1788d\&width=768\&dpr=4\&quality=100\&sign=8084fde0\&sv=2)

A Salesforce Project (in a git repository)

### [Direct link to heading](https://docs.flxbl.io/sfp#commands) Commands

**sfp** incorporates a suite of commands to aid in your end-to-end development cycle for Salesforce. Starting with the core commands, you can perform basic workflows to build and deploy artifacts (locally to start and to a NPM artifact repository after) across environments through the command line. As you get comfortable with the core commands, you can utilize more advanced commands and flags in your CI/CD platform to drive a complete release process, leveraging release definitions, change logs, metrics, and more.

**sfp** is constantly evolving and driven by the passionate community that has embraced our work methods. Over the years, we have introduced utility commands to solve pain points specific to the Salesforce Platform. The commands have been successfully tested and used on large scale enterprise implementations. As we continue to grow the toolset, we hope to introduce more commands to address the future wave of challenges.

Below is a high-level snapshot of the main topics and commands of sfp.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FBxEUajmkhxDDFGN6FQwU%252Fimage.png%3Falt%3Dmedia%26token%3Db7d64af6-176f-488e-b579-045084c3ae95\&width=768\&dpr=4\&quality=100\&sign=a94d2126\&sv=2)

sfp command groupings

### [Direct link to heading](https://docs.flxbl.io/sfp#undefined)

[NextPre-Requisites](https://docs.flxbl.io/sfp/getting-started/pre-requisites)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### SFP Prerequisites

The following list of software is the minimum to get started using sfp. Our assumption is that you are familiar with the Salesforce CLI and are comfortable using VS Code. To keep the material simple, we will be assuming you already have access to a Salesforce Sandbox that you can build and install artifacts.

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/pre-requisites#salesforce) Salesforce

* Salesforce Org with [sfpowerscripts-artifact](https://docs.flxbl.io/sfp/configuring-a-project/setup-salesforce-org#id-2.-install-sfpowerscripts-artifact-unlocked-package) Unlocked Package installed.

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/pre-requisites#workstation) Workstation

* [git](https://git-scm.com/)
* [NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* [VS Code](https://code.visualstudio.com/)
* [sf CLI](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm)

### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/pre-requisites#undefined)

[PreviousOverview](https://docs.flxbl.io/sfp) [NextInstall sfp](https://docs.flxbl.io/sfp/getting-started/install-sfp)

Last updated 3 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Install sfp Editions

### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#install-sfp-community-edition) Install sfp community edition

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#a.-install-sfp-in-your-local-machine) A. Install sfp in your local machine

Copy

```inline-grid
npm i -g @flxbl-io/sfp
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#b.-check-sfp-version) B. Check sfp version

Copy

```inline-grid
sfp --version
@flxbl-io/sfp/37.0.0 darwin-arm64 node-v20.3.1
```

sfp requires node-gyp for its dependencies. If you are facing issues during installation, with node-gyp, please follow the instructions here [https://www.npmjs.com/package/node-gyp](https://www.npmjs.com/package/node-gyp)

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#c.-validate-installation) C. Validate Installation

Copy

```inline-grid
sfp commands

Command                         Summary
 ─────────────────────────────── ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 apextests trigger               Triggers Apex unit test in an org. Supports test level RunAllTestsInPackage, which optionally allows validation of individual class code coverage
 artifacts fetch                 Fetch sfp artifacts from a NPM compatible registry using a release definition file
 artifacts promote               Promotes artifacts predominantly for unlocked packages with code coverage greater than 75%
 artifacts query                 Fetch details about artifacts installed in a target org
 build                           Build artifact(s) of your packages in the current project
 ...
```

### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#install-sfp-pro) Install sfp - pro

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#a.-download-the-latest-release-from-source.flxbl.io) A. Download the latest release from source.flxbl.io

Head to [https://source.flxbl.io/flxbl/sfp-pro/releases](https://source.flxbl.io/flxbl/sfp-pro/releases) and download the .tgz file from the one of the releases

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F1OcbLl40rWUqHcHSW68N%252FCleanShot%25202024-10-21%2520at%252014.30.57.png%3Falt%3Dmedia%26token%3Dc79c72e7-0058-447a-aa80-ba202631df1f\&width=768\&dpr=4\&quality=100\&sign=49d7ccef\&sv=2)

Releases

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#b.-install-the-version-in-your-local-machine) B. Install the version in your local machine

Copy

```inline-grid
npm i -g <path-to-downloaded-file>
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#c.-check-sfp-version) C. Check sfp version

Copy

```inline-grid
sfp --version
@flxbl-io/sfp/43.1..0 darwin-arm64 node-v20.3.1
```

sfp-pro requires node-gyp for its dependencies. If you are facing issues during installation, with node-gyp, please follow the instructions here [https://www.npmjs.com/package/node-gyp](https://www.npmjs.com/package/node-gyp)

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/install-sfp#d.-validate-installation) D. Validate Installation

Copy

```inline-grid
sfp commands

Command                         Summary
 ─────────────────────────────── ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 apextests trigger               Triggers Apex unit test in an org. Supports test level RunAllTestsInPackage, which optionally allows validation of individual class code coverage
 artifacts fetch                 Fetch sfp artifacts from a NPM compatible registry using a release definition file
 artifacts promote               Promotes artifacts predominantly for unlocked packages with code coverage greater than 75%
 artifacts query                 Fetch details about artifacts installed in a target org
 build                           Build artifact(s) of your packages in the current project
 ...
```

[PreviousPre-Requisites](https://docs.flxbl.io/sfp/getting-started/pre-requisites) [NextConfigure Your Project](https://docs.flxbl.io/sfp/getting-started/setup-source-project)

Last updated 5 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce DX Setup Guide

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/setup-source-project#a.-ensure-you-have-enabled-and-authenticated-to-devhub) A. Ensure you have enabled and authenticated to DevHub

Ensure you have [enabled DevHub](https://docs.flxbl.io/sfp/configuring-a-project/setup-salesforce-org) in your production org or created a developer org.

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/setup-source-project#b.-authenticate-your-salesforce-cli-to-devhub) B. Authenticate your Salesforce CLI to DevHub

Ensure you have authenticated your local development environment to your DevHub. If you are not familiar with the process, you can follow the instructions provided by Salesforce [here](https://trailhead.salesforce.com/content/learn/projects/quick-start-salesforce-dx/set-up-your-salesforce-dx-environment).

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/setup-source-project#c.-add-additional-attributes-to-your-salesforce-dx-project) C. Add additional attributes to your Salesforce DX Project

sfp cli only works with a Salesforce DX project, with source format as described [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_source_file_format.htm) . If your project is not source format, you would need to convert into source format using the [options provided by salesforce cli.](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_create_from_existing.htm)

1. Navigate to your sfdx-project.json file and locate the `packageDirectories` property.

Copy

```inline-grid
{
"packageDirectories" : [\
    { "path": "force-app", "default": true},\
    { "path" : "unpackaged" },\
    { "path" : "utils" }\
  ],
"namespace": "",
"sfdcLoginUrl" : "https://login.salesforce.com",
"sourceApiVersion": "60.0"
}
```

In the above example, the package directories are

* `force-app`
* `unpackaged`
* `utils`

1. Add the following additional attributes to the sfdx-project.json and save.

* `package`
* `versionNumber`

Copy

```inline-grid
{
   "packageDirectories" : [\
    {\
       "package": "force-app",\
       "versionNumber": "1.0.0.NEXT",\
       "path": "force-app",\
       "default": true\
     },\
    { "path" : "unpackaged" },  // You can repeat the same for additonal directories\
    { "path" : "utils" }  // You can repeat the same for additonal directories\
   ],
"namespace": "",
"sfdcLoginUrl" : "https://login.salesforce.com",
"sourceApiVersion": "60.0"
}
```

Thats the minimal configuration required to run sfp on a project.

Move on to the next chapter to execute sfp commands in this directory.

For detailed configurations on this sfdx-project.json schema for sfp, click [here](https://github.com/flxbl-io/sfp/blob/main/packages/sfp-cli/resources/schemas/sfdx-project.schema.json).

[PreviousInstall sfp](https://docs.flxbl.io/sfp/getting-started/install-sfp) [NextBuild & Install an Artifact](https://docs.flxbl.io/sfp/getting-started/build-and-install-an-artifact)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Build and Install Artifacts

In the earlier section, we looked at how we configured the project directory for sfp, Now lets walk through some core commands.

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/build-and-install-an-artifact#a.-build-an-artifact-for-a-package) A. Build an artifact for a package

The build command will generate a zipped artifact file for each package you have defined in the sfdx-project.json file. The artifact file will contain metadata and the source code at the point build creation for you to use to install.

1. Open up a terminal within your Salesforce project directory and enter the following command:

Copy

```inline-grid
sfp build -v <DevHubAlias/DevHubUsername>
```

You will see the logs with details of your package creation, for instance here is a sample output

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F7UFp5ZqULAb67i3Y5qYj%252Fimage.png%3Falt%3Dmedia%26token%3D9baa067a-b9f9-44de-96e6-7fb068879d8d\&width=768\&dpr=4\&quality=100\&sign=6d6b32fe\&sv=2)

Build Outputs

1. A new "artifacts" folder will be generated within your source project containing a zipped artifact file for each package defined in your sfdx-project.json file.

For example, the artifact files will contain the following naming convention with the "\_sfpowerscript\_artifact\_" in between the package name and version number.

`package-name_sfpowerscripts_artifact_1.0.0-1.zip`

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FvRd3R2rLaC7VQzQEGAOK%252Fimage.png%3Falt%3Dmedia%26token%3Db57bb70d-5ca0-45f5-85d5-49d0f6f654c7\&width=768\&dpr=4\&quality=100\&sign=f884a69f\&sv=2)

artifacts folder

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/build-and-install-an-artifact#b.-install-the-artifact-to-a-target-org) B. Install the artifact to a target org

Ensure you have authenticated your salesforce cli to a org first. If you haven't, please find the instructions [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_web_flow.htm).

1. Once you have the authenticated to the sandbox, you could execute the installation command as below

Copy

```inline-grid
sfp install -o <TargetOrgAlias/TargetOrgUsername>
```

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FjEaNDKHasvZSRqDYF6BJ%252Fimage.png%3Falt%3Dmedia%26token%3D6450b4ab-4833-4506-8ab1-efd709255fb0\&width=768\&dpr=4\&quality=100\&sign=cce8927a\&sv=2)

Install Outputs

1. Navigate to your target org and confirm that the package is now installed with the expected changes from your artifact. In this example above, a new custom field has been added to the Account Standard Object.

Depending on the type of packages, [sfp will issue the equivalent test classes](https://docs.flxbl.io/sfp/concepts/supported-package-types) with in the package directory and it could result in failures during installation, Please fix the issues in your code and repeat till you get a sucessful installation. If your packages doesn't have sufficient test coverage, you may need to use the all tests in the org to get your package installed. Refer to the material [here](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation)

[PreviousConfigure Your Project](https://docs.flxbl.io/sfp/getting-started/setup-source-project) [NextCongratulations!](https://docs.flxbl.io/sfp/getting-started/congratulations)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### SFP Integration Success

Good Work! If you made it past the getting started guide with minimal errors and questions, you are well on your way to introduce sfp into your Salesforce Delivery workflow.

Let's summarize what you have done:

1. Setup pre-requisite software on your workstation and have access to a Salesforce Org.
2. Installed the latest sfp cli.
3. Configured your source project and added additional properties required for sfp cli to generate artifacts.
4. Build artifact(s) locally to be used to deploy.
5. Installed artifact(s) to target org.

This is just the tip of the iceberg for the full features sfp can provide for you and your team. Please continue to read further and experiment.

For any comments/recommendations to sfp so please join our [Slack Community](https://www.launchpass.com/flxblio). If you are adventurous, contribute!

[PreviousBuild & Install an Artifact](https://docs.flxbl.io/sfp/getting-started/build-and-install-an-artifact) [NextDocker Images](https://docs.flxbl.io/sfp/getting-started/docker-images)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### SFP Docker Images Guide

sfp docker images are published from the flxbl-io Github packages registry at the link provided below

[![Logo](https://github.com/fluidicon.png)GitHubGitHub](https://github.com/orgs/flxbl-io/packages)

One can utilize the flxbl-io sfp images by using the

Copy

```inline-grid
docker pull ghcr.io/flxbl-io/sfp:latest
```

You can also pin to a specific version of the docker image, by using the version published [here](https://github.com/flxbl-io/sfp/pkgs/container/sfp)

To preview latest images for the docker image, visit the [release candidate page](https://github.com/flxbl-io/sfp/pkgs/container/sfp-rc) and update your container image reference.

For example:

Copy

```inline-grid
default:
   image: ghcr.io/flxbl-io/sfp-rc:<version-number>

or
   image: ghcr.io/flxbl-io/sfp-rc:<sha>
```

[PreviousCongratulations!](https://docs.flxbl.io/sfp/getting-started/congratulations) [Nextsfp-pro](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### SFP-Pro Docker Images

SFP-Pro provides Docker images through our self-hosted Gitea registry at source.flxbl.io. These pre-built images are maintained and updated regularly with the latest features and security patches.

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#prerequisites) Prerequisites

1. Access to source.flxbl.io (Gitea server)
2. Docker installed on your machine
3. Registry credentials from your welcome email

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#accessing-the-images) Accessing the Images

1. Login to the Gitea registry:

Copy

```inline-grid
docker login source.flxbl.io -u your-username
```

1. Pull the desired image:

The version numbers can be found at https://source.flxbl.io/flxbl/-/packages/container/sfp-pro/

Copy

```inline-grid
# For base sfp-pro image
docker pull source.flxbl.io/sfp-pro:version

# For sfp-pro with SF CLI
docker pull source.flxbl.io/sfp-pro-sf-cli:version
```

1. (Optional) Tag for your registry:

Copy

```inline-grid
# Tag for your registry
docker tag source.flxbl.io/sfp-pro:version your-registry/sfp-pro:version

# Push to your registry
docker push your-registry/sfp-pro:version
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#best-practices) Best Practices

1. Use specific version tags in production
2. Cache images in your private registry for better performance
3. Implement proper access controls in your registry
4. Document image versions used in your pipelines

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#building-docker-images) Building Docker Images

If you need to build the images yourself, you can access the source code from source.flxbl.io and follow these instructions:

[**Direct link to heading**](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#prerequisites-1) **Prerequisites**

* Docker with BuildKit support
* GitHub Personal Access Token with `packages:read` permissions
* Node.js (for local development)

[**Direct link to heading**](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#building-the-main-image) **Building the Main Image**

Copy

```inline-grid
# Create a file containing your  GITEA token
echo "YOUR_GITEA_TOKEN" > .npmrc.token

# Build the image
docker buildx build \
  --secret id=npm_token,src=.npmrc.token \
  --build-arg NODE_MAJOR=22 \
  -f dockerfiles/sfp-pro.Dockerfile \
  -t sfp-pro:local .

# Remove the token file
rm .npmrc.token
```

[**Direct link to heading**](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#building-the-sf-cli-image) **Building the SF CLI Image**

Copy

```inline-grid
# Create a file containing your GitHub NPM token
echo "YOUR_GITEA_TOKEN" > .npmrc.token

# Build the image
docker buildx build \
  --secret id=npm_token,src=.npmrc.token \
  --build-arg NODE_MAJOR=22 \
  -f dockerfiles/sfp-pro-sf-cli.Dockerfile \
  -t sfp-pro-sf-cli:local .

# Remove the token file
rm .npmrc.token
```

[**Direct link to heading**](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#build-arguments) **Build Arguments**

The following build arguments are supported:

* `NODE_MAJOR`: Node.js major version (default: 22)
* `SFP_VERSION`: Version of SFP Pro to build
* `GIT_COMMIT`: Git commit hash for versioning
* `SF_COMMIT_ID`: Salesforce commit ID

#### [Direct link to heading](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro#support) Support

For issues or questions about Docker images, please contact flxbl support through your designated support channels.

[PreviousDocker Images](https://docs.flxbl.io/sfp/getting-started/docker-images) [NextOverview](https://docs.flxbl.io/sfp/concepts/overview)

Last updated 3 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### sfp Tool Overview

**sfp** is a purpose-built cli tool used predominantly in a modular salesforce project.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F25NBdGeewEf4AOzWdrWP%252Fconcept.png%3Falt%3Dmedia%26token%3D52ba493c-594b-4f35-91ea-7ba2efa1788d\&width=768\&dpr=4\&quality=100\&sign=8084fde0\&sv=2)

How sfp works

A project utilizing **sfp** implements the following concepts.

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/overview#domains) Domains

In an sfp-powered Salesforce project, "Domains" represent distinct business capabilities. A project can encompass multiple domains, and each domain may include various sub-domains. Domains are not explicitly declared within **sfp** but are conceptually organized through " [**Release Configs**](https://docs.flxbl.io/sfp/configuring-a-project/release-config)"

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/overview#packages) [**Packages**](https://docs.flxbl.io/sfp/concepts/packages)

A package (synonymous with modules) is a container that groups related metadata together. A package would contain components such as objects, fields, apex, flows and more, allowing these elements to be easily installed, upgraded and managed as a single unit. A package is defined as a directory in your project repository and is defined by an entry in sfdx-project.json

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/overview#artifacts) [**Artifacts**](https://docs.flxbl.io/sfp/concepts/artifacts)

An artifact represents a versioned snapshot of a package at a specific point in time. It includes the source code from the package directory (as specified in `sfdx-project.json`), along with metadata about the version, change logs, and other relevant details. Artifacts are the deployable units in the sfp framework, ensuring consistency and traceability across the development lifecycle.

When sfp is integrated into a Salesforce project, it centralizes around the following key essential process

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/overview#building-a-domain) [**Building a domain**](https://docs.flxbl.io/sfp/building-artifacts/overview)

Building' refers to the creation of an artifact from a package. Utilizing the `build` command, sfp facilitates the generation of artifacts for each package in your repository. This process encapsulates the package's source code and metadata into a versioned artifact ready for installation.

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/overview#publishing-a-domain) [**Publishing a domain**](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact)

Publishing a domain involves the process of publishing the artifacts generated by the build command into an artifact repository. This is the storage area where the artifacts are fetched for releases, rollback, etc.

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/overview#releasing-a-domain) **Releasing a domain**

Releasing an domain involves the process of promoting, installing a collection of artifacts to a higher org such as production, generating an associated changelog for the domain. This process is driven by the release command along with a [release definition](https://docs.flxbl.io/sfp/releasing-artifacts/release-definitions).

[Previoussfp-pro](https://docs.flxbl.io/sfp/getting-started/docker-images/sfp-pro) [NextSF CLI vs. SFP](https://docs.flxbl.io/sfp/concepts/sf-cli-vs.-sfp)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce CLI vs SFP

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/sf-cli-vs.-sfp#salesforce-cli) Salesforce CLI

The Salesforce CLI is a command-line interface that simplifies development and build automation when working with your Salesforce org. There are numerous of commands that the sf cli provides natively that is beyond the scope of this site and can be found on the official Salesforce Documentation Site.

From a build, test, and deployment perspective, the following diagram depicts the bare minimum commands necessary to get up and running in setting up your sf project, retrieving and deploying code to the target environments.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FYVACMKnJnwtgoeJUTotT%252Fimage.png%3Falt%3Dmedia%26token%3Dfb06a8fe-0bf0-4ed9-85fb-bdc2c5153848\&width=768\&dpr=4\&quality=100\&sign=ef3579ba\&sv=2)

sf cli deployments

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/sf-cli-vs.-sfp#sfp) SFP

sfp is based on the [The Open CLI Framework](https://oclif.io/) for building a command line interface (CLI) in [Node.js](https://nodejs.org/api/cli.html). Instead of being a typical Salesforce CLI plugin, sfp is standalone and leverages the same core libraries and APIs as the [@salesforce/cli](https://www.npmjs.com/package/@salesforce/cli). sfp releases are independently managed and as core npm libraries are stable, we will update them as needed to ensure no breaking changes are introduced.

The diagram below depicts the basic flow of the development and test process, building artifacts, and deploying to target environments.

Once you have mastered the basic workflow, you can progress to publishing artifacts to a NPM Repository that will store immutable, versions of the metadata and code used to drive the release of your packages across Salesforce Environments.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FlWXTpfqxGtZYkGmZGeUb%252Fimage.png%3Falt%3Dmedia%26token%3D0612f814-5b66-47d2-8985-280955ccd995\&width=768\&dpr=4\&quality=100\&sign=b3286ee2\&sv=2)

sfp Develop, Test, and Release Workflow

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/sf-cli-vs.-sfp#references) References

The list below is a curated list of core sf cli and Salesforce DX developer guides for your reference.

* SF CLI
* [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
* [Salesforce CLI Release Notes](https://github.com/forcedotcom/cli/blob/main/releasenotes/README.md)
* [Salesforce CLI Status Page](https://github.com/salesforcecli/status)
* [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
* [Developing sf Plugins](https://github.com/salesforcecli/cli/wiki/Quick-Introduction-to-Developing-sf-Plugins)
* [@salesforce/cli NPM Repository](https://www.npmjs.com/package/@salesforce/cli)
* sfp
* [@flxblio/sfp NPM Repository](https://www.npmjs.com/package/@flxblio/sfp)
* [GitHub Repository](https://github.com/flxbl-io/sfp)

[PreviousOverview](https://docs.flxbl.io/sfp/concepts/overview) [NextDomains](https://docs.flxbl.io/sfp/concepts/domains)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce Package Domains

sfp is a natural fit for organisations that utilize Salesforce in a large enterprise setting as its purpose built to deal with modular saleforce development. Often these organisations have teams dedicated to a particular business function, think of a team who works on features related to billing, while a team works on features related to service

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252Fpukp6E0K3Ge9cEiCggKl%252Fimage.png%3Falt%3Dmedia%26token%3Dfe856081-5fa2-4079-a032-417ef8570188\&width=768\&dpr=4\&quality=100\&sign=f4cfedd2\&sv=2)

Packages in an org organised by domains

The diagram illustrates the method of organizing packages into specific categories for various business units. These categories are referred to as **'domains'** within the context of sfp cli.

Each domain can either contain further domains ( sub-domains) and each domain constitute of one or more packages.

**sfp** cli utilizes ['Release Config'](https://docs.flxbl.io/sfp/configuring-a-project/release-config) to organise packages into domains. You can read more about creating a release config in the next section

[PreviousSF CLI vs. SFP](https://docs.flxbl.io/sfp/concepts/sf-cli-vs.-sfp) [NextPackages](https://docs.flxbl.io/sfp/concepts/packages)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Understanding Packages

Packages are containers used to group related metadata together. A package would contain components such as objects, fields, apex, flows and more allowing these elements to be easily installed, upgraded and managed as a single unit

Packages in the context of sfp are not limited to second generation packaging (2GP), sfp supports various types of packages which you can read in the following sections

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FkGaogJezm3frteQ6b68V%252Fpackage-directory.png%3Falt%3Dmedia%26token%3D032a8a72-bfbb-4a0d-8657-98162a41c296\&width=768\&dpr=4\&quality=100\&sign=a38b274a\&sv=2)

Defining a package in repository

[PreviousDomains](https://docs.flxbl.io/sfp/concepts/domains) [NextSupported package types](https://docs.flxbl.io/sfp/concepts/supported-package-types)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Supported Package Types

sfp cli supports operations on various types of packages within your repository. A short summary on the comparison between different package types is provided below

Features

Unlocked Packages

Org Dependent Unlocked Packages

Source Packages

Data Packages

Diff Packages

Dependency Validation

Occurs during package creation

Occurs during package installation

Occurs during package installation

N/A

Occurs during package installation

Dependency Declaration

Yes

Yes (supported by sfp)

Yes

Yes

Yes (supported by sfp)

Requires dependency to be resolved during creation

Yes

No

No

N/A

No

Supported Metadata Types

Unlocked Package Section in [Metadata Coverage Report](https://developer.salesforce.com/docs/metadata-coverage/)

Unlocked Package Section in [Metadata Coverage Report](https://developer.salesforce.com/docs/metadata-coverage/)

Metadata API\
Section in [Metadata Coverage Report](https://developer.salesforce.com/docs/metadata-coverage/)

N/A

Metadata API\
Section in [Metadata Coverage Report](https://developer.salesforce.com/docs/metadata-coverage/)

Code Coverage Requirement

Package should have 75% code coverage or more

Not enforced by Salesforce, sfp by default checks for 75% code coverage

Each apex class should have a coverage of 75% or above for optimal deployment, otherwise the entire coverage of the org will be utilized for deployment

N/A

Each apex class that's part of the delta between the current version and the baseline needs a test class and requires a coverage of 75%.

Component Lifecycle

Automated

Automated

Explicit, utilize destructiveManifest or manual deletion

N/A

Explicit, utilize destructiveManifest or manual deletion

Component Lock

Yes, only one package can own the component

Yes, only one package can own the component

No

N/A

No

Version Management

Salesforce enforced versioning; Promotion required to deploy to prod

Salesforce enforced versioning; Promotion required to deploy to prod

sfp enforced versioning

sfp enforced versioning

sfp enforced versioning

[PreviousPackages](https://docs.flxbl.io/sfp/concepts/packages) [NextUnlocked Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Unlocked Packages Overview

There is a huge amount of documentation on unlocked packages. Here are list of curated links that can help you get started on learning more about unlocked package

_**The Basics**_

* [Package Development Model](https://trailhead.salesforce.com/content/learn/modules/sfdx_dev_model)
* [Unlocked Package for Customers](https://trailhead.salesforce.com/content/learn/modules/unlocked-packages-for-customers)
* [Successfully Creating Unlocked Package](https://www.youtube.com/watch?v=xJNmHOtIgO0)
* [Salesforce Developer Guide to Unlocked Package](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_intro.htm)
* [Unlocked Package FAQ](https://sfdc-db-gmail.github.io/unlocked-packages/faq-unlocked-pkgs.html)

_**Advanced Materials**_

* [Anti Patterns in Package Dependency Design](https://medium.com/salesforce-architects/5-anti-patterns-in-package-dependency-design-and-how-to-avoid-them-87bb50331cb8)

The following sections deals with more of the operational aspects when dealing with unlocked packages

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#unlocked-package-and-test-coverage) Unlocked Package and Test Coverage

Unlocked Packages, excluding Org-Dependent unlocked packages have mandatory test coverage requirements. Each package should have minimum of 75% coverage requirement. A validated build (or [build command](https://dxatscale.gitbook.io/sfpowerscripts/commands/build-and-quickbuild) in sfp) validates the coverage of package during the build phase. To enable the feedback earlier in the process, sfp provide you functionality to validate test coverage of a package such as during the Pull Request Validation process.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#managing-version-numbers-of-unlocked-package) Managing Version Numbers of Unlocked Package

For unlocked packages, we ask users to follow a [semantic](https://semver.org/) versioning of packages.

Please note Salesforce packages do not support the concept of PreRelease/BuildMetadata. The last segment of a version number is a build number. We recommend to utilize the auto increment functionality provided by Salesforce rather than rolling out your own build number substitution ( Use 'NEXT' while describing the build version of the package and 'LATEST' to the build number where the package is used as a dependency)

Note that an unlocked package must be [promoted](https://sfpowerscripts.dxatscale.io/commands/command-glossary#sfdx-sfpowerscripts-orchestrator-promote) before it can be installed to a production org, and either the major, minor or patch (not build) version **must** be higher than the last version of this package which was promoted. These version number changes should be made in the `sfdx-project.json` file before the final package build and promotion.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#deprecating-components-from-an-unlocked-package) Deprecating Components from an Unlocked Package

Unlocked packages provide traceability in the org by locking down the metadata components to the package that introduces it. This feature which is the main benefit of unlocked package can also create issues when you want to refactor components from one package to another. Let's look at some scenarios and common strategies that need to be applied

For a project that has two packages.

* **Package A** and **Package B**
* **Package B** is dependent on **Package A.**

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#scenario-1) **Scenario 1:**

* Remove a component from **Package A**, provided the component has no dependency
* **Solution:** Create a new version of **Package A** with the metadata component being removed and install the package.

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#scenario-2) **Scenario 2:**

* Move a metadata component from **Package A** to **Package B**
* **Solution:** This scenario is pretty straight forward, one can remove the metadata component from **Package A** and move that to **Package B**. When a new version of **Package A** gets installed, the following things happen:
* If the deployment of the unlocked package is set to mixed, and no other metadata component is dependent on the component, the component gets [deleted](https://help.salesforce.com/articleView?id=sf.fields_managing_deleted_fields.htm\&type=5).
* On the subsequent install of **Package B**, **Package B** restores the field and takes ownership of the component.

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#scenario-3) **Scenario 3:**

* Move a metadata component from **Package B** to **Package A**, where the component currently has other dependencies in **Package B**
* **Solution:** In this scenario, one can move the component to **Package A** and get the packages built. However during deployment to an org, **Package A** will fail with an error this component exists in **Package B**. To mitigate this one should do the following:
* Deploy a version of **Package B** which removes the lock on the metadata component using deprecate mode. Some times this needs extensive refactoring to other components to break the dependencies. So evaluate whether the approach will work.
* If not, you can go to the **UI** ( **Setup > Packaging > Installed Packages > > View Components and Remove**) and remove the lock for a package.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#managing-package-dependencies) Managing Package Dependencies

Package dependencies are defined in the sfdx-project.json. More information on defining package dependencies can be found in the Salesforce [docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev2gp_config_file.htm).

Copy

```inline-grid
{
  "packageDirectories": [\
    {\
      "path": "util",\
      "default": true,\
      "package": "Expense-Manager-Util",\
      "versionName": "Winter ‘20",\
      "versionDescription": "Welcome to Winter 2020 Release of Expense Manager Util Package",\
      "versionNumber": "4.7.0.NEXT"\
    },\
    {\
      "path": "exp-core",\
      "default": false,\
      "package": "ExpenseManager",\
      "versionName": "v 3.2",\
      "versionDescription": "Winter 2020 Release",\
      "versionNumber": "3.2.0.NEXT",\
      "dependencies": [\
        {\
          "package": "ExpenseManager-Util",\
          "versionNumber": "4.7.0.LATEST"\
        },\
          {\
          "package": "TriggerFramework",\
          "versionNumber": "1.7.0.LATEST"\
        },\
        {\
          "package": "External Apex Library - 1.0.0.4"\
        }\
      ]\
    }\
  ],
  "sourceApiVersion": "47.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

Let's unpack the concepts utilizing the above example:

* There are two unlocked packages
* Expense Manager - Util is an unlocked package in your DevHub, identifiable by 0H in the packageAlias
* Expense Manager - another unlocked package which is dependent on ' Expense Manager - Util', 'TriggerFramework' and 'External Apex Library - 1.0.0.4'
* External Apex Library is an external dependency, it could be a managed package or any unlocked package released on a different Dev Hub. All external package dependencies must be defined with a 04t ID, which can be determined from the installation URL from AppExchange or by contacting your vendor.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#build-options-with-unlocked-packages) Build Options with Unlocked Packages

Unlocked packages have two build modes, one with [skip dependency check](https://github.com/dxatscale/dxatscale-guide/blob/april-22/development-practices/types-of-packaging/broken-reference/README.md) and one without. A package being built without skipping dependency check cant be deployed into production and can usually take a long time to build. sfp cli tries to build packages in parallel understanding your dependency, however some of your packages could spend a significant time in validation.

During these situations, we ask you to consider whether the time taken to build all validated packages on an average is within your build budget, If not, here are your options

* [Move to org dependent package](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_org_dependent.htm): Org-dependent unlocked packages are a variant of unlocked packages. Org-dependent packages do not validate the dependencies of a package and will be faster. However please note that all the org's where the earlier unlocked package was installed, had to be deprecated and the component locks removed, before the new org-dependent unlocked package is installed.
* [Move to source-package:](https://github.com/dxatscale/dxatscale-guide/blob/april-22/development-practices/types-of-packaging/broken-reference/README.md) Use it as the least resort, source packages have a fairly loose lifecycle management.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages#handling-metadata-that-is-not-supported-by-unlocked-packages) Handling metadata that is not supported by unlocked packages

Create a [source package](https://github.com/dxatscale/dxatscale-guide/blob/april-22/development-practices/types-of-packaging/broken-reference/README.md) and move the metadata and any associated dependencies over to that particular package.

[PreviousSupported package types](https://docs.flxbl.io/sfp/concepts/supported-package-types) [NextOrg-Dependent Unlocked Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/org-dependent-unlocked-packages)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Org-Dependent Unlocked Packages

Org-dependent unlocked packages, a variation of unlocked packages, allow you to create packages that depend on unpackaged metadata in the target org. Org dependent package is very useful in the context of orgs that have lots of metadata and is struggling with understanding the dependency while building a ' [unlocked package](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages)'

Org dependent packages significantly enhance the efficiency of #flxbl projects who are already on scratch org based development. By allowing installation on a clean slate, these packages validate dependencies upfront, thereby reducing the additional validation time often required by unlocked packages.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/org-dependent-unlocked-packages#org-dependent-unlocked-package-and-test-coverage) Org-Dependent Unlocked Package and Test Coverage

Org-dependent unlocked packages bypass the test coverage requirements, enabling installation in production without standard validation. This differs significantly from metadata deployments, where each Apex class deployed must meet a 75% coverage threshold or rely on the org's overall test coverage. While beneficial for large, established orgs, this approach should be used cautiously.

To address this, sfpowerscripts incorporates a default test coverage validation for org-dependent unlocked packages during the validation process. To disable this test coverage check during validation, additional attributes must be added to the package directory in the `sfdx-project.json` file.

Copy

```inline-grid
// Org dependent unlocked package with additonal attributes

  "packageDirectories": [\
    {\
      "path": "src/core/core-crm",\
      "package": "core-crm",\
      "versionNumber": "4.7.0.NEXT",\
      "skipTesting": true,\
      "skipCoverageValidation": true\
    },\
     ...\
   ]
}
```

[PreviousUnlocked Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/unlocked-packages) [NextSource Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/source-packages)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Source Packages Overview

Source Packages is an sfp cli feature that mimics the behaviour of native unlocked packages.

Source Packages are metadata deployments from a Salesforce perspective, it is a group of components that are deployed to an org. Unlocked packages are a **First Class** Salesforce deployment construct, where the lifecycle is governed by the org, such as deleting/deprecating metadata and validating versions.

We always recommend using unlocked packages over source packages whenever you can. As a matter of preference, this is our priority of package types

1. [Unlocked Packages](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_intro.htm)
2. [Unlocked Packages (Org-Dependent)](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_org_dependent.htm)
3. Source Packages

Source Packages are typically used for application-config (configuring an application delivered by a managed package such as changes to help text/description of fields ) or when you come across these constraints

* [Metadata not supported by Unlocked Packages](https://developer.salesforce.com/docs/metadata-coverage)
* Facing bugs while deploying the metadata using unlocked packages
* Unlocked Package validation takes too long (still we recommend go org-dependent)
* Dealing with metadata that is global or org-specific in nature (such as queues, profiles, etc or composite UI layouts, which doesn't make sense to be packaged using unlocked package)
* Development teams who are starting to adopt package-based development and want to organize their metadata

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/source-packages#a-salesforce-org-composed-only-of-source-packages) **A Salesforce Org composed only of source packages**

A Salesforce Org can be composed only with source packages, however the lack of dependency validation as in unlocked packages, lack of automated destruction of changes can make it a bit challenging. As salesforce org is not aware of the source package, there is no component lock , so another source package with same metadata component can alwasy overwrite a metadata component deployed by another package. For these, reasons, we always recommend you prefer unlocked package or its variant org dependent unlocked packages.

An example of a common metadata component that typically gets overridden is [Custom Labels](https://developer.salesforce.com/docs/atlas.en-us.api_meta.meta/api_meta/meta_customlabels.htm) which can span across multiple packages.

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/source-packages#dependency-management-for-source-packages) Dependency Management for source packages

**Source packages can depend on other unlocked packages or managed packages**, however dependencies of source packages are validated during deployment time, ie., source packages assume that dependent metadata is already there in your org before the metadata in the source package is being deployed. That being said, for purposes of development in scratch org, you could add ' **unlocked/managed package'** dependencies to a source package, so sfp cli commands like prepare and validate (in sfpowerscripts:orchestrator) will install the dependencies to the target org before proceedint to install source package

[PreviousOrg-Dependent Unlocked Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/org-dependent-unlocked-packages) [NextDiff Package](https://docs.flxbl.io/sfp/concepts/supported-package-types/diff-package)

Last updated 8 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Diff Package Overview

A diff package is a variant of 'source package' , where the contents of the package only contain the components that are changed. This package is generated by sfpowerscripts by computing a `git diff` of the current commit id against a baseline set in the Dev Hub Org

A diff package mimics a [Change Set](https://help.salesforce.com/s/articleView?id=sf.changesets.htm\&type=5) model, where only changes are contained in the artifact. As such, this package is always an incremental package. It only deploys the changes incrementally compared to baseline and applied to the target org. Unless both previous version and the current version have the exact same components, a diff package can never be rolled back, as the impact on the org is unpredictable. It is always recommended to roll forward a diff package by fixing or creating a necessary change that counters the impact that you want on the target orgs.

Diff packages doesnt work with scratch orgs. It should be used with sandboxes only.

A diff package is the least consistent package among the various package types available within sfpowerscripts, and should only be used for transitioning to a modular development model as prescribed by flxbl

The below example demonstrates a sfdx-project.json where the package `unpackaged` is a diff package. You can mark a diff package with the type 'diff'. All other attributes applicable to source packages are applicable for diff packages.

Copy

```inline-grid
// sfdx-project.json

{
  "packageDirectories": [\
    {\
      "path": "util",\
      "default": false,\
      "package": "Expense-Manager-Util",\
      "versionName": "Winter ‘24",\
      "versionDescription": "Welcome to Winter 2024 Release of Expense Manager Util Package",\
      "versionNumber": "4.7.0.NEXT"\
    },\
     {\
      "path": "unpackaged",\
      "default": true,\
      "package": "unpackaged",\
      "versionName": "v3.2",\
      "type":"diff"\
    }\
  ],
  "sourceApiVersion": "58.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

A manual entry in `sfpowerscripts_Artifact2__c` custom object should be made with the name of the package and the baseline commit id and version. Subsequent deployments will automatically reset the baseline when the package gets deployed to Dev Hub

[PreviousSource Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/source-packages) [NextData Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce Data Packages

Data packages are a sfpowerscripts construct that utilise the [SFDMU plugin](https://github.com/forcedotcom/SFDX-Data-Move-Utility) to create a versioned artifact of Salesforce object records in csv format, which can be deployed to the a Salesforce org using the sfpowerscripts package installation command.

The Data Package offers a seamless method of integrating Salesforce data into your CICD pipelines , and is primarily intended for record-based configuration of managed package such as CPQ, Vlocity (Salesforce Industries), and nCino.

Data packages are a wrapper around SFDMU that provide a few key benefits:

* **Ability to skip the package if already installed:** By keeping a record of the version of the package installed in the target org with the support of an unlocked package, sfpowerscripts can skip installation of data packages if it is already installed in the org
* **Versioned Artifact:** Aligned with sfpowerscripts principle of traceability, every deployment is traceable to a versioned artifact, which is difficult to achieve when you are using a folder to deploy
* **Orchestration:** Data package creation and installation can be orchestrated by sfpowerscripts, which means less scripting

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages#defining-a-data-package) Defining a Data package

Simply add an entry in the package directories, providing the package's name, path, version number and type (data). Your editor may complain that the 'type' property is not allowed, but this can be safely ignored.

Copy

```inline-grid
  {
    "path": "path--to--data--package",
    "package": "name--of-the-data package", //mandatory, when used with sfpowerscripts
    "versionNumber": "X.Y.Z.0 // 0 will be replaced by the build number passed",
    "type": "data", // required
  }
```

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages#generating-the-contents-of-the-data-package) Generating the contents of the data package

Export your Salesforce records to csv files using the [SFDMU plugin](https://github.com/forcedotcom/SFDX-Data-Move-Utility). For more information on plugin installation, creating an export.json file, and exporting to csv files, refer to _Plugin Basic > Basic Usage_ in SFDMU's [documentation](https://help.sfdmu.com/quick-start).

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FCRKy5wYLC4JElbDVxSRt%252Fimage.png%3Falt%3Dmedia%26token%3D3298d3fc-2b9e-46b6-8933-6c2c026fd4fa\&width=768\&dpr=4\&quality=100\&sign=1c704e9b\&sv=2)

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages#options-with-data-packages) **Options with Data Packages**

Data packages support the following options, through the sfdx-project.json.

Copy

```inline-grid
  {
    "path": "path--to--package",
    "package": "name--of-the-package", //mandatory, when used with sfpowerscripts
    "versionNumber": "X.Y.Z.[NEXT/BUILDNUMBER]",
    "type": "data", // required
    "aliasfy": <boolean>, // Only for source packages, allows to deploy a subfolder whose name matches the alias of the org when using deploy command
    "assignPermSetsPreDeployment: ["","",],
    "assignPermSetsPostDeployment: ["","",],
    "preDeploymentScript":<path>, //All Packages
    "postDeploymentScript":<path> // All packages
  }
```

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages#adding-pre-post-deployment-scripts-to-data-packages) Adding Pre/Post Deployment Scripts to Data Packages

Refer to this link for more details on how to add a pre/post script to data package

Copy

```inline-grid
# $1 package name
# $2 org
# $3 alias
# $4 working directory
# $5 package directory

sfdx force:apex:execute -f scripts/datascript.apex -u $2
```

### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages#defining-a-vlocity-data-package) Defining a vlocity Data Package

sfpowerscripts support vlocity RBC migration using the vlocity build tool (vbt). sfpowerscripts will be automatically able to detect whether a data package need to be deployed using vlocity or using sfdmu. (Please not to enable vlocity in preparing scratchOrgs, the enableVlocity flag need to be turned on in the pool configuration file)

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FnGhWIwgbWw7zyqZe50Wp%252Fimage.png%3Falt%3Dmedia%26token%3D4fdec8ce-8faf-4929-b855-25e6371034d5\&width=768\&dpr=4\&quality=100\&sign=8540e49f\&sv=2)

A vlocity data package need to have vlocityComponents.yaml file in the root of the package directory and it should have the following definition

Copy

```inline-grid
projectPath: src/vlocity-config // Path to the package directory
expansionPath: datapacks // Path to the folder containing vlocity attributes
autoRetryErrors: true //Additional items
manifest:
```

The same package would be defined in the sfdx-project.json as follows

Copy

```inline-grid
        {
            "path": "./src/vlocity-config",
            "package": "vlocity-attributes",
            "versionNumber": "1.0.0.0",
            "type": "data"
        }
```

[PreviousDiff Package](https://docs.flxbl.io/sfp/concepts/supported-package-types/diff-package) [NextArtifacts](https://docs.flxbl.io/sfp/concepts/artifacts)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Artifact Registry Overview

Why do you need an artifact registry? - YouTube

flxbl

134 subscribers

[Why do you need an artifact registry?](https://www.youtube.com/watch?v=Vrjl-ISUaC8)

flxbl

Search

Watch later

Share

Copy link

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

More videos

### More videos

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

[Watch on](https://www.youtube.com/watch?v=Vrjl-ISUaC8\&embeds_referring_euri=https%3A%2F%2Fcdn.iframe.ly%2F)

0:00

0:00 / 2:59•Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=Vrjl-ISUaC8)

An artifact is a key concept within sfp. An artifact is a point in time snapshot of a version of a package, as mentioned in sfdx-project.json . The snapshot contains source code of a package directory , additional metadata information regarding the particular version, changelog and other details. An artifact for 2GP package would also contain details such as [Subscriber Package Version ID](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_pkg_ids.htm)

In the context of sfp, packages are represented in your version control, and artifact is an output of a the build command when operated on a package

Artifacts provide an abstraction over version control, as it detaches the version control from from the point of releasing into a salesforce org. Almost all commands in sfp operates on an artifact or generates an artifact.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FWCk2peMG0qGDaFHyDarz%252Fimage.png%3Falt%3Dmedia%26token%3D1fb26c71-b5ca-4325-b185-e4c0cac00f7a\&width=768\&dpr=4\&quality=100\&sign=ba457175\&sv=2)

An expansion of an artifact generated by sfp

[![Logo](https://miro.medium.com/v2/resize:fill:152:152/1*sHhtYhaCe2Uc3IU0IgKwIQ.png)Simplify your Salesforce Branching StrategyFLXBL](https://medium.com/flxbl/simplify-your-salesforce-branching-strategy-915565e8efa6)

[![Logo](https://miro.medium.com/v2/resize:fill:152:152/1*sHhtYhaCe2Uc3IU0IgKwIQ.png)Navigating Salesforce Deployment Strategies: Artifact vs. Delta DeploymentsFLXBL](https://medium.com/flxbl/navigating-salesforce-deployment-strategies-artifact-vs-delta-deployments-e704824acea3)

In the context of **sfp**, an **artifact** represents a more enhanced concept compared to a Salesforce package. While it is based on a Salesforce package or specific package types introduce by sfp, an artifact in sfp includes additional attributes and metadata that describe the package version, dependencies, installation behavior, and other context-specific information relevant to the CI/CD process. Artifacts in sfp are designed to be more than just a bundle of code and metadata; they encapsulate the package along with its CI/CD lifecycle information, making them more aligned with DevOps practices.

sfp's artifacts are built to be compatible for npm package supported registries , most CI/CD providers provide a npm compatible registry to host these packages/artifacts. Here is the link to operate on Github Package Manager for instance ( [https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry))

Artifacts built by sfp follow a naming convention that starts with the \<name\_of\_the\_package> _sfpowerscripts\_artifact\_..-._ One can use any of the npm commands to interact with sfp artifacts.

[PreviousData Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages) [NextPackage vs Artifacts](https://docs.flxbl.io/sfp/concepts/package-vs-artifacts)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce Packages Overview

In Salesforce, a **package** is a container that groups together metadata and code in order to facilitate the deployment and distribution of customizations and apps. Salesforce supports different types of packages, such as:

* **Unlocked Packages:** These are more modular and flexible than traditional managed packages, allowing developers to group and release customizations independently. They are version-controlled, upgradeable, and can include dependencies on other packages.
* **Org-Dependent Unlocked Packages:** Similar to unlocked packages but with a dependency on the metadata of the target org, making them less portable but useful for specific org customizations.
* **Managed Packages**: Managed packages are a type of Salesforce package primarily used by ISVs (Independent Software Vendors) for distributing and selling applications on the Salesforce AppExchange. They are fully encapsulated, which means the underlying code and metadata are not accessible or editable by the installing organization. Managed packages support versioning, dependency management, and can enforce licensing. They are ideal for creating applications that need to be securely distributed and updated across multiple Salesforce orgs without exposing proprietary code.

sfp auguments the above formal salesforce package types with additional package types such as below

* **Source Packages:** These are not formal packages in Salesforce but refer to a collection of metadata and code retrieved from a Salesforce org or version control that can be deployed to an org but aren't versioned or managed as a single entity.
* **Diff Packages:** These are not a formal type of Salesforce package but refer to packages created by determining the differences (diff) between two sets of metadata, often used for deploying specific changes.

In the context of **sfp**, an **artifact** represents a more enhanced concept compared to a Salesforce package. While it is based on a Salesforce package (of any type mentioned above), an artifact in sfp includes additional attributes and metadata that describe the package version, dependencies, installation behavior, and other context-specific information relevant to the CI/CD process. Artifacts in sfp are designed to be more than just a bundle of code and metadata; they encapsulate the package along with its CI/CD lifecycle information, making them more aligned with DevOps practices.

Key differences between Salesforce packages and sfp artifacts include:

* **Versioning and Dependencies:** While Salesforce packages support versioning, sfp artifacts enrich this with detailed dependency tracking, ensuring that the CI/CD pipeline respects the order of package installations based on dependencies.
* **Installation Behavior:** Artifacts in sfp carries additional metadata that defines custom installation behaviors, such as pre- and post-installation scripts or conditional installation steps, which are not inherently a part of Salesforce packages.
* **CI/CD Integration:** Artifacts in sfp are specifically designed to fit into a CI/CD pipeline, such as supporting storing in an artifact registory, version tracking, and release management that are essential for automated deployments but are outside the scope of Salesforce packages themselves.

[PreviousArtifacts](https://docs.flxbl.io/sfp/concepts/artifacts) [NextIdentifying types of a package](https://docs.flxbl.io/sfp/concepts/identifying-types-of-a-package)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Identifying Package Types

This section details identifies on how sfp analyzes and classifies different package types by using the information in sfdx-project.json

* **Unlocked Packages** are identified if a matching alias with a package version ID is found and verified through the DevHub. For example, the package named "Expense-Manager-Util" is found to be an Unlocked package upon correlation with its alias "Expense Manager - Util" and subsequent verification.
* **Source Packages** are assumed if no matching alias is found in `packageAliases`. These packages are typically used for source code that is not meant to be packaged and released as a managed or unlocked package.
* The presence of an additional `type` attribute within a package directory will further inform sfp of the specific nature of the package. For instance, types such as "data" for data packages or "diff" for diff packages

The sfdx-project.json file outlines various specifications for Salesforce DX projects, including the definition and management of different types of Salesforce packages. From the sample provided, sfp (Salesforce Package Builder) analyzes the "package" attribute within each `packageDirectories` entry, correlating with `packageAliases` to identify package IDs, thereby determining the package's type as 2GP (Second Generation Packaging).

Consider the following sfdx-project.json

Copy

```inline-grid
// Sample sfdx-project.json
{
  "packageDirectories": [\
    {\
      "path": "util",\
      "default": true,\
      "package": "Expense-Manager-Util",\
      "versionName": "Winter ‘20",\
      "versionDescription": "Welcome to Winter 2020 Release of Expense Manager Util Package",\
      "versionNumber": "4.7.0.NEXT"\
    },\
    {\
      "path": "exp-core",\
      "default": false,\
      "package": "ExpenseManager",\
      "versionName": "v 3.2",\
      "versionDescription": "Winter 2020 Release",\
      "versionNumber": "3.2.0.NEXT",\
      "dependencies": [\
        {\
          "package": "ExpenseManager-Util",\
          "versionNumber": "4.7.0.LATEST"\
        },\
          {\
          "package": "TriggerFramework",\
          "versionNumber": "1.7.0.LATEST"\
        },\
        {\
          "package": "External Apex Library - 1.0.0.4"\
        }\
      ]\
    },\
    {\
      "path": "exp-core-config",\
      "package": "expense-manager-config",\
      "versionNumber": "1.0.1.NEXT",\
      "versionDescription": "This source package extends expense manager unlocked package",\
    },\
    {\
      "path": "expense-manager-test-data",\
      "package": "exppense-manager-test-data",\
      "type":"data",\
      "versionNumber": "1.0.1.NEXT",\
      "versionDescription": "This source package extends expense manager unlocked package",\
    },\
  ],
  "sourceApiVersion": "47.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

[**Direct link to heading**](https://docs.flxbl.io/sfp/concepts/identifying-types-of-a-package#understanding-package-type-determination-using-the-samplesfdx-project.json) **Understanding Package Type Determination using the sample `sfdx-project.json`**

The `sfdx-project.json` sample can be used to determine how sfp processes and categorizes packages within a Salesforce DX project. The determination process for each package type, based on the attributes defined in the `packageDirectories`, unfolds as follows:

* **Unlocked Packages:** For a package to be identified as an Unlocked package, sfp looks for a correlation between the `package` name defined in `packageDirectories` and an alias within `packageAliases`. In the provided example, the "Expense-Manager-Util" under the `util` path is matched with its alias "Expense Manager - Util", subsequently confirmed through the DevHub with its package version ID, categorizing it as an Unlocked package.
* **Source Packages:** If a package does not have a corresponding alias in `packageAliases`, it is treated as a Source package. These packages are typically utilized for organizing source code not intended for release. For instance, packages specified in paths like "exp-core-config" and "expense-manager-test-data" would default to Source packages if no matching aliases are found.
* **Specialized Package Types:** The explicit declaration of a `type` attribute within a package directory allows for the differentiation into more specialized package types. For example, the specification of `"type": "data"` explicitly marks a package as a Data package, targeting specific use cases different from typical code packages.

[PreviousPackage vs Artifacts](https://docs.flxbl.io/sfp/concepts/package-vs-artifacts) [NextDependency management](https://docs.flxbl.io/sfp/concepts/dependency-management)

Last updated 11 months ago

Was this helpful?

### Dependency Management

sfp features commands and functionality that helps in dealing with complexity of defining dependency of unlocked packages. These functionalities are designed considering aspects of a #flxbl project, such as the predominant use of mono repositories in the context of a non-ISV scenarios.

Package dependencies are defined in the sfdx-project.json (Project Manifest). More information on defining package dependencies can be found in the Salesforce [docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev2gp_config_file.htm).

Copy

```inline-grid
{
  "packageDirectories": [\
    {\
      "path": "util",\
      "package": "Expense-Manager-Util",\
      "versionName": "Winter ‘25",\
      "versionDescription": "Welcome to Winter 2025 Release of Expense Manager Util Package",\
      "versionNumber": "4.7.0.NEXT"\
    },\
    {\
      "path": "exp-core",\
      "default": false,\
      "package": "ExpenseManager",\
      "versionName": "v 3.2",\
      "versionDescription": "Winter 2025 Release",\
      "versionNumber": "3.2.0.NEXT",\
      "dependencies": [\
        {\
          "package": "ExpenseManager-Util",\
          "versionNumber": "4.7.0.LATEST"\
        },\
          {\
          "package": "TriggerFramework",\
          "versionNumber": "1.7.0.LATEST"\
        },\
        {\
          "package": "External Apex Library - 1.0.0.4"\
        }\
      ]\
    },\
    {\
      "path": "src/exp-core-config",\
      "package": "Expense-Manager-Org-Config",\
      "type" : "source",\
      "versionName": "Winter ‘25",\
      "versionDescription": "Welcome to Winter 2025 Release of Expense Manager Util Package",\
      "versionNumber": "4.7.0.NEXT"\
    },\
  ],
  "sourceApiVersion": "47.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

Let's unpack the concepts utilizing the above example:

* There are two unlocked packages and one source package
* Expense Manager - Util is an unlocked package in your DevHub, identifiable by 0H in the packageAlias
* Expense Manager - another unlocked package which is dependent on ' Expense Manager - Util', 'TriggerFramework' and 'External Apex Library - 1.0.0.4'
* Expense Manager Org Config - a source package, lets assume has some reports and dashboards
* External Apex Library is an external dependency, it could be a managed package or any unlocked package released on a different Dev Hub. All external package dependencies must be defined with a 04t ID, which can be determined from the installation URL from AppExchange or by contacting your vendor.
* Source Packages/Org Dependent Unlocked / Data packages have an implied dependency as defined by the order of installation, as in it assumes any depedent metadata is already available in the target org before the installation of components within the package

[PreviousIdentifying types of a package](https://docs.flxbl.io/sfp/concepts/identifying-types-of-a-package) [NextTransitive Dependency Resolution](https://docs.flxbl.io/sfp/concepts/transitive-dependency-resolution)

Last updated 1 year ago

Was this helpful?

### Transitive Dependency Resolution

This feature is by default activated whenever build/quickbuild even in implicit scenarios such as validate, prepare etc, which might result in building packages.

Let's consider the following sfdx-project.json to explain how this feature works.

Copy

```inline-grid
{
    "packageDirectories": [\
       {\
            "path": "./src/frameworks/sfdc-logging",\
            "package": "sfdc-logging",\
            "versionName": "Version 1.0.2",\
            "versionNumber": "1.0.2.NEXT"\
        },\
        {\
            "path": "./src/frameworks/feature-mgmt",\
            "package": "feature-mgmt",\
            "versionName": "Version 1.0.6",\
            "versionNumber": "1.0.6.NEXT",\
            "dependencies": [\
                {\
                    "package": "sfdc-logging",\
                    "versionNumber": "1.0.2.LATEST"\
                }\
            ]\
        },\
        {\
            "path": "./src/core-crm",\
            "package": "core-crm",\
            "versionName": "Version 1.0.4",\
            "versionNumber": "1.0.4.NEXT",\
            "dependencies": [\
                {\
                    "package": "feature-mgmt",\
                    "versionNumber": "1.0.6.LATEST"\
                }\
            ]\
        }\
    ],
    "namespace": "",
    "sfdcLoginUrl": "https://login.salesforce.com",
    "sourceApiVersion": "60.0",
    "packageAliases": {
        "feature-mgmt": "0Ho5f000000GmkrCAC",
        "sfdc-logging": "0Ho5f000000GmerCAC",
        "core-crm": "0Ho5f000000Amz7CAC"
    },
    "plugins": {}
}
```

The above project manifest (sfdx-project.json) describes three packages, **sfdc-logging, feature-mgmt., core-crm .** Each package are defined with dependencies as described below

Package

Incorrectly Defined Dependencies

sfdc-logging

None

feature-mgmt

sfdc-logging

core-crm

feature-mgmt

As you might have noticed, this is an incorrect representation, as per the definitions of unlocked package, the package 'core-crm' should be explicitly defining all its dependencies. This means it should be as described below.

Package

Correctly Defined Dependencies

sfdc-logging

None

feature-mgmt

sfdc-logging

core-crm

sfdc-logging, feature-mgmt

To successfully create a version of core-crm , both sfdc-logging and feature-mgmt. should be defined as an explicit dependency in the sfdx-project.json

As the number of packages grow in your project, it is often seen developers might accidentally miss declaring dependencies or the sfdx-project.json has become so large due to large amount of repetition of dependencies between packages. This condition would result in build stage often failing with missing dependencies.

sfp features a transitive dependency resolution which can **autofill** the dependencies of the package by inferring the dependencies from sfdx-project.json, so the above package descriptor of core-crm will be resolved correctly to

Copy

```inline-grid
 {
            "path": "./src/core-crm",
            "package": "core-crm",
            "versionName": "Version 1.0.6",
            "versionNumber": "1.0.6.NEXT",
            "dependencies": [\
                  {\
                    "package": "sfdc-logging",\
                    "versionNumber": "1.0.2.LATEST"\
                },\
                {\
                    "package": "feature-mgmt",\
                    "versionNumber": "1.0.6.LATEST"\
                }\
            ]
        },

```

Please note, in the current iteration, it will autofill dependency information from the current sfdx-project.json and doesn't consider variations among previous versions.

For dependencies outside of the sfdx-project.json, one could define an externalDependencyMap as shown below

Copy

```inline-grid
 "plugins": {
        "sfp": {
            "disableTransitiveDependencyResolver": true,
            "ignoreFiles": {
                "prepare": ".forceignore",
                "validate": ".forceignore",
                "quickbuild": "forceignores/.quickbuildignore",
                "build": "forceignores/.buildignore"
            },
            "externalDependencyMap": {
                "trigger-framework": [\
                    {\
                        "package": "0H1000XRTCam",\
                        "versionNumber": "1.0.3.LATEST"\
                    }\
                ],
          }
      }
}
```

If you need to disable this feature and have stringent dependency management rules, utilize the following in your sfdx-project.json

An external dependency is a package that is not defined within the current repositories sfdx-project.json. Managed packages and unlocked packages built from other repositories fall into 'external dependency' bucket. IDs of External packages have to be defined explicitly in the **packageAliases** section.

Copy

```inline-grid
"plugins": {
        "sfp": {
            "disableTransitiveDependencyResolver": true,
        }
    }
```

[PreviousDependency management](https://docs.flxbl.io/sfp/concepts/dependency-management) [NextDestructive Changes](https://docs.flxbl.io/sfp/concepts/destructive-changes)

Last updated 3 months ago

Was this helpful?

### Destructive Changes in Salesforce

sfp handles destructive changes according to the type of package. Here is a rundown on how the behaviour is according to various package types and modes

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/destructive-changes#unlocked-packages) Unlocked Packages

Salesforce handles destructive changes in unlocked packages / org dependent unlocked packages as part of the package upgrade process.

From the Salesforce documentation ( [https://developer.salesforce.com/docs/atlas.en-us.sfdx\_dev.meta/sfdx\_dev/sfdx\_dev\_unlocked\_pkg\_install\_pkg\_upgrade.htm?q=delete+metadata](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_install_pkg_upgrade.htm?q=delete+metadata))

_Metadata that was removed in the new package version is also removed from the target org as part of the upgrade. Removed metadata is metadata not included in the current package version install, but present in the previous package version installed in the target org. If metadata is removed before the upgrade occurs, the upgrade proceeds normally. Some examples where metadata is deprecated and not deleted are:_

* _User-entered data in custom objects and fields are deprecated and not deleted. Admins can export such data if necessary._
* _An object such as an Apex class is deprecated and not deleted if it’s referenced in a Lightning component that is part of the package._

sfp utilizes `mixed` mode while installing unlocked packages to the target org. So any metadata that can be deleted is removed from the target org. If the component is deprecated, it has to be manually removed.\
Components that are hard deleted upon a version upgrade is found [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_hard_deleted_components.htm).

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/destructive-changes#source-packages) Source Packages

Source packages support destructive changes using folder structure to demarcate components that need to be deleted. One can make use of `pre-destructive and` \` `post-destructive` folders to mark components that need to be deleted

Copy

```inline-grid
// Consider a source package feature-management
// with path as src/feature-management

└── feature-management
    ├── main
    ├──── default
    ├────────  <metadata-contents>
    ├── post-destructive
    ├────────  <metadata-contents>
    ├── pre-destructive
    ├────────  <metadata-contents>
    └── test
```

The package installation is a single deployment transaction with components that are part of pre/post deployed along with destructive operation as specified in the folder structure. This would allow one to refactor the current code to facilitate refactoring for the destructive changes to succeed, as often deletion is only allowed if there are no existing components in the org that have a reference to the component that is being deleted

Destructive Changes support for source package is currently available only in **sfp (pro)** version.

#### [Direct link to heading](https://docs.flxbl.io/sfp/concepts/destructive-changes#data-packages) Data Packages

Data packages utilize sfdmu under the hood, and one can utilize any of the below approaches to remove data records.

**Approach 1: Combined Upsert and Delete Operations**

One effective method involves configuring SFDMU to perform both upsert and delete operations in sequence for the same object. This approach ensures comprehensive data management—updating and inserting relevant records first, followed by removing outdated entries based on specific conditions.

**Upsert Operation**: Updates or inserts records based on a defined external ID, aligning the Salesforce org with new or updated data from a source file.

Copy

```inline-grid
{
  "name": "CustomObject__c",
  "operation": "Upsert",
  "externalId": "External_Id__c",
  "query": "SELECT Id, Name, IsActive__c FROM CustomObject__c WHERE SomeCondition = true"
}
```

**Delete Operation**: Deletes specific records that meet certain criteria, such as being marked as inactive, to ensure the org only contains relevant and active data.

Copy

```inline-grid
{
  "name": "CustomObject__c",
  "operation": "Delete",
  "query": "SELECT Id FROM CustomObject__c WHERE IsActive__c = false"
}
```

**Approach 2: Utilizing** `deleteOldData`

Another approach involves using the `deleteOldData` parameter. This parameter is particularly useful when needing to clean up old data that no longer matches the current dataset in the source before inserting or updating new records.

* **Delete Old Data**: Before performing data insertion or updates, SFDMU can be configured to remove all existing records that no longer match the new dataset criteria, thus simplifying the maintenance of data freshness and relevance in the target org

Copy

```inline-grid
// Use of deleteOldData
{
  "name": "CustomObject__c",
  "operation": "Upsert",
  "externalId": "External_Id__c",
  "deleteOldData": true
}

```

[PreviousTransitive Dependency Resolution](https://docs.flxbl.io/sfp/concepts/transitive-dependency-resolution) [NextProject structure](https://docs.flxbl.io/sfp/configuring-a-project/project-structure)

Last updated 10 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Project Structure Overview

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FBWaAdy7IPrZmNq989QDC%252Fimage.png%3Falt%3Dmedia%26token%3Dae75dc87-1518-4978-ac53-3d9671550ea6\&width=768\&dpr=4\&quality=100\&sign=2f557631\&sv=2)

A sample repository structure used with sfp

Projects that utilise **sfp** predominantly follow a mono-repo structure similar to the picture shown above. Each repository has a " **src"** folder that holds one or more packages that map to your **sfdx-project.json** file.

Different folders in each of the structure are explained as below:

1. **core-crm:** A folder to house all the core model of your org which is shared with all other domains.
2. **frameworks:** This folder houses multiple packages which are basically utilities/technical frameworks such as Triggers, Logging and Error Handling, Dependency Injection etc.
3. **sales:** An example of a domain in your org. Under this particular domain, multiple packages that belong to the domain are included.
4. **src-access-mgmt:** This package is typically one of the packages that is deployed second to last in the deployment order and used to store profiles, permission sets, and permission set groups that are applied across the org. Permission Sets and Permission Set Groups particular to a domain should be in their respective package directory.
5. **src-env-specific:** An aliasified package which carries metadata for each particular stage (environment) of your path to production. Some examples include named credentials, remote site settings, web links, custom metadata, custom settings, etc.
6. **src-temp:** This folder is marked as the default folder in `sfdx-project.json`. This is the landing folder for all metadata and this particular folder doesn't get deployed anywhere other than a developers scratch org. This place is utilized to decide where the new metadata should be placed into.
7. **src-ui:** Should include page layouts, flexipages and Lightning/Classic apps unless we are sure these will only reference the components of a single domain package and its dependencies. In general, custom UI components such as LWC, Aura and Visualforce should be included in a relevant domain package.
8. **runbooks:** This folder stores markdown files required for each release and or sandbox refresh to ensure all manual steps are accounted for and versioned control. As releases are completed to production, each release run book can be archived as the manual steps should typically no longer be required. Sandbox refresh run books should be managed accordingly to the type of sandbox depending if they have data or only contain metadata.
9. **scripts:** This optional folder is to store commonly used APEX or SOQL scripts that need to be version controlled and reference by multiple team members.

src-env-specific should be added to .forceignore files and should not be deployed to a scratch org.

[PreviousDestructive Changes](https://docs.flxbl.io/sfp/concepts/destructive-changes) [NextSetup Salesforce Org](https://docs.flxbl.io/sfp/configuring-a-project/setup-salesforce-org)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce Org Setup

To fully leverage the capabilities of sfp, a few addition steps need to be configured in your Salesforce Orgs. Please follow the following steps.

### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/setup-salesforce-org#id-1.-enable-dev-hub) 1. Enable Dev Hub

To enable modular package development, the following configurations need to be turned on in order to create Scratch Orgs and Unlock Packages.

[Enable Dev Hub](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_setup_enable_devhub.htm) in your Salesforce org so you can create and manage scratch orgs and second-generation packages. Scratch orgs are disposable Salesforce orgs to support development and testing.

1. Navigate to the **Setup** menu
2. Go to **Development > Dev Hub**
3. Toggle the button to on for **Enable Dev Hub**
4. **Enable Unlocked Packages and Second-Generation Managed Packages**

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252Fe0xiVImkulJX4XXBkALL%252Fimage.png%3Falt%3Dmedia%26token%3Dec0ef7af-0fc0-4355-8837-2742fbbb194e\&width=768\&dpr=4\&quality=100\&sign=ee717251\&sv=2)

Enable Dev Hub

### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/setup-salesforce-org#id-2.-install-sfpowerscripts-artifact-unlocked-package) 2. Install sfpowerscripts-artifact Unlocked Package

The sfpowerscripts-artifact package is a lightweight unlocked package consisting of a custom setting **SfpowerscriptsArtifact2\_\_c** that is used to keep a record of the artifacts that have been installed in the org. This enables package installation, using sfp, to be skipped if the same artifact version already exists in the target org.

Copy

```inline-grid
sf package install --package 04t1P000000ka9mQAA -o <your_org_alias> --security-type=AdminsOnly --wait=120

Waiting 120 minutes for package install to complete.... done
Successfully installed package [04t1P000000ka9mQAA]
```

Once the command completes, confirm the unlocked package has been installed.

1. Navigate to the **Setup** menu
2. Go to **Apps > Packaging > Installed Packages**
3. Confirm the package **sfpowerscripts-artifact** is listed in the " **Installed Packages**"

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FtBtmljD9Kckai6wwAOfk%252Fimage.png%3Falt%3Dmedia%26token%3Dc52634f4-c1d2-4cc3-b2d3-a91a9fc9c99b\&width=768\&dpr=4\&quality=100\&sign=11807571\&sv=2)

sfpowerscripts-artifact

Ensure that you install sfpowerscripts artifact unlocked package in all your target orgs that you intend to deploy using sfp.

If refreshing from Production with **sfpowerscripts-artifact** already installed, you do not need to install again to your sandboxes.

[PreviousProject structure](https://docs.flxbl.io/sfp/configuring-a-project/project-structure) [NextCreating a package](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Creating Salesforce Packages

A package is a collection of metadata grouped together in a directory, and defined by an entry in your sfdx-project.json ( Project Manifest).

Copy

```inline-grid
// A sample sfdx-project.json with a packag
{
  "packageDirectories": [\
    {\
      "path": "src-env-specific-pre",\
      "package": "env-specific-pre",\
      "versionNumber": "4.7.0.NEXT",\
    },\
     ...\
   ]
}
```

Each package in the context of sfp need to have the following attributes as the absolute minimum\\

Attribute

Required

path

yes

Path to the directory that contains the contents of the package

package

yes

The name of the package

versionNumber

yes

The version number of the package

versionDescription

no

Description for a particular version of the package

sfp will not consider any entries in your sfdx-project.json for its operations if it is missing 'package' or 'versionNumber' attribute,

By default, sfp treats all entries in sfdx-project.json as [Source Packages](https://docs.flxbl.io/sfp/concepts/supported-package-types/source-packages). If you need to create an unlocked or an org depednent unlocked package, you need to proceed to create the packages using the follwing steps detailed below

sfp-pro users can create a source package by using the following command,`sfp package create source -n "my-source-package" --domain "my-domain" -r "path"`

#### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package#creating-an-unlocked-package) Creating an Unlocked Package

[**Direct link to heading**](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package#creating-an-unlocked-package-with-sf-cli) **Creating an Unlocked Package with SF CLI**

To create an unlocked package using the Salesforce CLI, follow the steps below:

1. **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
2. **Create a Package**: If the package does not already exist, create it with the command:

Copy

```inline-grid
sf package:create --name <package_name> --packagetype Unlocked  --nonamespace -o <alias_for_org>
```

Replace `<package_name>` with the name of your unlocked package, with the package directory specified in your `sfdx-project.json`, and `<alias_for_org>` with the alias for your Salesforce org.

3. Ensure that an entry for your package is created in your packageAliases section. Please commit the updated sfdx-project.json to your version control, before proceeding with sfp commands

sfp-pro users can create an unlocked package by using the following command,`sfp package create unlocked -n "my-unlocked-package" --domain "my-domain" -r "path" -v devhub`

#### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package#creating-an-org-dependent-unlocked-package) Creating an Org Dependent Unlocked Package

[**Direct link to heading**](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package#creating-an-org-dependent-unlocked-package-with-sf-cli) **Creating an Org Dependent Unlocked Package with SF CLI**

To create an unlocked package using the Salesforce CLI, follow the steps below:

1. **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
2. **Create a Package**: If the package does not already exist, create it with the command:

Copy

```inline-grid
sf package:create --name <package_name> --packagetype Unlocked  --org-dependent      --nonamespace -o <alias_for_org>
```

Replace `<package_name>` with the name of your unlocked package, with the package directory specified in your `sfdx-project.json`, and `<alias_for_org>` with the alias for your Salesforce org.

3. Ensure that an entry for your package is created in your packageAliases section. Please commit the updated sfdx-project.json to your version control, before proceeding with sfp commands

sfp-pro users can create an org dependent unlocked package by using the following command,`sfp package create unlocked -n "my-unlocked-package" --domain "my-domain" -r "path" --orgdepedendent -v devhub`

#### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package#creating-a-data-package) Creating a data package

* **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
* Ensure your package directory is populated with an export-json and the required CSV files. Read on [here](https://docs.flxbl.io/sfp/concepts/supported-package-types/data-packages) to learn more about data packages
* **Add an additional attribute of "type":"data"**\
  \\

Copy

```inline-grid
      {
      "path": "path--to--package",
      "package": "name--of-the-package",
      "versionNumber": "X.Y.Z.[NEXT/BUILDNUMBER]",
      "type": "data", // required for data packages
      }
```

sfp-pro users can create a diff package by using the following command,`sfp package create data -n "my-source-package" -r "path" --domain "my-domain"`

#### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package#creating-a-diff-package) Creating a diff package

* **Identify the Package Directory**: Ensure that your `sfdx-project.json` contains an entry for the package you wish to turn into an unlocked package. The entry must include `path`, `package`, and `versionNumber`.
* Ensure your package directory is populated with an export-json and the required CSV files. Read on [here](https://docs.flxbl.io/sfp/concepts/supported-package-types/diff-package) to learn more about diff packages
* **Add an additional attribute of "type":"diff"**\
  \\

Copy

```inline-grid
      {
      "path": "path--to--package",
      "package": "name--of-the-package",
      "versionNumber": "X.Y.Z.[NEXT/BUILDNUMBER]",
      "type": "diff", // required for data packages
      }
```

* Ensure a new record is created in SfpowerscriptsArtifact2\_\_c object in your devhub, with the details such as the name of the package, the initial version number, the baseline commit id

sfp-pro users can create a diff package by using the following command,`sfp package create diff -n "my-diff-package" -r "path" -c "commit-id" --domain "my-domain" -v devhub`

[PreviousSetup Salesforce Org](https://docs.flxbl.io/sfp/configuring-a-project/setup-salesforce-org) [NextDefining a domain](https://docs.flxbl.io/sfp/configuring-a-project/defining-a-domain)

Last updated 3 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Defining a Domain

A domain is defined by a release configuration. In order to define a domain, you need to create a new [release config](https://docs.flxbl.io/sfp/configuring-a-project/release-config) yaml file in your repository

A simple release config can be defined as shown below

Copy

```inline-grid
// Sample release config <business_domain.yaml>
releaseName: <business_domain>   # --> The name of the domain
pool: <sandbox/scratch org pools>
excludeAllPackageDependencies: true
includeOnlyArtifacts:   # --> Insert packages
  - <pkg1>
releasedefinitionProperties:
  promotePackagesBeforeDeploymentToOrg: prod
```

The resulting file could be stored in your repository and utilized by the commands such as build, release etc.

[PreviousCreating a package](https://docs.flxbl.io/sfp/configuring-a-project/creating-a-package) [NextRelease Config](https://docs.flxbl.io/sfp/configuring-a-project/release-config)

Last updated 1 year ago

Was this helpful?

### Release Configuration Guide

Release configuration is a fundamental setup that outlines the organisation of packages within a project, streamlining across different lifecycle of your project, such as validating, building, deploying/release of artifacts. In flxbl projects, a release config is used to define the concept of a domain/subdomains.

This configuration is instrumental when using sfp commands, as it allows for selective operations on specified packages defined by a configuration. By employing a release configuration, teams can efficiently manage a mono repository of packages across various teams.

Copy

```inline-grid
---
​releaseName: core
pool: core_pool
includeOnlyArtifacts:
  - src-env-specific-pre
  - src-env-specific-alias-pre
  - core-crm
  - telephony-service
excludePackageDependencies:
  - Genesys Cloud for Salesforce
  - Marketing Cloud
releasedefinitionProperties:
  changelog:
    workItemFilters:
      -  BRO-[0-9]{3,4}
    workItemUrl: https://bro.atlassian.net/browse
    limit: 30
```

The below table list the options that are currently available for release configuration

Parameter

Required

Type

Description

releaseName

No

String

Name of the release config, in flxbl project, this name is used as the name of the domain

pool

No

String

Name of the scratch org or sandbox pool associated with this release config during validation

excludeArtifacts

No

Array

An array of artifacts that need to be excluded while creating the release definition

includeOnlyArtifacts

No

Array

An array of artifacts that should only be included while creating the release definition

dependencyOn

No

Array

An array of packages that denotes the dependency this configuration has. The dependencies mentioned will be used for synchronization in review sandboxes

excludePackageDependencies

No

Array

Exclude the mentioned package dependencies from the release definition

includeOnlyPackageDependencies

No

Array

Include only the mentioned package dependencies from the release definition

releasedefinitionProperties

No

Object

Properties of release definition that should be added to the generated release definition. See below

A release configuration also can contain additional options that can be used by certain sfp commands to generate [release definitions](https://docs.flxbl.io/sfp/releasing-artifacts/release-definitions). These properties in a release definiton alters the behaviour of deployment of artifacts during a release

#### [Direct link to heading](https://docs.flxbl.io/sfp/configuring-a-project/release-config#release-definition-properties) Release Definition Properties

Parameter

Required

Type

Description

releasedefinitionProperties.skipIfAlreadyInstalled

No

boolean

Skip installation of artifact if it's already installed in target org

releasedefinitionProperties.baselineOrg

No

string

The org used to decide whether to to skip installation of an artifact. Defaults to the target org when not provided

releasedefinitionProperties.promotePackagesBeforeDeploymentToOrg

No

string

Promote packages before they are installed into an org that matches alias of the org

releasedefinitionProperties.changelog.repoUrl

No

Prop

The URL of the version control system to push changelog files

releasedefinitionProperties.changelog.workItemFilters

No

Prop

An array of regular expression used to identify work items in your commit messages

releasedefinitionProperties.changelog.workitemUrl

No

Prop

The generic URL of work items, to which to append work item codes. Allows easy redirection to user stories by clicking on the work-item link in the changelog.

releasedefinitionProperties.changelog.limit

No

Prop

Limit the number of releases to display in the changelog markdown

releasedefinitionProperties.changelog.showAllArtifacts

No

Prop

Whether to show artifacts that haven't changed between releases

[PreviousDefining a domain](https://docs.flxbl.io/sfp/configuring-a-project/defining-a-domain) [NextOverview](https://docs.flxbl.io/sfp/building-artifacts/overview)

Last updated 11 months ago

Was this helpful?

### SFP Build Artifacts

sfp cli features an intiutive command to build artifacts of all your packages in your project directory. The 'build' command automatically detects the type of package and builds an artifact individually for each package

Copy

```inline-grid
sfp build -v <devhub_name> --branch <value>
```

By default, the behaviour of sfp's build command is to build a new version of all packages in your project directory and and creates it associated artifact.

sfp's build command is also equipped with an ability to selectively build only packages that are changed. Read more on how sfp determines a package to be built on the subsequent sections.

Copy

```inline-grid
sfp build -v <devhub_name> --branch <value>  --diffcheck
```

[PreviousRelease Config](https://docs.flxbl.io/sfp/configuring-a-project/release-config) [NextDetermining whether an artifact need to be built](https://docs.flxbl.io/sfp/building-artifacts/determining-whether-an-artifact-need-to-be-built)

Last updated 1 year ago

Was this helpful?

### Artifact Build Determination

sfp's build commands process the packages in the order as mentioned in your sfdx-project.json. The commands also read your dependencies property, and then when triggered, will wait till all its dependencies are resolved, before triggering the equivalent package creation command depending on the type of the package and generating an artifact in the due process

sfp's build command is equipped with a diffcheck functionality, which is enabled when one utilizes **diffcheck** flag, A comparison (using git diff) is made between the latest source code and the previous version of the package published by the ' [publish](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact)' command. If any difference is detected in the **package directory**, **package version** or **scratch org definition file** (applies to unlocked packages only), then the package will be created - otherwise, it is skipped.

For eg: provided the followings packages in sfdx-project.json along with its dependencies

**Scenario 1 : Build All**

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FLsgaOamkqa8uHbRG6IYb%252Fimage.png%3Falt%3Dmedia%26token%3Dad7cdebc-85fd-47f1-aeff-7f15a82fb7cc\&width=768\&dpr=4\&quality=100\&sign=abe518f4\&sv=2)

1. Trigger creation of artifact for package A
2. Once A is completed, trigger creation of artifacts for packages B & C \*\*,\*\*using the version of A, created in step 1
3. Once C is completed, trigger creation of package D

**Scenario 2 : Build with diffCheck enabled on a package with no dependencies**

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FFmbgH0xjbieUugbD7VjM%252Fimage.png%3Falt%3Dmedia%26token%3D077dc1ba-bfed-4a58-890e-6432844b189a\&width=768\&dpr=4\&quality=100\&sign=f028ff55\&sv=2)

In this scenario, where only a single package has changed and **diffCheck** is enabled, the build command will only trigger the creation of Package B

**Scenario 3 : Build with diffCheck enabled on changes in multiple packages**

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FTO4tAq0AGZcRmOEmIvMi%252Fimage.png%3Falt%3Dmedia%26token%3D4d01c275-bd1e-49e1-a761-efe06225f6ab\&width=768\&dpr=4\&quality=100\&sign=c40e477b\&sv=2)

In this scenario, where there are changes in multiple packages, say B & C, the build command will trigger the creation of artifacts for these packages in parallel, as their dependent package A has not changed (hence fulfilled). Please note even though there is a change in C, package D will not be triggered, unless there is an explicit version change of version number (major.minor.patch) of package D

[PreviousOverview](https://docs.flxbl.io/sfp/building-artifacts/overview) [NextBuilding a domain](https://docs.flxbl.io/sfp/building-artifacts/building-a-domain)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Building Artifacts Guide

Assume you have a domain 'sales' as defined by release config `sales.yaml` provided as shown in this example here.

Copy

```inline-grid
# release-config for sales
releaseName: sales
pool: sales-pool
excludeAllPackageDependencies: true
includeOnlyArtifacts:
  - sales-ui
  - sales-channels
releasedefinitionProperties:
  skipIfAlreadyInstalled: true
  usePackageDiffDeploymentMode: true
  promotePackagesBeforeDeploymentToOrg: prod
  changelog:
    workItemFilters:
      -  (AKG|GIK)-[0-9]{2,5}
    workItemUrl: https://example.atlassian.net/browse
    limit: 30
```

In order to build the artifact of the packages defined by the above release config, you would use the the build command with the flags as described here.

Copy

```inline-grid
sfp build --releaseconfig sales.yaml -v devhub --branch main
```

If you require only to build packages that's changed form the last published packages, you would add an additional diffcheck flag.

Copy

```inline-grid
sfp build --releaseconfig sales.yaml -v devhub --branch main  --diffcheck
```

diffcheck will work accurately only if the build command is able to access the latest tag in the repository. In certain CI system if the command is operated on a repository where only the head commit is checked out, diffchek will result in building all the artifacts for all packages within the domain

[PreviousDetermining whether an artifact need to be built](https://docs.flxbl.io/sfp/building-artifacts/determining-whether-an-artifact-need-to-be-built) [NextBuilding an artifact for package individually](https://docs.flxbl.io/sfp/building-artifacts/building-an-artifact-for-package-individually)

Last updated 1 year ago

Was this helpful?

### Building Artifacts Individually

An artifact can be build individually for a package, sfp will determine the [type of a package](https://docs.flxbl.io/sfp/concepts/identifying-types-of-a-package) and the corresponding api's will be invoked

Copy

```inline-grid
// Sample sfdx project json
{
  "packageDirectories": [\
    {\
      "path": "./src-env-specific-pre",\
      "package": "src-env-specific-pre",\
      "versionNumber": "1.0.0.0",\
    },\
    {\
      "path": "./src/frameworks/feature-mgmt",\
      "package": "feature-mgmt",\
      "versionNumber": "1\
    }\
  ]
}
```

Copy

```inline-grid
// Build a single package
sfp build -v devhub --branch=main -p feature-mgmt
```

[PreviousBuilding a domain](https://docs.flxbl.io/sfp/building-artifacts/building-a-domain) [NextLimiting artifacts to be built](https://docs.flxbl.io/sfp/building-artifacts/limiting-artifacts-to-be-built)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Limiting Built Artifacts

Artifacts to be built can be limited by various mechanisms. This section deals with various techniques to limit artifacts being built

#### [Direct link to heading](https://docs.flxbl.io/sfp/building-artifacts/limiting-artifacts-to-be-built#limiting-artifacts-by-domain) Limiting artifacts by domain

Artifacts to be built can be restricted by [domain](https://docs.flxbl.io/sfp/concepts/domains) during a build process in **sfp** by utilizing specific configurations. Consider the [release config](https://docs.flxbl.io/sfp/configuring-a-project/release-config) example provided in

Copy

```inline-grid
# release-config for sales
releaseName: sales
pool: sales-pool
excludeAllPackageDependencies: true
includeOnlyArtifacts:
  - sales-ui
  - sales-channels
releasedefinitionProperties:
  skipIfAlreadyInstalled: true
  usePackageDiffDeploymentMode: true
  promotePackagesBeforeDeploymentToOrg: prod
  changelog:
    workItemFilters:
      -  (AKG|GIK)-[0-9]{2,5}
    workItemUrl: https://example.atlassian.net/browse
    limit: 30
```

You can use the path to the config file to the build command to limit the artifacts being built as in the sample below

Copy

```inline-grid
// Limit build by release config
sfp build -v devhub --branch=main --domain config/sales.yaml
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/building-artifacts/limiting-artifacts-to-be-built#limiting-artifacts-by-packages) Limiting artifacts by packages

Artifacts to be built can be limited only to certain packages. This could be very helpful, in case you want to build an artifact of only one package or a few while testing locally.

Copy

```inline-grid
// Limit build by certain packages
sfp build -v devhub --branch=main -p sales-ui,sales-channels
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/building-artifacts/limiting-artifacts-to-be-built#limiting-artifacts-by-ignoring-packages) Limiting artifacts by ignoring packages

Packages can be ignored from the build command by utilising an additional descriptor on the package in project manifest (sfdx-project.json)

Copy

```inline-grid
// Sample sfdx project json
{
  "packageDirectories": [\
    {\
      "path": "./src-env-specific-pre",\
      "package": "src-env-specific-pre",\
      "versionNumber": "1.0.0.0",\
      "ignoreOnStage": [\
        "build"\
      ]\
    },\
    {\
      "path": "./src/frameworks/feature-mgmt",\
      "package": "feature-mgmt",\
      "versionNumber": "1.0.0.NEXT"\
    }\
  ]
}
```

In the above scenario, src-env-specific-pre will be ignored while build command is invoked

[PreviousBuilding an artifact for package individually](https://docs.flxbl.io/sfp/building-artifacts/building-an-artifact-for-package-individually) [NextControlling aspects of the build command](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command)

Last updated 1 year ago

Was this helpful?

### Build Command Control

sfp provides mechanisms to control the aspects of the build command, the following section details how one can configure these mechanisms in your sfdx-project.json

[Ignoring packages from being built](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/ignoring-packages-from-being-built) [Building a collection of packages together](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/building-a-collection-of-packages-together) [Selective ignoring of components from being built](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/selective-ignoring-of-components-from-being-built) [Use of multiple config file in build command](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/use-of-multiple-config-file-in-build-command)

[PreviousLimiting artifacts to be built](https://docs.flxbl.io/sfp/building-artifacts/limiting-artifacts-to-be-built) [NextIgnoring packages from being built](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/ignoring-packages-from-being-built)

Last updated 1 year ago

Was this helpful?

### Ignoring Packages in Builds

Attribute

Type

Description

Package Types Applicable

ignoreOnStage

array

Ignore a package from being processed by a particular stage

* unlocked
* org-dependent unlocked
* source
* diff

Using the `ignoreOnStage:[ "build" ]` property on a package, causes the particular package to be skipped by the **build** command. Similarly you can use `ignoreOnStage:[ "quickbuild" ]` to skip packages in the quickbuild stage.

Copy

```inline-grid
{
  "packageDirectories": [\
    {\
      "path": "./src-env-specific-pre",\
      "package": "src-env-specific-pre",\
      "versionNumber": "1.0.0.NEXT",\
      "ignoreOnStage": [\
       "build"\
      ]\
    },\
    {\
      "path": "./src/frameworks/feature-mgmt",\
      "package": "feature-mgmt",\
      "versionNumber": "1.0.0.NEXT"\
    }\
  ]
}
```

[PreviousControlling aspects of the build command](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command) [NextBuilding a collection of packages together](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/building-a-collection-of-packages-together)

Last updated 1 year ago

Was this helpful?

### Building Package Collections

Attribute

Type

Description

Package Types Applicable

ignoreOnStage

array

Ignore a package from being processed by a particular stage

* unlocked
* org-dependent unlocked
* source
* diff

In certain scenarios, it's necessary to build a new version of a package when any package in the collection undergoes changes. This can be accomplished by utilizing the `buildCollection` attribute in the `sfdx-project.json` file.

To ensure that a new version of a package is built whenever any package in a specified collection undergoes a change, you can use the `buildCollection` attribute in the `sfdx-project.json` file. Below is an example illustrating how to define a collection of packages that should be built together.

Copy

```inline-grid
{
  "packageDirectories": [\
    {\
      "path": "core",\
      "package": "core-package",\
      "versionName": "Core 1.0",\
      "versionNumber": "1.0.0.NEXT",\
      "default": true,\
      "buildCollection": [\
        "core-package",\
        "featureA-package",\
        "featureB-package"\
      ]\
    },\
    {\
      "path": "features/featureA",\
      "package": "featureA-package",\
      "versionName": "Feature A 1.0",\
      "versionNumber": "1.0.0.NEXT"\
       "buildCollection": [\
        "core-package",\
        "featureA-package",\
        "featureB-package"\
      ]\
    },\
    {\
      "path": "features/featureB",\
      "package": "featureB-package",\
      "versionName": "Feature B 1.0",\
      "versionNumber": "1.0.0.NEXT",\
      "buildCollection": [\
        "core-package",\
        "featureA-package",\
        "featureB-package"\
      ]\
    }\
  ],
}
```

[PreviousIgnoring packages from being built](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/ignoring-packages-from-being-built) [NextSelective ignoring of components from being built](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/selective-ignoring-of-components-from-being-built)

Last updated 1 year ago

Was this helpful?

### Selective Ignoring of Components

Sometimes, due to certain platform errors, some metadata components need to be ignored during **build**(especially for unlocked packages) while the same being required forother commands like [validat](https://docs.flxbl.io/sfp/command-guide/advanced/validate) [e](https://docs.flxbl.io/sfp/command-guide/advanced/validate). sfp offer you an easy mechanism which allows to switch .forceignore files depending on the operation.

Add this entry to your sfdx-project.json and as in the example below, mention the path to different files that need to be used for different stages

Copy

```inline-grid
 {
  "packageDirectories": [\
    {\
      "path": "core",\
      "package": "core-package",\
      "versionName": "Core 1.0",\
      "versionNumber": "1.0.0.NEXT",\
      "default": true,\
    }\
  ],
  "plugins": {
        "sfp": {
            "ignoreFiles": {
                "build": "forceignores/.buildignore"
                "validate": ".forceignore"
            }
   }
 }
```

[PreviousBuilding a collection of packages together](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/building-a-collection-of-packages-together) [NextUse of multiple config file in build command](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/use-of-multiple-config-file-in-build-command)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Multiple Config Files

The `configFile` flag in the `build` command is used for features and settings of the scratch org used to validate your unlocked package. It is optional and if not passed in, it will assume the default one which is none.

Typically, all packages in the same repo share the same scratch org definition. Therefore, you pass in the definition that you are using to build your scratch org pool and use the same to build your unlocked package.

Copy

```inline-grid
sfp build --configFile <path-to-config-file> -v <devhub>
```

However, there is an option to use multiple definitions.

For example, if you have two packages, package 1 is dependent on `SharedActivities`, and package 2 is not. You would pass a scratch org definition file to package 1 via `scratchOrgDefFilePaths` in `sfdx-project`. Package 2 would be using. the default `definitionFile`.

Copy

```inline-grid
{
  "packageDirectories": [\
    {\
      "path": "package1",\
      "default": true,\
    },\
    {\
      "path": "package2",\
      "default": false\
    }\
  ],
  "plugins": {
    "sfpowerscripts":{
      "scratchOrgDefFilePaths":{
        "enableMultiDefinitionFiles": true,
        "packages": {
          "package1":"scratchOrgDef/package1-def.json"
        }
      }
    }
  }
}
```

[PreviousSelective ignoring of components from being built](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/selective-ignoring-of-components-from-being-built) [NextConfiguring installation behaviour of a package](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package)

Last updated 1 year ago

Was this helpful?

### Configuring Package Installation

sfp provides various features to alter the installation behaviour of a package. These behaviours have to be applied as an additional property of a package during build time. The following section details each of the parameters that is available.

sfp is built on the concept of immutable artifacts, hence any properties to control the installation aspects of a package need to be applied during the build command. Installation behaviour of an package cannot be controlled dynamically. If you need to alter or add a new behaviour, please build a new version of the artifact

[PreviousUse of multiple config file in build command](https://docs.flxbl.io/sfp/building-artifacts/controlling-aspects-of-the-build-command/use-of-multiple-config-file-in-build-command) [NextAlways deploy a package](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/always-deploy-a-package)

Last updated 12 months ago

Was this helpful?

### Skip Install on Orgs

Attribute

Type

Description

Package Types Applicable

skipDeployOnOrgs

array

Skips installation of an artifact on a target org

* org-dependent unlocked
* unlocked
* data
* source
* diff

sfp cli when encounters the attribute `skipDeployOnOrgs` on a package, the generated artifact during installation is checked against the alias or the username passed onto the installation command. If the username or the alias is matched, the artifact installation is skipped

Copy

```inline-grid
// Demonstrating how to do use skipDeployOnOrgs
{
  "packageDirectories": [\
    {\
      "path": "core-crm",\
      "package": "core-crm",\
      "versionDescription": "Package containing core schema and classes",\
      "versionNumber": "4.7.0.NEXT",\
      "skipDeployOnOrgs":[\
       "qa",\
       "user@colaorg.com.au.sit\
      ]\
    },\
     ...\
   ]
}
```

In the above example, if the alias to installation command is qa, the artifact for core-crm will get skipped during intallation. The same can also be applied for username of an org as well

[PreviousAlways deploy a package](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/always-deploy-a-package) [NextOptimized Installation](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation)

Last updated 11 months ago

Was this helpful?

### Optimized Installation Guide

Attribute

Type

Description

Package Types Applicable

isOptimizedDeployment

boolean

Detects test classes in a source package automatically and utilise it to deploy the provided package

* source
* diff

sfp cli optimally deploys artifacts to the target organisation by reducing the time spent on running Apex tests where possible. This section explains how this optimisation works for different package types.

Package Type

Apex Test Execution during installation

Coverage Requirement

Unlocked Package

No

75% for the contents of a package validated during build

Org Dependent Unlocked Package

No

No

Source Package

Yes

Yes, either each classes need to have individual 75% coverage or use the entire org's apex test coverage

Diff Package

Yes

Yes, either each classes need to have individual 75% coverage or use the entire org's coverage

Data Package

No

No

To ensure salesforce deployment requirements for source packages, each Apex class within the source package must achieve a minimum of 75% test coverage. This coverage must be verified individually for each class. It's imperative that the test classes responsible for this coverage are included within the same package.

During the artifact preparation phase, sfp cli will automatically identify Apex test classes included within the package. These identified test classes will then be utilised at the time of installation to verify that the test coverage requirement is met for each Apex class, ensuring compliance with Salesforce's deployment standards.

When there are circumstances, where individual coverage cannot be achieved by the apex classes within a package, one can disable 'optimized deployment' feature by using the attribute mentioned below.

This option is only applicable to source/diff packages. Disabling this option will trigger the entire local tests in the org and can lead to considerable delays

Copy

```inline-grid
// Demonstrating how to disable optimized deployment
{
  "packageDirectories": [\
    {\
      "path": "core-crm",\
      "package": "core-crm",\
      "versionDescription": "Package containing core schema and classes",\
      "versionNumber": "4.7.0.NEXT",\
      "isOptimizedDeployment": false\
    },\
     ...\
   ]
}
```

[PreviousSkip Install on Certain Orgs](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/skip-install-on-certain-orgs) [NextPre/Post Deployment Script](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/pre-post-deployment-script)

Last updated 8 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Deployment Script Configuration

Attribute

Type

Description

Package Types Applicable

preDeploymentScript

string

Run an executable script before deploying an artifact. Users need to provide a path to the script file

* unlocked
* org-dependent unlocked
* source
* diff

postDeploymentScript

string

Run an executable script after deploying an package. Users need to provide a path to the script file

* unlocked
* org-dependent unlocked
* source
* diff

In some situations, you might need to execute a pre/post deployment script to do manipulate the data before or after being deployed to the org. **sfp** allow you to provide a path to a shell script (Mac/Unix) / batch script (on Windows).

The scripts are called with the following parameters. In your script you can refer to the parameters using [positional parameters.](https://linuxcommand.org/lc3_wss0120.php)

Please note scripts are copied into the artifacts and are not executed from version control. sfpowerscripts only copies the script mentioned by this parameter and do not copy any additional files or dependencies. Please ensure pre/post deployment scripts are independent or should be able to download its dependencies

Position

Value

1

Name of the Package

2

Username of the target org where the package is being deployed

3

Alias of the target org where the package is being deployed

4

Path to the working directory that has the contents of the package

5

Path to the package directory. One would need to combine parameter 4 and 5 to find the absolute path to the contents of the package

Copy

````inline-grid
// Sample package

{
  "packageDirectories": [\
      {\
      "path": "src/data-package-cl",\
      "package": "data-package-cloudlending",\
      "type": "data",\
      "versionNumber": "2.0.10.NEXT",\
      "preDeploymentScript": "scripts/enableEmailDeliverability.sh"\
      "postDeploymentScript": "scripts/pushData.sh"\
    }\
\
```\
\
Please note the script has to be completely independent and should not have dependency on a file in the version control, as scripts are executed within the context of an artifact.\
\
Copy\
\
```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]\
// A script to enable email deliverablity to All\
\
#!/bin/bash\
\
export SF_DISABLE_DNS_CHECK=true\
\
# Create a temporary file\
temp_file="$(mktemp)"\
\
# Write the anonymous Apex code to the temp file\
cat << EOT >> "$temp_file"\
{\
  "$schema": "https://raw.githubusercontent.com/amtrack/sfdx-browserforce-plugin/master/src/plugins/schema.json",\
  "settings": {\
    "emailDeliverability": {\
      "accessLevel": "All email"\
    }\
  }\
}\
EOT\
\
if [ "$3" = 'ci' ]; then\
# Execute the browserforce configuration to this org\
  sf browserforce:apply  -f "$temp_file" --target-org $3\
fi\
\
# Clean up by removing the temporary file\
rm "$temp_file"\
```\
\
[PreviousOptimized Installation](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation) [NextReconciling Profiles](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/reconciling-profiles)\
\
Last updated 8 months ago\
\
Was this helpful?\
\
This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).\
\
AcceptReject

## Reconcile Profiles Guide
Attribute

Type

Description

Package Types Applicable

reconcileProfiles

boolean

Reconcile profiles to only apply permissions to objects, fields and features that are present in the target org.

- source

- diff


In order to prevent deployment errors to target Salesforce Orgs, enabling **reconcileProfiles** to `true` ensures and extra steps are taken to ensure the profile metadata is cleansed of any missing attributes prior to deployment.

During the reconcilement process, you can provide the follow flags to the command:

- Folder - path to the folder which contains the profiles to be reconciled, if project contain multiple package directories, please provide a comma separated list, if

- Profile List - list of profiles to be reconciled. If omitted, all the profiles components will be reconciled.

- Source Only - set this flag to reconcile profiles only against component available in the project only. Configure ignored permissions in sfdx-project.json file in the array

- Destination Folder - the destination folder for reconciled profiles, if omitted existing profiles will be reconciled and will be rewritten in the current location


For more details on the `sfp profile reconcile` command, [click here](https://docs.flxbl.io/sfp/command-guide/utilities/profile).

Copy

```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]
// Sample package

{
  "packageDirectories": [\
      {\
      "path": "packages/access-mgmt",\
      "package": "access-mgmt",\
      "default": false,\
      "versionName": "access-mgmt",\
      "versionNumber": "1.0.0.0",\
      "reconcileProfiles": "true"\
    }\
\
```\
\
For more a more detailed understanding of the ProfileReconcile library, visit the GitHub repository " [sfprofiles](https://github.com/flxbl-io/sfprofiles)" for details of on the [`source`](https://github.com/flxbl-io/sfprofiles/blob/main/src/impl/source/profileReconcile.ts).\
\
[PreviousPre/Post Deployment Script](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/pre-post-deployment-script) [NextPermissionSet Assignment](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/permissionset-assignment)\
\
Last updated 1 month ago\
\
Was this helpful?

## Permission Set Assignment
Attribute

Type

Description

Package Types Applicable

assignPermSetsPreDeployment

array

Apply permsets before installing an artifact to the deployment user

- unlocked

- org-dependent unlocked

- source

- diff


assignPermSetsPostDeployment

array

Apply permsets after installing an artifact to the deployment user

- unlocked

- org-dependent unlocked

- source

- diff


Occasionally you might need to assign a permission set for the deployment user to successfully install the package, run apex tests or functional tests, sfp provides you an easy mechanism to assign permission set either before installation or after installation of an artifact.

**assignPermSetsPreDeployment** assumes the permission sets are already deployed on the target org and proceed to assign these permission sets to the user

Copy

```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]
 {
      "path": "src/health-cloud",
      "package": "health-cloud",
      "versionName": "health-cloud-1.0",
      "versionNumber": "1.0.0.0",
      "assignPermSetsPreDeployment": [\
        "HealthCloudFoundation",\
        "HealthCloudSocialDeterminants",\
        "HealthCloudAppointmentManagement",\
        "HealthCloudVideoCalls",\
        "HealthCloudUtilizationManagement",\
        "HealthCloudMemberServices",\
        "HealthCloudAdmin",\
        "HealthCloudApi",\
        "HealthCloudLimited",\
        "HealthCloudPermissionSetLicense",\
        "HealthCloudStandard",\
        "HealthcareAssociatedInfectionDiseaseGroupAccess"\
      ]
    },
````

**assignPermSetsPostDeployment** can be used to assign permission sets that are introduced by the artifact to the target org for any aspects of testing or any other automation usage

[PreviousReconciling Profiles](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/reconciling-profiles) [NextUpdating Picklist](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/updating-picklist)

Last updated 8 months ago

Was this helpful?

### Picklist Update Management

Attribute

Type

Description

Package Types Applicable

enablePicklist

boolean

Enable picklist upgrade for unlocked packages

* unlocked
* org dependent unlocked

Salesforce inherently does not support the deployment of picklist values as part of unlocked package upgrades. This limitation has led to the need for workarounds, traditionally solved by either replicating the picklist in the source package or manually adding the changes in the target organization. Both approaches add a burden of extra maintenance work. This issue has been documented and recognised as a known problem, which can be reviewed in detail [here](https://issues.salesforce.com/issue/a028c00000qPzYUAA0).

To ensure picklist consistency between local environments and the target Salesforce org, the following pre-deployment steps outline the process for managing picklists within unlocked packages:

1. **Retrieval and Comparison**: Initially, picklists defined within the packages marked for deployment will be retrieved from the target org. These retrieved values are then compared with the local versions of the picklists.
2. **Update on Change Detection**: Should any discrepancies be identified between the local and retrieved picklists, the differing values will be updated in the target org using the Salesforce Tooling API.
3. **Handling New Picklists**: During the retrieval phase, picklists that are present locally but absent in the target org are identified as newly created. These new picklists are deployed using the standard deployment process, as Salesforce permits the deployment of new picklists from unlocked packages.
4. **Picklist Enabled Package Identification**: Throughout the build process, the sfp cli examines the contents of each unlocked package artifact. A package is marked as 'picklist enabled' if it contains one or more picklist(s).

Copy

```inline-grid
// Demonstrating how to disable optimized deployment
{
  "packageDirectories": [\
    {\
      "path": "core-crm",\
      "package": "core-crm",\
      "versionDescription": "Package containing core schema and classes",\
      "versionNumber": "4.7.0.NEXT",\
      "enablePicklist": false\
    },\
     ...\
   ]
}
```

[PreviousPermissionSet Assignment](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/permissionset-assignment) [NextEntitlement Deployment Helper](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/entitlement-deployment-helper)

Last updated 8 months ago

Was this helpful?

### Entitlement Deployment Helper

Have you ever encountered this error when deploying an updated Entitlement Process to your org?

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A636%2F1*ekdOVguScwFGaPF0BcGdvg.png\&width=768\&dpr=4\&quality=100\&sign=3f940b7a\&sv=2)

[**Direct link to heading**](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/entitlement-deployment-helper#deployment-issue-with-entitlement-process-in-salesforce) **Deployment Issue with Entitlement Process in Salesforce**

It's important to note that Salesforce prevents the deployment of an Entitlement Process that is currently in use, irrespective of its activation status in the org. This limitation holds true even for inactive processes, potentially impacting deployment strategies.

To create a new version of an Entitlement Process, navigate to the Entitlement Process Detail page in Salesforce and perform the creation. If you then use sf cli to retrieve this new version and attempt to deploy it into another org, you're likely to encounter errors. Deploying it as a new version can bypass these issues, but this requires enabling versioning in the target org first.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A1202%2F1*_vOy9x2TpxEPHgX5r33vzg.png\&width=768\&dpr=4\&quality=100\&sign=4e62b7f9\&sv=2)

The culprit behind this is an attribute in the Entitlement Process metadata file: versionMaster.

According to the Salesforce documentation, versionMaster ‘identifies the sequence of versions to which this entitlement process belongs and it can be any value as long as it is identical among all versions of the entitlement process.’ An important factor here about versionMaster is: versionMaster is org specific. When you deploy a completely new Entitlement Process to an org with a randomly defined versionMaster, Salesforce will generate an ID for the Entitlement Process and map it to this specific versionMaster.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A804%2F1*07-ZYlYhk1d8RvJwuDUNWw.png\&width=768\&dpr=4\&quality=100\&sign=7749350b\&sv=2)

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A1088%2F1*hNIWSejsazCfoIsXrWMWRw.png\&width=768\&dpr=4\&quality=100\&sign=2a814c35\&sv=2)

Deploying updated Entitlement Processes from one Salesforce org to another can often lead to deployment errors due to discrepancies in the `versionMaster` attribute, sfp's enitlement helper enables you to automatically align the `versionMaster` by pulling its value from the target org and updating the deploying metadata file accordingly. This ensures that your deployment process is consistent and error-free.

1. **Enable Versioning**: First and foremost, activate versioning in the target org to manage various versions of the Entitlement Process.
2. **Create or Retrieve Metadata File**:

* Create a new version of the Entitlement Process as a metadata file, which can be done via the Salesforce UI and retrieved with the Salesforce CLI (sf cli).

3. **Ensure Metadata Accuracy**:

* **API Name**: Validate that the API name within the metadata file accurately reflects the new version.
* **Version Number**: Update the `versionNumber` in your metadata file to represent the new version clearly.
* **Default Version**: Confirm that only one version of the Entitlement Process is set as Default to avoid deployment conflicts.

Automation around entitlement filter can be disabled globally by using these attributes in your sfdx-project.json

Copy

```inline-grid
    "plugins": {
        "sfp": {
          "disableEntitlementFilter": true //disable entitlement filtering
          }
        }
```

[PreviousUpdating Picklist](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/updating-picklist) [NextField History & Feed Tracking](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/field-history-and-feed-tracking)

Last updated 8 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Field History Tracking Guide

Attribute

Type

Description

Package Types Applicable

enableFHT

boolean

Enable field history tracking for fields

* unlocked
* org dependent unlocked

enableFT

boolean

Enable Feed Tracking for fields

* unlocked
* org dependent unlocked

Salesforce has a strict limit on the number of fields that can be tracked. Of course, this limit can be increased by raising it to the Salesforce Account Executive (AE). However, it would be problematic if an unlocked package is deployed to orgs that do not have the limit increased. So don’t be surprised when you are working on a flxbl project and happen to find that the deployment of field history tracking from unlocked packages is disabled by Salesforce.

One workaround is to keep a copy of all the fields that need to be tracked in a separate source package (field-history-tracking or similar) and deploy it as one of the last packages with the ‘alwaysDeploy’ option.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A536%2F1*p43ndpkmL2HzW7qr4pHAuQ.png\&width=768\&dpr=4\&quality=100\&sign=d2534ebd\&sv=2)

Files to be maintained to enable field history tracking deployment before the Jan 23 Release

However, this specific package has to be carefully aligned with the original source/unlocked packages, to which the fields originally belong. As the number of tracked fields increases in large projects, this package becomes larger and more difficult to maintain. In addition, since it’s often the case that the project does not own the metadata definition of fields from managed packages, it doesn’t make much sense to carry the metadata only for field history tracking purposes.

To resolve this, **sfp** features the ability to automate the deployment of field history tracking for both unlocked packaged and managed packages without having to maintain additional packages.

Two mechanisms are implemented to ensure that all the fields that need to be field history tracking enabled are properly deployed. Specifically, a YAML file that contains all the tracked fields is added to the package directory.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A1002%2F1*mMeUFwmSJwiSvIRIz0DfkQ.png\&width=768\&dpr=4\&quality=100\&sign=d96f67cf\&sv=2)

Sample history-tracking.yml

During deployment, the YAML file is examined and the fields to be set are stored in an internal representation inside the deployment artifact. Meanwhile, the components to be deployed are analyzed and the ones with ‘trackHistory’ on are added to the same artifact. This acts as a double assurance that any field that is missed in the YAML files due to human error will also be picked up. After the package installation, all the fields in the artifact are retrieved from the target org and, for those fields, the trackHistory is turned on before they are redeployed to the org.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A692%2F1*ozMGWIZ9wNm7-IdlLfFPsA.png\&width=768\&dpr=4\&quality=100\&sign=f0f39f50\&sv=2)

Files to be maintained to enable field history tracking deployment after the Jan 23 Release.\
(Needs to be kept in 'postDeploy' folder)

In this way, the deployment of field history tracking is completely automated. One now has a declarative way of defining field history tracking (as well as feed tracking) without having to store the metadata in the repo. The only maintenance effort left for developers is to manage the YAML file.

Copy

````inline-grid
// Sample package

{
  "packageDirectories": [\
      {\
      "path": "src/core-crm",\
      "package": "core-crm",\
      "versionNumber": "2.0.10.NEXT",\
      "enableFHT" : true,\
      "enableFT" : true\
    }\
\
```\
\
[PreviousEntitlement Deployment Helper](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/entitlement-deployment-helper) [NextAliasfy Packages](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages)\
\
Last updated 1 month ago\
\
Was this helpful?\
\
This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).\
\
AcceptReject

## Aliasfy Packages Configuration
Attribute

Type

Description

Package Types Applicable

aliasfy

boolean

Enable deployment of contents of a folder that matches the alias of the environment

- source


Aliasified packages are available only for Source Package

Aliasify enables deployment of a subfolder in a source package that matches the target org. For example, you have a source package as listed below.

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FyNt04AGdmG5VOduNo9jy%252Fimage.png%3Falt%3Dmedia%26token%3Dc37aeb61-43f9-4f6f-843b-4a96eb0b72ed&width=768&dpr=4&quality=100&sign=a64a817d&sv=2)

During Installation, only the metadata contents of the folder that matches the alias gets deployed. If the alias is not found, sfp will fallback to the ' **default**' folder. If the default folder is not found, an error will be displayed saying default folder or alias is missing.

Copy

```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]
// Demonstrating package directory with aliasfy
{
  "packageDirectories": [\
      {\
      "path": "src/src-env-specific-alias-post",\
      "package": "src-env-specific-alias-post",\
      "versionNumber": "2.0.10.NEXT",\
      "aliasfy" : true\
    }\
```\
\
**Default** folder are only deployed to sandboxes\
\
[PreviousField History & Feed Tracking](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/field-history-and-feed-tracking) [NextAliasfy Packages - Merge Mode](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/aliasfy-packages-merge-mode)\
\
Last updated 6 months ago\
\
Was this helpful?\
\
This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).\
\
AcceptReject

## Aliasfy Packages Configuration
Attribute

Type

Description

Package Types Applicable

mergeMode

boolean

Enable deployment of contents of a folder that matches the alias of the environment using merge

- source


sfp-pro

sfp (community)

Availability

✅

❌

From

September 24

**mergeMode** adds an additional mode for deploying aliasified packages with **content inheritance**. During package build, the default folder's content is merged with subfolders that matches the org with alias name, along with subfolders able to override inherited content. This reduces metadata duplication while using aliasifed packages.

Copy

```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]
// Demonstrating package directory with aliasfy merge mode
{
  "packageDirectories": [\
      {\
      "path": "src/src-env-specific-alias-post",\
      "package": "src-env-specific-alias-post",\
      "versionNumber": "2.0.10.NEXT",\
      "aliasfy" : {\
          "mergeMode": true\
      }\
    }\
```\
\
![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F3qv0pVRoJV9pqYTYZaBQ%252FScreenshot%25202024-09-17%2520at%252012.58.56.png%3Falt%3Dmedia%26token%3D74cac8ec-ddd9-4ca1-b306-08db4712ce6f&width=768&dpr=4&quality=100&sign=7d5446a1&sv=2)\
\
Package Content (before build)\
\
![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FVLsGA6mfXJKptzV2nNQi%252FScreenshot%25202024-09-17%2520at%252012.57.20.png%3Falt%3Dmedia%26token%3D25c26ccf-47c9-471b-aa12-3b73c2005091&width=768&dpr=4&quality=100&sign=907c2f1d&sv=2)\
\
Artifact Content (after build)\
\
Note in the image above that the `AccountNumberDefault__c` field is replicated in each deployment directory that matches the alias.\
\
The **default** folder is deployed to any type of environment, including production unlike when only used with aliasfy mode\
\
The table below describes the behavior of each command when merge mode is enabled/disabled.\
\
Merge Mode\
\
Default available?\
\
Build\
\
Install\
\
Push\
\
Pull\
\
**Default** metadata is merged into each subfolder\
\
Deploys subfolder that matches enviroment alias name.\
If there is no match, then **default** subfolder is deployed\
(Including production)\
\
Only **default** is pushed\
\
Only **default** is pulled\
\
**Default** merging not available\
\
Deploys subfolder that matches enviroment alias name.\
If there is no match, then **default** subfolder is deployed\
(Including production)\
\
**Default** merging not available\
\
Deploys subfolder that matches enviroment alias name.\
If there is no match, deploys default\
(Only sandboxes)\
\
Only **default** is pushed\
\
Only **default** is pushed\
\
**Default** merging not available\
\
Deploys subfolder that matches enviroment alias name.\
If there is no match, fails\
\
Nothing to push\
\
Nothing to pull\
\
When merge mode is enabled, it also supports push/pull commands, the **default** subfolder is always used during this process, make sure it is not force ignored.\
\
Before merge mode, the whole package was "force ignored." With merge mode, you have the option to allow push/pull from aliasfy packages by not ignoring the **default** subfolder.\
\
Non-merge mode\
\
Merge Mode\
\
Copy\
\
```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]\
# .forceIgnore (aliasfy non-merge mode)\
src-env-specific-alias-post\
```\
\
Copy\
\
```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]\
# .forceIgnore (aliasfy merge mode)\
src-env-specific-alias-post/dev\
src-env-specific-alias-post/test\
src-env-specific-alias-post/prod\
```\
\
[PreviousAliasfy Packages](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages) [NextState management for Flows](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/state-management-for-flows)\
\
Last updated 5 months ago\
\
Was this helpful?\
\
This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).\
\
AcceptReject

## Flow Activation Management
Attribute

Type

Description

Package Types Applicable

enableFlowActivation

boolean

Enable Flows automatically in Production

- source

- diff

- unlocked


While installing a source/diff package, flows by default are deployed as 'Inactive' in the production org. One can deploy flow as 'Active' using the steps mentioned [here](https://help.salesforce.com/s/articleView?id=sf.flow_distribute_deploy_active.htm&language=en_US&type=5), however, this requires the flow to meet test coverage requirement.

Also making a flow inactive, is convoluted, find the detailed article provided by [Gearset](https://gearset.com/blog/deactivate-flows-within-your-data-deployments/)

sfp has automation built in that attempts to align the status of a particular flow version as kept inside the package. It automatically activates the latest version of a Flow in the target org (assuming the package has the latest version). If an earlier version of package is deployed, it skips the activation.

At the same time, if the package contains a Flow with status Obsolete/InvalidDraft/Draft, all the versions of the flow would be rendered inactive.

This feature is enabled by default, if you would like to disable the feature, set enableFlowActivation to false on your package descriptor

Copy

```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]
// Demonstrating package by disabling flow activation
{
  "packageDirectories": [\
      {\
      "path": "src/order-management",\
      "package": "order-management",\
      "versionNumber": "2.0.10.NEXT",\
      "enableFlowActivation" : false\
    }\
```\
\
[PreviousAliasfy Packages - Merge Mode](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/aliasfy-packages/aliasfy-packages-merge-mode) [NextOverview](https://docs.flxbl.io/sfp/installing-an-artifact/overview)\
\
Last updated 8 months ago\
\
Was this helpful?

## Artifact Deployment Overview
Given a directory of artifacts and a target org, the **deploy** command will deploy the artifacts to the target org according to the sequence defined in the project configuration file.

Copy

```inline-grid min-w-full grid-cols-[auto_1fr] p-2 [count-reset:line]
// Command to install set of artifacts to devhub
sfp install -u devhub --artifactdir artifacts
````

#### [Direct link to heading](https://docs.flxbl.io/sfp/installing-an-artifact/overview#sequence-of-activities) Sequence of Activities

The deploy command runs through the following steps

* Reads all the sfp artifacts provided through the artifact directory
* Unzips the artifacts and finds the latest sfdx-project.json.ori to determine the deployment order, if this particular file is not found, it utilizes sfdx-project.json on the repo
* Read the installed packages in the target org utilizing the records in SfpowerscriptsArtifacts2\_\_c
* Install each artifact from the provided artifact directory to the target org based on the deployment order respecting the attributes configured for each package

[PreviousState management for Flows](https://docs.flxbl.io/sfp/building-artifacts/configuring-installation-behaviour-of-a-package/state-management-for-flows) [NextControlling Aspects of Installation](https://docs.flxbl.io/sfp/installing-an-artifact/controlling-aspects-of-installation)

Last updated 6 months ago

Was this helpful?

### Artifact Installation Control

#### [Direct link to heading](https://docs.flxbl.io/sfp/installing-an-artifact/controlling-aspects-of-installation#skip-artifacts-if-they-are-already-installed) Skip artifacts if they are already installed

Copy

```inline-grid
// Command to deploy set of artifacts to devhub
sfp install -u devhub --artifactdir artifacts --skipifalreadyinstalled
```

By using the `skipifalreadyinstalled` option with the deploy command, you can prevent the reinstallation of an artifact that is already present in the target organization.

#### [Direct link to heading](https://docs.flxbl.io/sfp/installing-an-artifact/controlling-aspects-of-installation#install-artifacts-to-an-org-baselined-on-an-another-org) **Install artifacts to an org baselined on an another org**

Copy

```inline-grid
// Command to deploy with baseline
sfp install -u qa \
           --artifactdir artifacts \
           --skipifalreadyinstalled --baselineorg devhub
```

The `--baselineorg` parameter allows you to specify the alias or username of an org against which to check whether the incoming package versions have already been installed and form a deployment plan.This overrides the default behaviour which is to compare against the deployment target org. This is an optional feature which allows to ensure each org's are updated with the same installation across every org's in the path to production.

[PreviousOverview](https://docs.flxbl.io/sfp/installing-an-artifact/overview) [NextApplying attributes of an artifact](https://docs.flxbl.io/sfp/installing-an-artifact/applying-attributes-of-an-artifact)

Last updated 6 months ago

Was this helpful?

### Artifact Installation Attributes

All the attributes that are configured additionally to a package in your project is recorded within the artifact. When the artifact is being installed to the target org, the following steps are undertaken in sequence as seen in the below diagram. Each of the configured attributes below to one of the category in the sequence and executed

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FPEvefLLjm7hvZPa0Ravw%252FInstallation%2520Attributes.png%3Falt%3Dmedia%26token%3D43ef6e13-26af-46dc-8005-9b73fd38b5cd\&width=768\&dpr=4\&quality=100\&sign=29e9c732\&sv=2)

Series of steps undertake while installnig an artifact

[PreviousControlling Aspects of Installation](https://docs.flxbl.io/sfp/installing-an-artifact/controlling-aspects-of-installation) [NextBuiltIn Deployment Helpers](https://docs.flxbl.io/sfp/installing-an-artifact/builtin-deployment-helpers)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Deployment Helpers Overview

[PermissionSet Group Awaiter](https://docs.flxbl.io/sfp/installing-an-artifact/builtin-deployment-helpers/permissionset-group-awaiter)

[PreviousApplying attributes of an artifact](https://docs.flxbl.io/sfp/installing-an-artifact/applying-attributes-of-an-artifact) [NextPermissionSet Group Awaiter](https://docs.flxbl.io/sfp/installing-an-artifact/builtin-deployment-helpers/permissionset-group-awaiter)

Last updated 11 months ago

Was this helpful?

### Permission Set Group Awaiter

[PreviousBuiltIn Deployment Helpers](https://docs.flxbl.io/sfp/installing-an-artifact/builtin-deployment-helpers) [NextPublish Artifact](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact)

Last updated 1 year ago

Was this helpful?

### Publish Artifacts

The Publish command pushes artifacts created by the build command to a npm registry and also provides you functionality to tag a version of the artifact in your git repository.

#### [Direct link to heading](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#publishing-to-an-npm-compatible-private-registry) Publishing to an NPM-Compatible Private Registry

To publish packages to your private registry, you'll need to configure your credentials accordingly. Follow the guidelines provided by your registry's service to ensure a smooth setup.

1. **Locate Documentation**: Jump into your private registry provider's documentation or help center.
2. **Credentials Setup**: Find the section dedicated to setting up publishing credentials or authentication.
3. **Follow Steps**: Adhere to the detailed steps provided by your registry to configure your system for publishing.

When working with sfp and managing artifacts, it’s required to use a private NPM registry. This allows for more control over package distribution, increased security, and custom package management. Here are some popular NPM compatible private registries, including instructions on how to configure NPM to use them:

[**Direct link to heading**](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#github-packages) **GitHub Packages**

GitHub Packages acts as a hosting service for npm packages, allowing for seamless integration with existing GitHub workflows. For configuration details, visit [GitHub's guide on configuring NPM for use with GitHub Packages](https://docs.github.com/en/packages/guides/configuring-npm-for-use-with-github-packages).

[**Direct link to heading**](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#gitlab-npm-registry) **GitLab NPM Registry**

GitLab offers a fully integrated npm registry within its continuous integration/continuous deployment (CI/CD) pipelines. To configure NPM to interact with GitLab’s registry, refer to [GitLab's NPM registry documentation](https://docs.gitlab.com/ee/user/packages/npm_registry/).

[**Direct link to heading**](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#azure-artifacts) **Azure Artifacts**

Azure Artifacts provides a npm feed that's compatible with NPM, enabling package hosting, sharing, and version control. Note: The link provided previously was incorrect. Azure Artifacts information can be found here: [Azure Artifacts documentation](https://docs.microsoft.com/en-us/azure/devops/artifacts/npm/npmrc?view=azure-devops).

[**Direct link to heading**](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#jfrog-artifactory) **JFrog Artifactory**

JFrog Artifactory offers a robust solution for managing npm packages, with features supporting binary repository management. For setting up Artifactory to work with NPM, refer to [JFrog’s npm Registry documentation](https://www.jfrog.com/confluence/display/JFROG/npm+Registry).

[**Direct link to heading**](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#myget) **MyGet**

MyGet provides package management support for npm, among other package managers, facilitating the hosting and management of private npm packages. For specifics on utilizing NPM with MyGet, check out [MyGet’s NPM support documentation](https://docs.myget.org/docs/reference/myget-npm-support).

#### [Direct link to heading](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#utilising-publish-command) Utilising publish command

* Each of these registries offers its own advantages, and the choice between them should be based on your project’s needs and existing infrastructure.
* Follow the instructions on your npm registry to generate . [npmrc](https://docs.npmjs.com/cli/v7/configuring-npm/npmrc) file with the correct URL and access token (which has the permission to publish into your registry.
* Utilize the parameters in sfp publish and provide the npmrc file along with activating npm

Copy

```inline-grid
  --npm                             Upload artifacts to a pre-authenticated private npm registry
  --npmrcpath=<value>               Path to .npmrc file used for authentication to registry. If left blank, defaults to home
                                    directory
  --npmtag=<value>                  Add an optional distribution tag to NPM packages. If not provided, the 'latest' tag is set
                                    to the published version.
  --scope=<value>                   (required for NPM) User or Organisation scope of the NPM package
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact#tagging-an-artifact) Tagging an artifact

sfp's publish command provides you with various options to tag the published artifacts in version control. Please note that these tags are utilized by sfp's build command to determine which packages are to be built when used with `diffcheck` flag

Copy

```inline-grid
  --gittag                          Tag the current commit ID with an annotated tag containing the package name and version -
                                    does not push tag
  --gittagage=<value>               Specifies the number of days,for a tag to be retained,any tags older the provided number
                                    will be deleted
  --gittaglimit=<value>             Specifies the minimum number of  tags to be retained for a package
  --pushgittag                      Pushes the git tags created by this command to the repo, ensure you have access to the repo
```

[PreviousPermissionSet Group Awaiter](https://docs.flxbl.io/sfp/installing-an-artifact/builtin-deployment-helpers/permissionset-group-awaiter) [NextFetching Artifacts](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/fetching-artifacts)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Fetching Artifacts

The fetch command provides you with the ability to fetch artifacts from your artifact registry to your local file system. Artifacts once fetched from your registry can the be used for installation or for further analysis

The fetch command requires a release definition file as the input.

Let's assume you have the following release definition file with the name of the file as `Sprint2-13-11.yaml`

Copy

```inline-grid
release: Sprint-2-13-11-6844956242
skipIfAlreadyInstalled: true
skipArtifactUpdate: false
artifacts:
  feature-management: 1.0.19-6844956242
promotePackagesBeforeDeploymentToOrg: prod
changelog:
  workItemFilters:
    - (FGK|FFK)-[0-9]{3,4}
  workItemUrl: https://flxbl.atlassian.net/browse
  limit: 30
```

One can use the fetch command to fetch all the artifacts by issuing the command as shown below

Copy

```inline-grid
sfp artifacts fetch -p Sprint2-13-11.yaml --npm --scope flxbl
```

Please note the scope flag, the scope flag should match exactly the scope that was provided during publish command

[PreviousPublish Artifact](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/publish-artifact) [NextOverview](https://docs.flxbl.io/sfp/releasing-artifacts/overview)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Artifact Installation Commands

sfp provides two commands to install artifacts into a target org, **sfp install** and **sfp release**. While **sfp install** installs a set of artifacts from a provided directory into a target org, sfp release allows you to install a defined collection of artifacts from an artifact registry directly into a target org, providing you with the consistency required in a pipeline.

sfp release is a combination of predominantly fetch and install commands, and the sequence can be explained with the below image

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F5Nd4kmYwIQMn6EFjerqP%252Fsfp%2520release.png%3Falt%3Dmedia%26token%3D1d1ef1c0-f91a-474e-8212-598512eb2882\&width=768\&dpr=4\&quality=100\&sign=bd076a15\&sv=2)

Sequence of steps undertaken by release command

The release command is invoked by using the command sfp release with the inputs as shown below

Copy

```inline-grid
  sfp release -p path/to/releasedefinition.yml \
              -u myorg --npm --scope myscope \
```

[PreviousFetching Artifacts](https://docs.flxbl.io/sfp/publishing-and-fetching-artifacts/fetching-artifacts) [NextRelease Definitions](https://docs.flxbl.io/sfp/releasing-artifacts/release-definitions)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Release Definitions Overview

A release definition is a YAML file that carries attributes of artifacts and other details. A release definition is the main input for a release command.

Copy

```inline-grid
release: Sprint-2-13-11-6844956242
skipIfAlreadyInstalled: true
artifacts:
  feature-management: 1.0.19-6844956242
  apex-logger: 1.0.20-89
promotePackagesBeforeDeploymentToOrg: prod
changelog:
  workItemFilters:
    - (FGK|FFK)-[0-9]{3,4}
  workItemUrl: https://flxbl.atlassian.net/browse
  limit: 30
```

Lets examine closely the above release defintion file and understand the various attributes and it purposes

Copy

```inline-grid
release: Sprint-2-13-11-6844956242
```

This defines the name of the release. this value will be utilised to generate changelogs or as an identification for other systems to understand what release went into an org

Copy

```inline-grid
skipIfAlreadyInstalled: true
```

This attribute determines whether artifacts should be skipped from installing if the same version number is already installed in the target org

Copy

```inline-grid
artifacts:
  feature-management: 1.0.19-6844956242
  apex-logger: 1.0.20-89
```

This attribute determines which artifacts constitute the provided release.

Please note, when using release definition with exact version numbers. The format of the version number would be : X.Y.Z-BuildNumber, where the version number adopts the following semantics\
X - Major\
Y - Minor\
Z - Patch\
BuildNumber - The build number that produced the package

The version exactly follows the semantics used by the artifact registry (such as Github Packages). However the format is slightly different from the version that is tagged in the git repository where it follows X.Y.Z.BuildNumber

**Example**:\
A package that has the version core\_crm\_v3.0.0.6936, tagged in git should be referenced\
as core\_crm: 3.0.0-6936

Copy

```inline-grid
promotePackagesBeforeDeploymentToOrg: <targetOrgAlias>
```

The above attribute determines whether an artifact of type unlocked package should be promoted before installing into the target org as provided by the alias. If the alias of the requested org and the targeOrgAlias matches, the unlocked packages are promoted before installing into the org

Copy

```inline-grid
changelog:
  workItemFilters:
    - (FGK|FFK)-[0-9]{3,4}
  workItemUrl: https://flxbl.atlassian.net/browse
  limit: 30
```

The reminder of the attributes are about generating change log, which are explained [here](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-changelog)

Here is a run down of all attributes that make up a release definition

Parameter

Required

Type

Description

release

Yes

string

Name of the release

skipIfAlreadyInstalled

No

boolean

Skip installation of artifact if it's already installed in target org

baselineOrg

No

string

The org used to decide whether or not to skip installation of an artifact. Defaults to the target org when not provided.

artifacts

Yes

Object

Map of artifacts to deploy and their corresponding version

promotePackagesBeforeDeploymentToOrg

No

string

Promote packages before they are installed into an org that matches alias of the org

packageDependencies

No

Object

Packages dependencies (e.g. managed packages) to install as part of the release. Provide the 04t subscriber package version Id.

changelog.repoUrl

No

Prop

The URL of the version control system to push changelog files

changelog.workItemFilters

No

Prop

An array of regular expression used to identify work items in your commit messages

changelog.workitemUrl

No

Prop

The generic URL of work items, to which to append work item codes. Allows easy redirection to user stories by clicking on the work-item link in the changelog.

changelog.limit

No

Prop

Limit the number of releases to display in the changelog markdown

changelog.showAllArtifacts

No

Prop

Whether to show artifacts that haven't changed between releases

[PreviousOverview](https://docs.flxbl.io/sfp/releasing-artifacts/overview) [NextGenerating a release definition](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-release-definition)

Last updated 1 year ago

Was this helpful?

### Release Definition Automation

In a high velocity project operating on a trunk such as a #flxbl project and with substantial number of packages, manually generating release definition can be a chore. This can eased by using generating the release definition file automatically after every publish command.

One can utilise [release config](https://docs.flxbl.io/sfp/configuring-a-project/release-config) along with release definition generate command to automate the process of generating release definitions.

Copy

```inline-grid

 sfp releasedefinition:generate -b releasedefns  \
                                -c  main  \
                                -d releasedefns_directory \
                                -f  ${{inputs.releaseconfig}} \
                                -n ${{env.RELEASE_NAME}}

```

The above command will generate a release definition file based on the head of the branch 'main', one can also provide a commit ref ( by providing a commit id to the `-c` or `--gitref` flag ) to utilize the latest artifacts on the particular commit. The generated release definition is then written to a directory called **releasedefns\_directory** and pushed to a branch called **releasedefns**

One can utlilze the `--nopush` flag, if the intent is only to create a releasedefinition locally and do not push the same to the git repository

[PreviousRelease Definitions](https://docs.flxbl.io/sfp/releasing-artifacts/release-definitions) [NextGenerating a changelog](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-changelog)

Last updated 1 year ago

Was this helpful?

### Changelog Generation Guide

sfp's release command will automate generating a change log that will help the team to understand what are the constituent work items/commits that are part of the release and which artifacts where impacted and what was installed

Here is an example of the changelog that is generated by sfp

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FnkRujgByezPuoJlTEgtq%252Fimage.png%3Falt%3Dmedia%26token%3D7c68553e-3feb-4c73-828a-542e4571d638\&width=768\&dpr=4\&quality=100\&sign=715b050c\&sv=2)

A changelog in markdown format rendered in Github

The changelog is available as both as a markdown file and also as an associated json file that has the all the necessary information to render the change log accordingly for instance in a different tool

One can instruct the release command to generate the changelog by utilizing the below flags

Copy

```inline-grid
  -b, --branchname=<value>            Repository branch in which the changelog files are located
  --generatechangelog                 Create a release changelog

```

For eg in the below release command, the release command looks for exisiting `releasechangelog.json` to understand the previous release deployed in the environment and it would append the changelog information to both `releasechangelog.json` and `Release-Changelog.md` file

Copy

```inline-grid
sfp release -u prod \
            -p  releasedefn.yaml \
            --npm --scope flxbl \
            -v prod \
             --generatechangelog --branchname changelog
```

Additional attributes to control changelog links for instance are available in the [release definition](https://docs.flxbl.io/sfp/releasing-artifacts/release-definitions)

#### [Direct link to heading](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-changelog#identifying-work-items) Identifying work items

Change log displays work item by identify attributes in commit message using a simple regex pattern as provided in the workItemFilters section of your release definition.

For instance, given a commit message with the following message , the work item BE-1836 is identified by the work item filter `` BE-[0-9]{2,5}` ``

Copy

```inline-grid
fix/BE-1836: ⚡ added Config changes for Field type changes (#1629)
* fix: ⚡ added Config changes for Field type changes
```

sfp will identify the part BE-1836 and add a URL link to your work item tracking system by using the provided `workItemUrl` attribute

#### [Direct link to heading](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-changelog#generating-changelog-individually) Generating changelog individually

sfp also has standalone commands to generate changelog to create changelog out of bound of the normal release command . One can use the `changelog generate` command for eg:

Copy

```inline-grid
 sfp changelog generate -b releasedefns \
                        -d artifacts \
                        -w "(FGK|FFK)-[0-9]{3,4}" \
                        -r "https://adiza.atlassian.net/browse" \
                        -n Release-1 \
                       --directory changelog
```

[PreviousGenerating a release definition](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-release-definition) [NextOverview](https://docs.flxbl.io/sfp/validating-a-change/overview)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Validate Configuration Changes

**validate** command helps you to validate a change made to your configuration / code against a target org. This command is typically triggered as part of your Pull Request (PR) or Merge process, to ensure the correctness of configuration/code, before being merged into your **main** branch.

validate can either utilise a scratch org from a tagged pool prepared earlier using the preparecommand or one could use the a target org for its purpose

Copy

```inline-grid
// An example where validate is utilized against a pool
// of earlier prepared scratch orgs with label review

sfp validate pool  -p review \
                   -v devhub  \
                   --diffcheck
```

Copy

```inline-grid
// An example where validate is utilized against a target org
sfp validate org -u  ci \
                 -v devhub  \
                 --installdeps  \
```

**validate pool / validate org** command runs the following checks with the options to enable additional features such as dependency and impact analysis:

* Checks accuracy of metadata by deploying the metadata to a org
* Triggers Apex Tests
* Validate Apex Test Coverage of each package (default: 75%)
* Toggle between different modes for validation
* Individual
* Fast Feedback
* Thorough _(Default)_
* Fast Feedback Release Config
* Thorough Release Config
* \[optional] - Validate dependencies between packages for changed component
* \[optional] - Disable diff check while validating, this will validate all the packages in the repository
* \[optional] - Disable parallel testing of apex tests, this will validate apex tests of each package in synchronous mode

[PreviousGenerating a changelog](https://docs.flxbl.io/sfp/releasing-artifacts/generating-a-changelog) [NextDifferent types of validation](https://docs.flxbl.io/sfp/validating-a-change/different-types-of-validation)

Last updated 11 months ago

Was this helpful?

### Validation Techniques Overview

sfp has a comprehensive collection of validation techniques one can adopt according to requirements of your projects. Read on more about various validation modes to achieve your objective

#### [Direct link to heading](https://docs.flxbl.io/sfp/validating-a-change/different-types-of-validation#validate-modes) Validate Modes

Mode

Description

Flag

Individual

Ignore packages installed in scratch org, identify list of changed packages from PR/Merge Request, and validate each of the changed packages (respecting any dependencies) using thorough mode.

`--mode=individual`

Fast Feedback

Skip package dependencies and code coverage and selective test class executions, install only changed components, and ignore changes in package descriptors

`--mode=fastfeedback`

Thorough (Default)

Include package dependencies, code coverage, all test classes during full package deployments

`--mode=thorough`

Fast Feedback Release Config

Extension of fast feedback mode but filtered using release configuration file that defines list of packages to validate with only changed packages ending up being used to validate against the scratch org

`--mode=ff-release-config --releaseconfig=<value>`

Thorough Release Config

Extension of thorough default mode but filtered using release configuration file that defines list of packages to validate with only changed packages ending up being used to validate against the scratch org

`--mode=thorough-release-config --releaseconfig=<value>`

#### [Direct link to heading](https://docs.flxbl.io/sfp/validating-a-change/different-types-of-validation#sequence-of-activities) Sequence of Activities

The following are the list of steps that are orchestrated by the **validate** command

**If used with pools**

* Fetch a scratch org from the provided pools in a sequential manner or
* Authenticate to the Scratch org using the [auth URL](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_auth_sfdxurl.htm) fetched from the [Scratch Org Info Object](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_scratchorginfo.htm)

**If used against a provided org**

* Authenticate to to the provided target org
* Build packages that are changed by comparing the tags in your repo against the packages installed in the target
* For each of the packages (internally calls the Install Command)

**Thorough Mode (Default)**

* Deploy all the built packages as source packages / data packages (unlocked packages are installed as source package)
* Trigger Apex Tests if there are any apex test in the package
* Validate test coverage of the package depending on the type of the package (source packages: each class needs to have 75% or more, unlocked packages: packages as whole need to have 75% or more)

**Fast Feedback Mode**

* Deploy only changed metadata components for built packages as source packages / data packages
* Trigger selective Apex Tests based on impact analysis of the changes in the package
* Skip coverage calculations
* Skip deployment of package if the descriptor is changed
* Skip deployment of top level packages direct dependency on the package containing changed components

**Individual Mode**

* Ignore packages that are installed in the scratch org (basically eliminate the requirement of using a pooled org)
* Compute changed packages by observing the diff of Pull/Merge Request
* Validate each of the changed packages (install any dependencies) using Thorough Mode

**Fast Feedback Release Config Mode**

* Inherit Fast Feedback Mode Features but filtered using a release configuration file containing list of packages to focus validation on
* Deploy only change packages in the list by comparing against what is installed in the org

**Thorough Release Config Mode**

* Inherit Thorough Mode Features but filtered using a release configuration file containing list of packages to focus validation on
* Deploy only change packages in the list by comparing against what is installed in the org

Use of default "thorough" mode is still recommended in the pipeline with fast feedback to ensure coverage computation and scripts added to the descriptor files are correctly working and deployable in an empty org.

By default, all the apex tests are triggered in parallel, with an automated retry, where any tests that fail to execute due to SOQL locks etc are retried synchronously. You can override this behaviour using `--disableparalleltesting` which will trigger tests every time in synchronous mode.

[PreviousOverview](https://docs.flxbl.io/sfp/validating-a-change/overview) [NextLimiting Validation by Domain](https://docs.flxbl.io/sfp/validating-a-change/limiting-validation-by-domain)

Last updated 12 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Domain-Specific Validation

Validation processes often aim to synchronize the provided organization by installing packages that differ from those already installed. This task can become particularly time-consuming for large projects with hundreds of packages.

To streamline the validation process and focus it on specific domains, employing release config based modes is highly recommended. This approach limits the scope of validation, enhancing efficiency and reducing time.

Copy

```inline-grid
 sfp validate org        -u ci
                         -v devhub \
                         --diffcheck \
                         --mode=thorough-release-config \
                         --releaseconfig=<path-to-release-config>
```

In the above example the changes are only validated against the packages as mentioned in the provided release config

[PreviousDifferent types of validation](https://docs.flxbl.io/sfp/validating-a-change/different-types-of-validation) [NextControlling validation attributes of a package](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package)

Last updated 11 months ago

Was this helpful?

### Controlling Validation Attributes

[Skip Testing](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/skip-testing) [Skip Coverage Validation](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/skip-coverage-validation) [Test Synchronously](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/test-synchronously)

[PreviousLimiting Validation by Domain](https://docs.flxbl.io/sfp/validating-a-change/limiting-validation-by-domain) [NextSkip Testing](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/skip-testing)

Was this helpful?

### Skip Testing Attribute

Attribute

Type

Description

Package Types Applicable

skipTesting

boolean

Skip trigger of apex tests while validating or deploying

* unlocked
* org-dependent unlocked
* source
* diff

One can utilize this attribute on a package definition is sfdx-project.json to skipTesting while a change is validated. Use this option with caution, as the package when it gets deployed ( [depending on the package behaviour](https://docs.flxbl.io/sfp/concepts/supported-package-types)) might trigger testing while being deployed to production

Copy

```inline-grid
// Demonstrating how to do use skipTesting
{
  "packageDirectories": [\
    {\
      "path": "core-crm",\
      "package": "core-crm",\
      "versionDescription": "Package containing core schema and classes",\
      "versionNumber": "4.7.0.NEXT",\
      "skipTesting": true\
    },\
     ...\
   ]
}
```

[PreviousControlling validation attributes of a package](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package) [NextSkip Coverage Validation](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/skip-coverage-validation)

Last updated 11 months ago

Was this helpful?

### Skip Coverage Validation

Attribute

Type

Description

Package Types Applicable

**skipCoverageValidation**

boolean

Skip apex test coverage validation of a package

* unlocked
* org-dependent unlocked
* source
* diff

sfp during validation checks the apex test coverage of a package depending on the [package type](https://docs.flxbl.io/sfp/concepts/supported-package-types). This is beneficial so that you dont run into any coverage issues while deploying into higher environments or building an unlocked package.

However, there could be situations where the test coverage calculation is flaky , sfp provides you with an option to turn the coverage validation off.

Copy

```inline-grid
// Demonstrating how to do use skipCoverageValidation
{
  "packageDirectories": [\
    {\
      "path": "core-crm",\
      "package": "core-crm",\
      "versionDescription": "Package containing core schema and classes",\
      "versionNumber": "4.7.0.NEXT",\
      "skipCoverageValidation": true\
    },\
     ...\
   ]
}
```

[PreviousSkip Testing](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/skip-testing) [NextTest Synchronously](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/test-synchronously)

Last updated 11 months ago

Was this helpful?

### Synchronous Test Control

Attribute

Type

Description

Package Types Applicable

**testSynchronous**

boolean

Ensure all tests of the package is triggered synchronously

* unlocked
* org-dependent unlocked
* source
* diff

sfp by default attempts to trigger all the apex tests in a package in asynchronous mode during validation. This is to ensure that you have a highly performant apex test classes which provide you fast feedback.

During installation of packages, test cases are triggered synchronously for source and diff packages. Unlocked and Org-depednendent packages do not trigger any apex test during installation

If you have inherited an existing code base or your unit tests have lot of DML statemements, you will encounter test failuers when a tests in package is triggered asynchronously. You can instruct sfp to trigger the tests of the package in a synchronous mode by setting ' `testSynchnronous` as true

Copy

```inline-grid
// Demonstrating how to do use testSynchronous
{
  "packageDirectories": [\
    {\
      "path": "core-crm",\
      "package": "core-crm",\
      "versionDescription": "Package containing core schema and classes",\
      "versionNumber": "4.7.0.NEXT",\
      "testSynchronous": true\
    },\
     ...\
   ]
}
```

[PreviousSkip Coverage Validation](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/skip-coverage-validation) [NextOverview](https://docs.flxbl.io/sfp/analysing-a-project/overview)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Salesforce Project Analysis

sfp-pro

sfp (community)

Availability

✅

❌

From

January 25

The project analysis command helps you analyze your Salesforce project for potential issues and provides detailed reports in various formats. This command is particularly useful for identifying issues such as duplicate components, hardcoding of environment configuration metadata and so fort the

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#usage) Usage

Copy

```inline-grid
sfp project:analyze [flags]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#common-use-cases) Common Use Cases

The analyze command serves several key purposes:

1. Runs various available linters across the project
2. Generating comprehensive analysis reports
3. Integration with CI/CD pipelines for automated checks

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#available-flags) Available Flags

Flag

Description

Required

Default

`--package, -p`

The name of the package to analyze

No

*

`--domain, -d`

The domain to analyze

No

*

`--source-path, -s`

The path to analyze

No

*

`--exclude-linters`

Comma-separated list of linters to exclude

No

\[]

`--fail-on`

Linters that should cause command failure if issues found

No

\[]

`--show-aliasfy-notes`

Show notes for aliasified packages

No

true

`--fail-on-unclaimed`

Fail when duplicates are found in unclaimed packages

No

false

`--output-format`

Output format (markdown, json, github)

No

markdown

`--report-dir`

Directory for analysis reports

No

*

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#scoping-analysis) Scoping Analysis

The command provides three mutually exclusive ways to scope your analysis:

1. **By Package**: Analyze specific packages

Copy

```inline-grid
sfp project:analyze -p core,utils
```

2. **By Domain**: Analyze all packages in a domain

Copy

```inline-grid
sfp project:analyze -d sales
```

3. **By Source Path**: Analyze a specific directory

Copy

```inline-grid
sfp project:analyze -s ./force-app/main/default
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#output-formats) Output Formats

The command supports multiple output formats:

* **Markdown**: Human-readable documentation format
* **JSON**: Machine-readable format for integration with other tools
* **GitHub**: Special format for GitHub Checks API integration

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#github-integration) GitHub Integration

When running in GitHub Actions, the command automatically:

1. Creates GitHub Check runs for each analysis
2. Adds annotations to the code for identified issues
3. Provides detailed summaries in the GitHub UI

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/overview#examples) Examples

1. Basic analysis of all packages:

Copy

```inline-grid
sfp project:analyze
```

2. Analyze specific packages with JSON output:

Copy

```inline-grid
sfp project:analyze -p core,utils --output-format json
```

3. Analyze with strict validation:

Copy

```inline-grid
sfp project:analyze --fail-on duplicates --fail-on-unclaimed
```

4. Generate reports in a specific directory:

Copy

```inline-grid
sfp project:analyze --report-dir ./analysis-reports
```

[PreviousTest Synchronously](https://docs.flxbl.io/sfp/validating-a-change/controlling-validation-attributes-of-a-package/test-synchronously) [NextDuplicate Check](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check)

Last updated 1 month ago

Was this helpful?

### Duplicate Check Functionality

sfp-pro

sfp (community)

Availability

✅

❌

From

January 25

The duplicate check functionality helps identify duplicate metadata components across your Salesforce project. This analysis is crucial for maintaining clean code organization and preventing conflicts in your deployment process.

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#overview) Overview

Duplicate checking scans your project's metadata components and identifies:

* Components that exist in multiple packages
* Components in aliasified packages
* Components in unclaimed directories (not associated with a package)

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#how-it-works) How It Works

The duplicate checker:

1. Scans all specified directories for metadata components
2. Creates a unique identifier for each component based on type and name
3. Identifies components that appear in multiple locations
4. Provides detailed reporting with context about each duplicate

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#configuration) Configuration

Duplicate checking can be configured through several flags in the project:analyze command:

Copy

```inline-grid
sfp project:analyze --fail-on duplicates --fail-on-unclaimed
```

[**Direct link to heading**](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#key-configuration-options) **Key Configuration Options**

Option

Description

Default

`--show-aliasfy-notes`

Show information about aliasified packages

true

`--fail-on-unclaimed`

Fail when duplicates are in unclaimed packages

false

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#understanding-results) Understanding Results

The duplicate check provides three types of findings:

1. **Direct Duplicates**: Same component in multiple packages
2. **Aliasified Components**: Components in aliasified packages (typically expected)
3. **Unclaimed Components**: Components in directories (such as unpacakged or src/temp) not associated with packages

[**Direct link to heading**](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#sample-output) **Sample Output**

Copy

```inline-grid
# Duplicate Analysis Report

## Aliasified Package Information
- Note: Package "env-specific" is aliasified - duplicates are allowed.

## Component Analysis

### ❌ CustomObject: Account.Custom_Field__c (Duplicate Component)
- `src/package1/objects/Account.object`
- `src/package2/objects/Account.object`

### ⚠️ CustomLabel: Common_Label (Aliasified Component)
- `src/env-specific/qa/labels/Custom.labels` (aliasified)
- `src/env-specific/prod/labels/Custom.labels` (aliasified)
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#understanding-indicators) Understanding Indicators

The analysis uses several indicators to help you understand the results:

* ❌ Indicates a problematic duplicate that needs attention
* ⚠️ Indicates a duplicate that might be intentional (e.g., in aliasified packages)
* _(aliasified)_ Marks components in aliasified packages
* _(unclaimed)_ Marks components in unclaimed directories

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#best-practices) Best Practices

1. **Regular Scanning**: Run duplicate checks regularly during development
2. **Clean Package Structure**: Keep each component in its appropriate package
3. **Proper Package Configuration**: Ensure all directories are properly claimed in sfdx-project.json
4. **Documented Exceptions**: Document cases where duplication is intentional

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#common-scenarios-and-solutions) Common Scenarios and Solutions

[**Direct link to heading**](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#id-1.-legitimate-duplicates) **1. Legitimate Duplicates**

Some components may need to exist in multiple packages. In these cases:

* Use aliasified packages when appropriate
* Document the reason for duplication
* Consider creating a shared package for common components

[**Direct link to heading**](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#id-2.-unclaimed-directories) **2. Unclaimed Directories**

If you find components in unclaimed directories:

1. Add the directory to sfdx-project.json
2. Assign it to an appropriate package
3. Re-run the analysis to verify the fix

[**Direct link to heading**](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#id-3.-aliasified-package-duplicates) **3. Aliasified Package Duplicates**

Duplicates in aliasified packages are often intentional:

* Used for environment-specific configurations
* Different versions of components for different contexts
* Not considered errors by default

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#integration-with-ci-cd) Integration with CI/CD

Integration is limited only to GitHub at the monent, The command needs GITHUB\_APP\_PRIVATE\_KEY and GITHUB\_APP\_ID to be set in an env variable for the results to be reported as a GitHub check

When integrating duplicate checking in your CI/CD pipeline:

1. **Configure Failure Conditions**:

Copy

```inline-grid
sfp project:analyze --fail-on duplicates --fail-on-unclaimed --output-format github
```

2. **Generate Reports**:

Copy

```inline-grid
sfp project:analyze --report-dir ./reports --output-format markdown
```

3. **Review Results**:

* Use generated reports for tracking
* Address critical duplicates
* Document accepted duplicates

#### [Direct link to heading](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check#troubleshooting) Troubleshooting

1. **False Positives**

* Verify package configurations in sfdx-project.json
* Check if components should be in aliasified packages
* Ensure directories are properly claimed

2. **Missing Components**

* Verify scan scope (package, domain, or path)
* Check file permissions
* Validate metadata format

[PreviousOverview](https://docs.flxbl.io/sfp/analysing-a-project/overview) [NextPools](https://docs.flxbl.io/sfp/environment-management/pools)

Last updated 1 month ago

Was this helpful?

### Org Pool Management

[Scratch Org Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools) [Sandbox Pools](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools)

[PreviousDuplicate Check](https://docs.flxbl.io/sfp/analysing-a-project/duplicate-check) [NextScratch Org Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Scratch Org Pools

When developers take on the task of creating their own local environments, the process, although tailored, can introduce significant inefficiencies, particularly in terms of spent time and resources. Dissecting the setup into its fundamental steps reveals why this method can be more tedious than beneficial:

1. **Data Imports**: The process of importing configurations and test data is not only crucial but also time-intensive.
2. **User Configurations**: Assigning the correct permissions requires careful consideration, further slowing down the setup.
3. **Deploy Metadata**: Adjusting and deploying metadata to fit the environment involves dealing with intricacies like endpoints and user integrations.
4. **Install Package Dependencies**: Vital for functionality, the installation of necessary packages consumes additional time.
5. **Org Shape and Scratch Org Definition**: Establishing a scratch org involves creating or utilizing a definition file, a step foundational yet time-consuming.

Scratch Orgs, lauded for their ability to provide a fresh, customizable development environment, also come with a significant drawback—the massive amount of setup time before they're ready for use. Starting with only the standard features, they require a thorough, often lengthy process to incorporate project-specific requirements. While Scratch Orgs offer unmatched flexibility and customization, it's essential to weigh these benefits against the considerable time investment required for their setup.

Scratch org pools represent an indispensable asset for development teams aiming to refine their operational efficiencies and enhance their workflow optimizations. A pool of pre preared scratch orgs facilitate a more rapid and straightforward setup and management of scratch orgs, thereby bolstering team productivity. By adopting scratch org pools, development entities are empowered to access new environments on-demand, circumventing the delays inherent to manual configurations.

[PreviousPools](https://docs.flxbl.io/sfp/environment-management/pools) [NextDefining a pool](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/defining-a-pool)

Last updated 6 months ago

Was this helpful?

### Defining Scratch Org Pools

Pools are defined by creating a pool definition file. The file can be placed in a directory within your repository.

The below configuration details a pool with tag 'DEV-POOL' allocated with 20 scratch orgs. Read on below to understand other configurations that are available

Copy

```inline-grid
{
    "tag": "DEV-POOL",
    "maxAllocation": 20,
    "expiry": 10,
    "batchSize": 10,
    "configFilePath": "config/project-scratch-def.json",
    "relaxAllIPRanges": true,
    "installAll": true,
    "enableSourceTracking": true,
    "retryOnFailure": true,
    "succeedOnDeploymentErrors": true,
    "fetchArtifacts": {
        "npm": {
            "scope": "myproject",
    }
}
```

The latest JSON schema for scratch org pool configuration can be found [here.](https://github.com/dxatscale/sfpowerscripts/blob/main/packages/core/resources/pooldefinition.schema.json) ​

The `pool prepare` command accepts a JSON configuration file that defines settings and attributes of the scratch org pool. The properties accepted by the configuration file are shown in the table below.

Property

Type

Description

tag

string

Name used to identify the scratch org pool

waitTime

int

Minutes the command should wait for a scratch org to be created, Default is 6 minutes

expiry

int

Number of days for which the scratch orgs are active

maxAllocation

int

Maximum capacity of the pool

batchSize

int

Number of processes for creating scratch orgs in parallel

configFilePath

string

Path to scratch org definition JSON file

succeedOnDeploymentErrors

boolean

Whether to persist scratch org to the pool for a deployment error, default:true

snapshotPool

string

Name of the earlier prepared scratch org pool that can be utilized by this pool, to prepare pools in multiple stages.

installAll

boolean

Install all package artifacts, in addition to the managed package dependencies

releaseConfigFile

string

Path to a release config file to create pools with selected packages. Use in conjunction with installAll

enableSourceTracking

boolean

Enable source tracking by deploying packages using source:push and persisting source tracking files

disableSourcePackageOverride

boolean

Disable overriding unlocked packages as source packages, Rather install unlocked packages as unlocked

relaxAllIPRanges

boolean

Relax all IP addresses, allowing all global access to scratch orgs

ipRangesToBeRelaxed

array

Range of IP addresses that can access the scratch orgs

retryOnFailure

boolean

Retry installation of a package on a failed deployment

maxRetryCount

number

Maximum number of times a package should be retried while deploying to a scratchorg, The default is 2

preDependencyInstallationScriptPath

string

Path to a script file that need to be executed before dependent packages are installed in a scratch org

postDeploymentScriptPath

string

Path to a script file that need to be exectued after all the packages (dependencies+repository) is installed

enableVlocity

boolean

Enable vlocity settings and config deployment. Please note it doesnt install vlocity managed package"

fetchArtifacts

object

Fetch artifacts, to be deployed to scratch orgs, from an artifact registry

fetchArtifacts.artifactFetchScript

string

Path to the shell script containing logic for fetching artifacts from a universal registry, if not using npm

fetchArtifacts.npm

object

Fetch artifacts from NPM registry

fetchArtifacts.npm.scope

string

Scope of the NPM package

[PreviousScratch Org Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools) [NextSetting up your Salesforce Org for Scratch Org Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools)

Last updated 11 months ago

Was this helpful?

### Salesforce Scratch Org Setup

### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools#install-sfpowerscripts-scratch-org-pooling-unlocked-package-in-devhub) Install sfpowerscripts Scratch Org Pooling Unlocked Package in DevHub

The Scratch Org Pooling Unlocked Package adds additional custom fields, validation rules, and workflow to the standard object " **ScratchOrgInfo**" in the DevHub to enable associated scratch org pool commands to work for the pipeline.

Copy

```inline-grid
sf package install -p 04t1P000000katQQAQ -o <Your_DevHub_Username> -r -a package -s AdminsOnly -w 30
```

### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools#generate-sfdx-auth-url-for-pipeline-authentication) Generate SFDX auth URL for Pipeline Authentication

In order for pools command to work effectively, ensure that you have authenticated to DevHub using SFDX Auth URL instead of other authentication strategies where you are executing the pool operations

Copy

```inline-grid
sf org display -o <orgAlias> --verbose --json > authFile.json
cat authFile.json
> {
  "status": 0,
  "result": {
    "id": "XXXXYYY",
    "accessToken": "00D8G0000009g7h!uhuRfGKbvPeubTZKztmFWgrykDuuVdxbffzjjVTqjMyRcV{wb+2JtxsevgKfGiGXRz02jY83uNBsD4CuWHwv.b21KZdFxbTi",
    "instanceUrl": "https://your.salesforce.com",
    "username": "vu.ha@dxatscale.io.dxatscale.shareddev",
    "clientId": "PlatformCLI",
    "connectedStatus": "Connected",
    "sfdxAuthUrl": "force://PlatformCLI::Cq$QLeQvDxpvUoNKgiDkoTqyVHdeoMupiZvkgHYcdVHsfMaDpqKJNbg#8ZtUpfBuIdVaUD0B21cFav5X2Pzv5X2@yoursalesforce.com",
    "alias": "SharedDev"
  }
}
```

Save only the following part of the **sfdxAuthUrl** to secret storate and use **sf org login sfdx-url**

`force://PlatformCLI::Cq$QLeQvDxpvUoNKgiDkoTqyVHdeoMupiZvkgHYcdVHsfMaDpqKJNbg#8ZtUpfBuIdVaUD0B21cFav5X2Pzv5X2@yoursalesforce.com`

### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools#public-group-and-sharing-rules-creation-for-developer-access-to-scratch-org-pools) Public Group and Sharing Rules Creation for Developer Access to Scratch Org Pools

For developers (who are on limited access license) to access scratch orgs created by the CI service user, for their local development, a sharing setting needs to be created on the **ScratchOrgInfo** object. The sharing setting should grant read/write access to the ScratchOrgInfo records owned by a public group consisting of the CI service user and a public group consisting of the developer users.

1. Create Public Groups **(Setup > Users > Public Groups)**

* CI Users (Admin users/ CI users who creates scratch orgs in pool)

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FPwwkZeDMOJJ1MgeG1hCT%252Fimage.png%3Falt%3Dmedia%26token%3D13ab665d-5490-42fb-b644-aeb2ed39da7c\&width=768\&dpr=4\&quality=100\&sign=57bcd719\&sv=2)

* Developers (developers who are allowed to fetch scratch orgs from pool)

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FWyhHNdCH2wj5XaGnfesF%252Fimage.png%3Falt%3Dmedia%26token%3D1eb94ec6-531f-4e9b-aaec-87d56defa423\&width=768\&dpr=4\&quality=100\&sign=88e6349c\&sv=2)

2. Create Sharing Rule **"ScratchOrgInfo RW to Developers"** **(Setup > Security > Sharing Settings)**

* Grant Read/Write access to the ScratchOrgInfos records owned by the CI Users to Developers

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FQgQTeJhJsU3jUivsfteT%252Fimage.png%3Falt%3Dmedia%26token%3Dfa3fe35c-e1f3-474a-8885-42dd2ecff97e\&width=768\&dpr=4\&quality=100\&sign=31d9865a\&sv=2)

1. Assign Users to Public Groups **(Setup > Security > Sharing Settings)**

* CI Users
* Developers

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FmjA2VRjPrDjVaMzDm11m%252Fimage.png%3Falt%3Dmedia%26token%3D717e60c0-c0f7-47a7-9486-5fc6228cc08d\&width=768\&dpr=4\&quality=100\&sign=226866b7\&sv=2)

### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools#j.-permission-set-creation-for-developer-access-to-scratchorginfo-object) J. Permission Set Creation for Developer Access to ScratchOrgInfo Object

The developers must also have object-level and FLS permissions on the ScratchOrgInfo object. One way to achieve this is to assign a permission set that has Read, Create, Edit and Delete access on ScratchOrgInfos, as well as Read and Edit access to the custom fields used for scratch org pooling: `Allocation_status__c`, `Password__c`, `Pooltag__c` and `SfdxAuthUrl__c`

**Permission Set Name:**"Scratch Org Developer"

**Object:** Scratch Org Info

1. Object Permissions

* Read, Create, Edit and Delete

2. Field Permissions

* Read, Edit for Custom Fields
* `Allocation_status__c`
* `Password__c`
* `Pooltag__c`
* `SfdxAuthUrl__c`

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252FiPtCt0vyWaM7sCTozA6J%252Fimage.png%3Falt%3Dmedia%26token%3Db13e9ab0-543f-41c8-bcc0-b34a68df84c7\&width=768\&dpr=4\&quality=100\&sign=5eecdfa2\&sv=2)

**System Permissions:**

* API Enabled = True
* API Only User = False
* Create and Update Second-Generation Packages = True

![](https://docs.flxbl.io/~gitbook/image?url=https%3A%2F%2F1646267036-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FYLI5Ts7pWhWQV9UaBn3H%252Fuploads%252F9bKcqGr0PFdLDXDZUv1S%252Fimage.png%3Falt%3Dmedia%26token%3D11d28a9a-3c66-4d5b-a678-9ed5af6ce6bf\&width=768\&dpr=4\&quality=100\&sign=b7b1aea\&sv=2)

### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools#k.-profile-and-permission-set-assignment-for-developers-in-production) **K. Profile and Permission Set Assignment for Developers in Production**

To onboard new developers, the following Profiles and Permission Set will need to be assigned to the new Developer User Account in Salesforce.

**Profile**(Choose 1 Only)

1. Minimum Access - Salesforce

* Licence Type - Salesforce

2. Limited Access User

* Licence Type - Salesforce Limited Access - Free

**Permission Set**

* Scratch Org Developer

**Public Groups**

* Developers - Add Users to "Developers" Group

[PreviousDefining a pool](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/defining-a-pool) [NextPool Operations](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Scratch Org Pool Operations

The following section details on how to operate a pool of scratch orgs

[Preparing pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools) [List Scratch Orgs in a pool](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/list-scratch-orgs-in-a-pool) [Fetch a scratch org](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/fetch-a-scratch-org) [Delete Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/delete-pools)

[PreviousSetting up your Salesforce Org for Scratch Org Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/setting-up-your-salesforce-org-for-scratch-org-pools) [NextPreparing pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools)

Last updated 11 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Scratch Org Pool Preparation

sfp's prepare command helps you to build a pool of pre-built scratch orgs which can include managed packages as well as packages in your repository. This process allows you to cut down time in re-creating a scratch org during validation process when a scratch org is used as just-in-time environment or when being used as a developer environment.

As you integrate more automated processes into Salesforce, incorporating third-party managed packages into your repository's configuration metadata and code becomes necessary. This integration increases the setup time for CI scratch orgs or developer environments for various tasks, such as running data loading scripts or assigning permission sets.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools#steps-undertaken-by-prepare-command) Steps undertaken by prepare command

The prepare command does the following sequence of activities:

1. **Calculate the number of scratch orgs to be allocated** (Based on your requested number of scratch orgs and your org limits, we calculate what is the number of scratch orgs to be allocated at this point in time)
2. **Fetch the artifacts from registry if "fetchArtifacts" is defined in config, otherwise build all artifacts**
3. **Create the scratch orgs in parallel (respecting the timeout requested in the config), and update Allocation\_status\_c of each these orgs to "In Progress"**
4. **On each scratch org, in parallel, do the following activities:**

* Install SFPOWERSCRIPTS\_ARTIFACT\_PACKAGE (04t1P000000ka9mQAA) for keeping track of all the packages which will be installed in the org. You can set an environment variable SFPOWERSCRIPTS\_ARTIFACT\_PACKAGE to override the installation with your own package id (the source code is available [here](https://github.com/dxatscale/sfpowerscripts-artifact))
* Install all the dependencies of your packages, such as managed packages that are marked as dependencies in your sfdx-project.json
* Install all the artifacts that is either built/fetched
* If `enableSourceTracking` is specified in the configuration, the command will create and deploy "sourceTrackingFiles" using sfpowerscripts artifacts to the scratch org. To store local source tracking files, we re-create it when fetching a scratch org from a pool, using the [@salesforce/source-tracking](https://github.com/forcedotcom/source-tracking) library. Checkout the commit from which each sfpowerscripts artifact was created, and update the local source tracking using the package directory. The files are retrieved to the local ".sf" directory, when using `sfpowerscripts:pool:fetch` to fetch a scratch org, and allows users to deploy their changes only, through source tracking. Refer to the [decision log](https://github.com/dxatscale/sfpowerscripts/blob/main/decision%20records/prepare/001-prepare-source-tracking.md) for more details.

5. **Mark each completed scratch org as "Available", depending on the pool config \`succeedOnDeploymentErrors\` is true, else scratch orgs are deleted**

**Ensure that your DevHub is authenticated using** [**SFDX Auth URL**](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_auth_sfdxurl.htm) **and the auth URL is stored in a secure place (Key Management System or Secure Storage).**

[PreviousPool Operations](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations) [NextHandling dependencies](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools/handling-dependencies)

Last updated 1 year ago

Was this helpful?

### Handling Package Dependencies

prepare command has inbuilt capability to orchestrate installation of external package dependencies. Package dependencies are defined in the sfdx-project.json. More information on defining package dependencies can be found in the Salesforce [docs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev2gp_config_file.htm).

Copy

```inline-grid
{
  "packageDirectories": [\
    {\
      "path": "util",\
      "default": true,\
      "package": "Expense-Manager-Util",\
      "versionName": "Winter ‘22",\
      "versionDescription": "Welcome to Winter 2022 Release of Expense Manager Util Package",\
      "versionNumber": "4.7.0.NEXT"\
    },\
    {\
      "path": "exp-core",\
      "default": false,\
      "package": "ExpenseManager",\
      "versionName": "v 3.2",\
      "versionDescription": "Winter 2022 Release",\
      "versionNumber": "3.2.0.NEXT",\
      "dependencies": [\
        {\
          "package": "ExpenseManager-Util",\
          "versionNumber": "4.7.0.LATEST"\
        },\
          {\
          "package": "TriggerFramework",\
          "versionNumber": "1.7.0.LATEST"\
        },\
        {\
          "package": "External Apex Library - 1.0.0.4"\
        }\
      ]\
    }\
  ],
  "sourceApiVersion": "59.0",
  "packageAliases": {
    "TriggerFramework": "0HoB00000004RFpLAM",
    "Expense Manager - Util": "0HoB00000004CFpKAM",
    "External Apex Library@1.0.0.4": "04tB0000000IB1EIAW",
    "Expense Manager": "0HoB00000004CFuKAM"
  }
}
```

Let's unpack the concepts utilizing the above example:

* There are two unlocked packages
* Expense Manager - Util is an unlocked package in your DevHub, identifiable by **0H** in the packageAlias
* Expense Manager - another unlocked package which is dependent on ' Expense Manager - Util', 'TriggerFramework' and 'External Apex Library - 1.0.0.4'
* External Apex Library is an external dependency, It could be a managed package or any unlocked package released on a different Dev Hub. All external package dependencies have to be defined with a **04t** ID, which can be determined from the installation URL from AppExchange or by contacting your vendor.
* sfpowerscripts parses sfdx-project.json and does the following in order
* Skips Expense manager - Util as it doesn't have any dependencies
* For Expense manager
* Checks whether any of the package is part of the same repo, in this example 'Expense Manager-Util' is part of the same repository and will not be installed as a dependency
* Installs the latest version of TriggerFramework ( with major, minor and patch versions matching 1.7.0) to the scratch org
* Install the 'External Apex Library - 1.0.0.4' by utilizing the 04t id provided in the packageAliases

If any of the managed package has keys, it can be provided as an argument to the prepare command. Check the command's flags for more information.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools/handling-dependencies#key-support-for-managed-packages) Key Support for Managed Packages

The format for the 'keys' parameter is a string of key-value pairs separated by spaces - where the key is the name of the package, the value is the protection key of the package, and the key-value pair itself is delimited by a colon .

e.g. `--keys "packageA:12345 packageB:pw356 packageC:pw777"`

The time taken by this command depends on how many managed packages and your packages that need to be installed. Please note, if you are triggering this command in a CI server, ensure proper time outs are provided for this task, as most cloud-based CI providers have time limits on how long a single task can be run. Explore [multi-stage prepare jobs](https://github.com/dxatscale/sfpowerscripts/blob/main/decision%20records/prepare/002-prepare-daisy-chaining.md) in case this is a blocker for the team.

[PreviousPreparing pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools) [NextList Scratch Orgs in a pool](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/list-scratch-orgs-in-a-pool)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Active Scratch Orgs

List all the active scratch orgs with the given pool tag, only 'available' and 'in progress' scratch orgs are displayed

Copy

```inline-grid
sfp pool list -t dev-pool -v devhub

======== Scratch org Details ========
Unused Scratch Orgs in the Pool : 1

Scratch Orgs being provisioned in the Pool : 1

 TAG                   ORG ID          USERNAME                      EXPIRY DATE STATUS                   LOGIN U R L
 ──────── ─────────────── ───────────────────────────── ─────────── ──────────────────────── ────────────────────────────────────────────────
 dev-pool 00D2O000000xxxx test-zey1higmxxxx@example.com 2023-03-03  Provisioning in progress https://efficiency-data-0000.my.salesforce.com/
 dev-pool 00D2N000000xxxx test-ofakal3gxxxx@example.com 2023-03-03  Available                https://inspiration-saas-0000.my.salesforce.com/
```

[PreviousHandling dependencies](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/preparing-pools/handling-dependencies) [NextFetch a scratch org](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/fetch-a-scratch-org)

Last updated 1 year ago

Was this helpful?

### Fetch Scratch Org

A developer can fetch a scratch org with all the dependencies installed and the latest code base has been pushed from a pool for their feature development. the developer can start developing new features without spending a few hours preparing the development environment.

Copy

```inline-grid
sfp pool fetch -t dev-pool -v devhub

Fetching a scratch org from pool dev-pool in Org 00D6F000002Xfaixxx
Enabling source tracking, this will take a bit of time, please hang on
Copying the repository to /var/folders/0n/qs0txqqj6p7gkjlztgmf224h0000gn/T/tmp-11226-pTuia24bl8Gs
Successfully created temporary repository at /var/folders/0n/qs0txqqj6p7gkjlztgmf224h0000gn/T/tmp-11226-pTuia24bl8Gs with commit HEAD
Total Artifacts to Analyze: 1

Installed managed packages:
Packages installed in org:

 Package             Version       Type      isOrgDependent
 FSL                 238.0.35.1    Managed   false
 Conga Composer      8.174.0.15    Managed   false
 Conga Batch         8.22.0.3      Managed   false
 Vlocity Insurance   890.333.0.1   Managed   false

Artifacts installed in org:

 Artifact                      Version         Commit Id
 utils                         1.2.2.1        f79efddfd7f57e52d63222f6d9c23fb1e6a47152


======== Scratch org details ========
 KEY         VALUE
 ─────────── ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 Id          2SR5m0000001d0vxxx
 orgId       00D2O000000Hxxx
 loginURL    https://efficiency-data-0000.my.salesforce.com/
 username    test-zey1higmxxxx@example.com
 password    **********
 expiryDate  2023-03-03
 sfdxAuthUrl force://PlatformCLI::*******@efficiency-data-0000.my.salesforce.com
 status      Assigned
----------------------------------------------------------------------------------------------------
Succesfully fetched a scratch org and enabled source tracking  in 00:00:11.291
----------------------------------------------------------------------------------------------------

```

[PreviousList Scratch Orgs in a pool](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/list-scratch-orgs-in-a-pool) [NextDelete Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/delete-pools)

Last updated 6 months ago

Was this helpful?

### Scratch Org Pool Management

To keep the pools up to date, a nightly scheduled job can be utilised which can delete all the scratch orgs in each pool. In the case of developer pools (pools with source tracking and used by developers as opposed to CI pools), care must be taken to delete only the `unassigned` pools by omitting the `--allscratchorgs` flag.

Be careful when running delete command against developer pools with `-allscratchorg` flag. It will delete the scratch orgs that are used by developer and can result in potential loss of work

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/delete-pools#deleting-orphaned-scratch-orgs) Deleting Orphaned Scratch orgs

sfpowerscripts during prepare attempts to create multiple scratch orgs in parallel, while respecting the timeout as provided in Pool configuration. At times when the Salesforce infrastructure is stretched such as during release windows or during maintenance windows, it could be often seen some of the scratch orgs requested results in being timed out and are discarded by the rest of the pool activities. However, some of these scratch orgs are in fact created by Salesforce without respecting the timeout parameter, and counted against the active scratch org limits.

These scratch orgs titled **orphaned scratch orgs** in sfpowerscripts lingo can be reclaimed by issuing the pool: delete command with --orphans flag. This will delete these scratch orgs and hence allow to recover your active scratch org limits

Copy

```inline-grid
// Example command & output demonstrating delete of orphaned scratch orgs
sfp pool:delete -o -v flxbl
```

[PreviousFetch a scratch org](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/fetch-a-scratch-org) [NextSandbox Pools](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools)

Last updated 1 year ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Sandbox Pools Overview

sfp-pro

sfp (community)

Availability

✅

❌

From

September 24

Sandbox pools in sfp are designed to provide instantly available Salesforce environments for development, testing, and review processes. By maintaining a pool of pre-created sandboxes, teams can significantly reduce wait times and streamline their development workflows.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#key-benefits) Key Benefits

1. **Immediate Availability**: Eliminate waiting times for sandbox creation, allowing developers to start work instantly.
2. **Reduced Overhead**: Minimize the administrative burden of creating and managing individual sandboxes for each task.
3. **Consistent Environment**: Ensure all team members work with standardized, pre-configured sandbox environments.
4. **Seamless Integration**: Easily incorporate sandbox allocation into automated CI/CD pipelines and development workflows.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#sandbox-lifecycle) Sandbox Lifecycle

The following diagram illustrates the lifecycle of a sandbox within a pool:

[iframe](https://integrations.gitbook.com/v1/integrations/mermaid/integration?v=149)

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#commands-overview) Commands Overview

[**Direct link to heading**](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#id-1.-sandbox-pool-initialization) **1. Sandbox Pool Initialization**

Copy

```inline-grid
sfp pool sandbox init -f <path/to/config-file> -v <devhub-alias> -r <owner/repo>
```

Initializes sandbox pools based on configuration files.

[**Direct link to heading**](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#id-2.-fetch-a-sandbox) **2. Fetch a Sandbox**

Copy

```inline-grid
sfp pool sandbox fetch --repository <owner/repo> --pool <pool-name> --branch <branch-name> --issue <issue-number> [--devhubalias <alias>] [--wait <minutes>] [--leasefor <minutes>]
```

Fetches an available sandbox from a pool and assigns it to an issue.

[**Direct link to heading**](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#id-3.-monitor-sandbox-pools) **3. Monitor Sandbox Pools**

Copy

```inline-grid
sfp pool sandbox monitor -v <devhub-alias> -r <owner/repo> -f <path/to/config-file> [<path/to/config-file>...]
```

Monitors sandbox status, handles activations, expirations, and deletions.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools#continuous-monitoring) Continuous Monitoring

The `sfp sandbox monitor` command is designed to be run as a continuous cron job. This ensures that:

1. Newly created sandboxes are activated promptly.
2. Expired sandboxes are identified and marked for deletion.
3. Marked sandboxes are deleted, freeing up resources.
4. The pool is kept in a healthy state, always ready for use.

It's recommended to set up this command to run at regular intervals (e.g., every 15-30 minutes) to maintain an up-to-date and efficient sandbox pool.

For detailed information on each command and the expiration process, please refer to the individual command documentation.

[PreviousDelete Pools](https://docs.flxbl.io/sfp/environment-management/pools/scratch-org-pools/pool-operations/delete-pools) [NextSandbox Pool Initialization](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/sandbox-pool-initialization)

Last updated 2 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Fetch Sandbox from Pool

sfp-pro

sfp (community)

Availability

✅

❌

From

September 24

The `sfp pool sandbox fetch` command is used to fetch an available sandbox from a pool and assign it to a specific issue or pull request.

Please note that for this feature to work, you need to use GitHub/GitLab token and have atleast maintainer access

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/fetch-a-sandbox-from-pool#usage) Usage

Copy

```inline-grid
sfp pool sandbox fetch --repository <owner/repo> --pool <pool-name> --branch <branch-name> --issue <issue-number> [--devhubalias <alias>] [--wait <minutes>] [--leasefor <minutes>]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/fetch-a-sandbox-from-pool#flags) Flags

* `--repository, -r`: The repository path (e.g., `owner/repo`). Default is the `GITHUB_REPOSITORY` environment variable.
* `--pool, -p`: The name of the pool to fetch the sandbox from (required).
* `--branch, -b`: The branch of the pool from where the environment is to be fetched (required).
* `--issue, -i`: Issue number to be associated with the sandbox (required).
* `--devhubalias`: The DevHub alias associated with the pool (default: 'devhub').
* `--wait`: Time in minutes to wait for an available sandbox (default: 20).
* `--leasefor`: Time in minutes to lease the sandbox for the current task (default: 15).

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/fetch-a-sandbox-from-pool#process) Process

1. Checks for existing sandbox assignments to the issue
2. If no assignment, acquires a lock on the pool
3. Fetches an available sandbox
4. Assigns the sandbox to the specified issue
5. Sets GitHub Actions output variable (if in GitHub Actions environment)
6. Releases the lock on the pool

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/fetch-a-sandbox-from-pool#leasing) Leasing

The `--leasefor` parameter specifies how long the sandbox is reserved for the current task before it can be reassigned within the same issue context. This is different from the overall expiration time of the sandbox in the pool.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/fetch-a-sandbox-from-pool#example) Example

Copy

```inline-grid
sfp pool sandbox fetch --repository myorg/myrepo --pool dev-pool --branch feature/new-feature --issue 1234 --leasefor 60
```

This fetches a sandbox from 'dev-pool' for the branch 'feature/new-feature', assigns it to issue 1234, and leases it for 60 minutes.

Please note that for this feature to work, you need to use GitHub/GitLab token and have atleast maintainer access

Copy

```inline-grid
GITHUB
-------------------------
EXPORT GITHUB=1
EXPORT GITHUB_TOKEN=<YOUR_GITHUB_TOKEN> // GITHUB TOKEN NEEDS REPO SCOPE

GITLAB
-------------------------------
EXPORT  GITLAB=1
EXPORT GITLAB_TOKEN=<YOUR_GITLAB_TOKEN> //  GITLAB TOKEN NEEDS API SCOPE
```

[PreviousSandbox Pool Initialization](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/sandbox-pool-initialization) [NextMonitor Sandbox Pools](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/monitor-sandbox-pools)

Last updated 2 months ago

Was this helpful?

### Review Environments Overview

Review environments are a crucial component in the CI/CD workflow of flxbl projects. They provide isolated, ephemeral environments (either scratch orgs or sandboxes) for validating deployments, running tests, and performing acceptance testing during the pull request process.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview#key-benefits) Key Benefits

1. **Isolation**: Each pull request gets its own environment, preventing conflicts between different features or bug fixes.
2. **Reproducibility**: Environments are created from consistent pool configurations, ensuring predictable testing conditions.
3. **Early Validation**: Catch deployment issues early in the development process.
4. **Automated Testing**: Run tests against review environments to verify changes and prevent regressions.
5. **Collaboration**: Provide a shared space for team members to perform acceptance testing and provide feedback.
6. **Compliance**: Enforce governance policies and organizational standards before merging changes.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview#how-it-works) How It Works

1. **Environment Pools**: Review environments are managed in pools, which can be either sandbox or scratch org pools.
2. **Fetching**: When a pull request is opened or updated, a review environment is fetched from the appropriate pool. If an environment is already assigned to the issue, it may be reused if its lease hasn't expired.
3. **Usage**: The environment is populated with the pull request changes and used for automated checks, manual testing, and review processes.
4. **Leasing**: Environments are leased for specific durations to manage resource usage efficiently. While an environment is valid for 24 hours, it can be leased for shorter periods for specific processes.
5. **Status Management**: Throughout its lifecycle, an environment's status can be transitioned between 'InUse', 'Available', and 'Expired' to reflect its current state and availability.
6. **Extension**: If needed, an environment's overall validity can be extended beyond the initial 24-hour period.
7. **Unassignment**: When no longer needed, environments are unassigned from issues and either returned to the pool or marked for deletion.

This lifecycle is managed through a set of `sfp reviewenv` commands, which automate the process of fetching, checking, transitioning, extending, and unassigning review environments.

[PreviousMonitor Sandbox Pools](https://docs.flxbl.io/sfp/environment-management/pools/sandbox-pools/monitor-sandbox-pools) [NextCommands](https://docs.flxbl.io/sfp/environment-management/overview/commands)

Last updated 6 months ago

Was this helpful?

### Review Environment Management

[Fetch a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment) [Check Review Environment Status](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status) [Extend a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment) [Transition Review Environment Status](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status) [Unassign a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment)

[PreviousReview Environments](https://docs.flxbl.io/sfp/environment-management/overview) [NextFetch a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment)

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Fetch Review Environment

The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment#usage) Usage

Copy

```inline-grid
sfp reviewenv fetch --repository <owner/repo> --pool <pool> --poolType <type> --branch <branch> --issue <issue> [--devhubAlias <alias>] [--wait <minutes>] [--leaseFor <minutes>]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment#options) Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--pool`: The name of the pool to fetch the review environment from (required).
* `--poolType`: The type of the pool, either `sandbox` or `scratchorg` (required).
* `--branch`: The pull request branch to fetch the environment for (required).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).
* `--devhubAlias`: The DevHub alias associated with the pool (default: `devhub`).
* `--wait`: Time in minutes to wait for an environment to become available (default: 20).
* `--leaseFor`: Time in minutes typically this environment is leased for during similar operations

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment#behavior) Behavior

1. Checks if an environment is already assigned to the issue.
2. If assigned, verifies if the previous lease has expired based on the `leaseFor` duration. If a job has not released the environment within the earlier mentioned leaseFor, the new job will be provided the environment
3. If no environment is assigned or the environment assigned to the issue has expired, fetches a new environment from the pool.
4. Automatically marks the fetched or reassigned environment as "InUse".
5. Waits for up to the specified `wait` time if no environment is immediately available.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment#notes) Notes

* The environment remains valid for 24 hours from assignment, regardless of the `leaseFor` duration.
* The `leaseFor` parameter determines how long the current process can use the environment before it becomes available for reassignment for a different job within the same issue.

[PreviousCommands](https://docs.flxbl.io/sfp/environment-management/overview/commands) [NextCheck Review Environment Status](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status)

Last updated 6 months ago

Was this helpful?

### Review Environment Status

The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status#usage) Usage

Copy

```inline-grid
sfp reviewenv check --repository <owner/repo> --issue <issue> [--pool <pool>] [--poolType <type>] [--branch <branch>]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status#options) Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).
* `--pool`: The name of the pool to filter by (optional).
* `--poolType`: The type of the pool to filter by, either `sandbox` or `scratchorg` (optional).
* `--branch`: The pull request branch to filter by (optional).

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status#behavior) Behavior

1. Searches the repository's stored environment data for environments associated with the specified issue.
2. Returns details of any found environments, including:

* Environment name or username
* Environment type (sandbox or scratch org)
* Pool the environment belongs to
* Associated pull request branch
* Environment status and expiration date

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status#notes) Notes

* This command is typically used as needed, not within the regular pull request workflow.
* It's useful for verifying environment details or troubleshooting.

[PreviousFetch a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/fetch-a-review-environment) [NextExtend a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment)

Last updated 6 months ago

Was this helpful?

### Extend Review Environment

The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment#usage) Usage

Copy

```inline-grid
sfp reviewenv extend --repository <owner/repo> --issue <issue>
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment#options) Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment#behavior) Behavior

1. Locates the environment assigned to the specified issue.
2. Extends the overall validity of the environment by an additional 24 hours from the current time.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment#notes) Notes

* This command is useful when more time is needed for thorough testing or when waiting for stakeholder approval.
* It extends the overall validity of the environment, not the lease time for a specific process.
* Use judiciously to avoid unnecessarily tying up resources.

[PreviousCheck Review Environment Status](https://docs.flxbl.io/sfp/environment-management/overview/commands/check-review-environment-status) [NextTransition Review Environment Status](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Review Environment Status

The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status#usage) Usage

Copy

```inline-grid
sfp reviewenv transition --repository <owner/repo> --issue <issue> --status <status>
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status#options) Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).
* `--status`: The status to transition the review environment to (required). Options: 'InUse', 'Available', 'Expired'

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status#behavior) Behavior

1. Locates the environment assigned to the specified issue.
2. Updates the status of the environment to the specified status.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status#status-meanings) Status Meanings

* 'InUse': The environment is currently being used by automated checks or another automated process.
* 'Available': The environment is available for reuse by another automation within the same issue's context.
* 'Expired': The environment will be picked up by Pool commands for deletion.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status#notes) Notes

* This command doesn't reflect the state of the pull request, but rather the current usage state of the environment within the issue's context.
* Transitioning to 'Available' before the lease expires allows for efficient reuse within the same issue.

[PreviousExtend a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/extend-a-review-environment) [NextUnassign a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment)

Last updated 6 months ago

Was this helpful?

### Unassign Review Environment

The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment#usage) Usage

Copy

```inline-grid
sfp reviewenv unassign --repository <owner/repo> --issue <issue> [--returntopool]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment#options) Options

* `--repository`: The repository path that stores the pool lock (default: current repo).
* `--issue`: The pull request number to assign the environment to, or a unique id that will be used subsequently to identify (required).
* `--returntopool`: If set to true, the environment will be returned to the pool for reuse. If false or not set, it will be marked as expired.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment#behavior) Behavior

1. Locates the environment assigned to the specified issue.
2. Removes the assignment of the environment from the issue.
3. Based on the `--returntopool` flag:

* If true: Marks the environment as available for reuse within the pool.
* If false or not set: Marks the environment as expired, to be picked up by Pool commands for deletion.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment#notes) Notes

* This command should be used when a review environment is no longer needed for an issue.
* Consider carefully whether to return the environment to the pool or mark it as expired based on your project's needs and the state of the environment.

[PreviousTransition Review Environment Status](https://docs.flxbl.io/sfp/environment-management/overview/commands/transition-review-environment-status) [NextConsiderations](https://docs.flxbl.io/sfp/environment-management/overview/considerations)

Last updated 6 months ago

Was this helpful?

### Review Environment Management

When working with review environments in flxbl projects, consider the following to optimize your workflow and resource usage:

1. **Lease Management**:

* Use the `--leaseFor` parameter in `sfp reviewenv fetch` to set appropriate lease durations for your processes.
* Balance between long enough leases to complete processes and short enough to allow efficient reuse.

2. **Wait Times**:

* Utilize the `--wait` parameter in `sfp reviewenv fetch` to allow for environment availability in busy systems.
* Set wait times based on your project's typical environment turnover time.

3. **Status Transitions**:

* If a process completes before its lease expires, transition the environment to 'Available' status to allow earlier reuse within the same issue.
* Keep environment statuses up-to-date to reflect their current use accurately.

4. **Automation**:

* Integrate `sfp reviewenv` commands into your CI/CD pipelines for automatic environment management.
* Include proper lease handling and status transitions in your automated workflows.

5. **Efficient Reuse**:

* Understand the interplay between the 24-hour validity period and the `leaseFor` duration.
* Design your workflows to maximize environment reuse efficiency within issues.

6. **Timely Unassignment**:

* Unassign environments promptly when they're no longer needed to free up resources.
* Consider whether to return to the pool or mark as expired based on the environment's state and your project's needs.

7. **Pool Management**:

* Regularly review and manage your environment pools.
* Ensure a healthy balance of available environments, considering both the number of environments and typical lease durations.

8. **Extension Judiciousness**:

* Use `sfp reviewenv extend` only when necessary to avoid tying up resources unnecessarily.
* Consider implementing approval processes for extensions in long-running projects.

9. **Monitoring and Logging**:

* Implement monitoring for environment usage, lease times, and pool availability.
* Maintain logs of environment assignments and transitions for troubleshooting and optimization.

10. **User Education**:

* Ensure team members understand the review environment lifecycle and the impact of their actions on resource availability.
* Provide guidelines for efficient use of review environments in your development processes.

By considering these points, you can streamline your development process, improve resource utilization, and ensure efficient use of review environments in your flxbl projects.

[PreviousUnassign a Review Environment](https://docs.flxbl.io/sfp/environment-management/overview/commands/unassign-a-review-environment) [NextSandbox](https://docs.flxbl.io/sfp/environment-management/sandbox)

Last updated 6 months ago

Was this helpful?

### Sandbox Management

[Create Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox) [Delete Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/delete-sandbox) [List Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox) [Login to Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/login-to-sandbox) [Update Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/update-sandbox)

[PreviousConsiderations](https://docs.flxbl.io/sfp/environment-management/overview/considerations) [NextCreate Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox)

Was this helpful?

### Create Sandbox Org

sfp-pro

sfp (community)

Availability

✅

❌

From

August 24

This command creates a new sandbox org from a source org (production or another sandbox). You can specify the sandbox configuration using command-line options or a definition file.

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox#usage) Usage

Copy

```inline-grid
$ sfp sandbox:create -v <string> -n <string> -s <string> [flags]
$ sfp org:create:sandbox -v <string> -n <string> -s <string> [flags]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox#flags) Flags

Flag

Required

Description

`-v, --targetdevhubusername=<value>`

No

Username or alias of the production org

`-n, --name=<value>`

Yes (if no definition file)

Name of the sandbox to create

`-s, --sourcesandbox=<value>`

No

Name of the source sandbox to clone

`-f, --definition-file=<value>`

No

Path to a sandbox definition file

`-d, --description=<value>`

No

Description of the sandbox

`--json`

No

Format output as JSON

`--loglevel=<option>`

No

Logging level for this command invocation

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox#examples) Examples

Copy

```inline-grid
$ sfp sandbox:create -v MyDevHub -n MySandbox1 -s SourceSandbox
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox#sandbox-definition-file) Sandbox Definition File

If using a definition file, create a JSON file (default location: `config/sandbox-def.json`) with the following structure:

Copy

```inline-grid
{
  "sandboxName": "MySandbox",
  "description": "My sandbox description",
  "sourceSandbox": "SourceSandboxName",
  "apexClassId": "01p...",
  "autoActivate": true,
  "copyChatter": true,
  "copyArchivedActivities": true,
  "licenseType": "Developer",
  "templateId": "0TT...",
  "historyDays": 0
}
```

Note: When using a definition file, the `--name` and `--sourcesandbox` flags are ignored.

[PreviousSandbox](https://docs.flxbl.io/sfp/environment-management/sandbox) [NextDelete Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/delete-sandbox)

Last updated 6 months ago

Was this helpful?

### Delete Sandbox Commands

sfp-pro

sfp (community)

Availability

✅

❌

From

August 24

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/delete-sandbox#usage) Usage

Copy

```inline-grid
$ sfp sandbox:delete -n <string> [flags]
$ sfp org:delete:sandbox -n <string> [flags]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/delete-sandbox#flags) Flags

Flag

Required

Description

`-v, --targetdevhubusername=<value>`

No

Username or alias of the production org

`-n, --name=<value>`

Yes

Comma-separated list of sandbox names to delete

`--json`

No

Format output as JSON

`--loglevel=<option>`

No

Logging level for this command invocation

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/delete-sandbox#examples) Examples

Copy

```inline-grid
$ sfp sandbox:delete -n SIT -v my-sandbox-org
$ sfp sandbox:delete -n SIT,UAT,DEV -v my-sandbox-org
```

[PreviousCreate Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/create-sandbox) [NextList Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox)

Last updated 6 months ago

Was this helpful?

### Sandbox Management Commands

sfp-pro

sfp (community)

Availability

✅

❌

From

September 24

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox#usage) Usage

Copy

```inline-grid
$ sfp sandbox:list [flags]
$ sfp sandbox:status [flags]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox#flags) Flags

Flag

Required

Description

`-v, --targetdevhubusername=<value>`

No

Username or alias of the production org

`-n, --name=<value>`

No

Filter results by sandbox name (max 10 characters)

`-s, --status=<value>`

No

Filter results by sandbox status

`--json`

No

Format output as JSON

`--loglevel=<option>`

No

Logging level for this command invocation

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox#status-options) Status Options

Available status options: 'Pending', 'Processing', 'Completed', 'Deleted', 'Deleting', 'Discarding', 'Locked', 'Locking', 'Sampling', 'Stopped', 'Suspended', 'Activating'

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox#examples) Examples

Copy

```inline-grid
$ sfp sandbox:list --name mySandbox1 -v myDevHub
$ sfp sandbox:status --status Completed -v myDevHub
```

[PreviousDelete Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/delete-sandbox) [NextLogin to Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/login-to-sandbox)

Last updated 6 months ago

Was this helpful?

This site uses cookies to deliver its service and to analyse traffic. By browsing this site, you accept the [privacy policy](https://www.termsfeed.com/live/58242e96-b694-4c5b-be80-8ef0f341edd7).

AcceptReject

### Sandbox Login Guide

sfp-pro

sfp (community)

Availability

✅

❌

From

August 24

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/login-to-sandbox#usage) Usage

Copy

```inline-grid
$ sfp sandbox:login -n <string> [flags]
$ sfp org:login:sandbox -n <string> [flags]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/login-to-sandbox#flags) Flags

Flag

Required

Description

`-v, --targetdevhubusername=<value>`

No

Username or alias of the production org

`-n, --name=<value>`

Yes

Name of the sandbox to log in to (max 10 characters)

`-a, --alias=<value>`

No

Alias to set for the authenticated sandbox org

`-w, --write-file`

No

Write login details to a file (org-info.json)

`--json`

No

Format output as JSON

`--loglevel=<option>`

No

Logging level for this command invocation

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/login-to-sandbox#examples) Examples

Copy

```inline-grid
$ sfp sandbox:login --name mySandbox1 -v myDevHub
$ sfp sandbox:login --name mySandbox1 -v myDevHub --alias mySandboxAlias --write-file
```

[PreviousList Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/list-sandbox) [NextUpdate Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/update-sandbox)

Last updated 6 months ago

Was this helpful?

### Update Sandbox Instructions

sfp-pro

sfp (community)

Availability

✅

❌

From

August 24

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/update-sandbox#usage) Usage

Copy

```inline-grid
$ sfp sandbox:update [flags]
$ sfp org:update:sandbox [flags]
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/update-sandbox#flags) Flags

Flag

Required

Description

`-v, --targetdevhubusername=<value>`

No

Username or alias of the production org

`-n, --name=<value>`

Yes (if no definition file)

Name of the sandbox to update

`-f, --definition-file=<value>`

No

Path to a sandbox definition file

`--json`

No

Format output as JSON

`--loglevel=<option>`

No

Logging level for this command invocation

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/update-sandbox#examples) Examples

Copy

```inline-grid
$ sfp sandbox:update -o MyDevHub -n my-sandbox
$ sfp sandbox:update -o MyDevHub -f config/sandbox-def.json
```

#### [Direct link to heading](https://docs.flxbl.io/sfp/environment-management/sandbox/update-sandbox#sandbox-definition-file) Sandbox Definition File

If using a definition file, create a JSON file (default location: `config/sandbox-def.json`) with the following structure:

Copy

```inline-grid
{
  "sandboxName": "MySandbox",
  "description": "Updated sandbox description",
  "apexClassId": "01p...",
  "autoActivate": true,
  "copyChatter": true,
  "historyDays": 0,
  "licenseType": "DEVELOPER",
  "templateId": "0TT..."
}
```

Note: When using a definition file, the `--name` flag is ignored.

[PreviousLogin to Sandbox](https://docs.flxbl.io/sfp/environment-management/sandbox/login-to-sandbox) [NextDevelopment Environment](https://docs.flxbl.io/sfp/development/development-environment)

Last updated 6 months ago

Was this helpful?

# Publish Artifact

The Publish command pushes artifacts created by the build command to a npm registry and  also provides you functionality to tag a version of the artifact in your  git repository. &#x20;

### Publishing to an NPM-Compatible Private Registry

To publish packages to your private registry, you'll need to configure your credentials accordingly. Follow the guidelines provided by your registry's service to ensure a smooth setup.

1. **Locate Documentation**: Jump into your private registry provider's documentation or help center.
2. **Credentials Setup**: Find the section dedicated to setting up publishing credentials or authentication.
3. **Follow Steps**: Adhere to the detailed steps provided by your registry to configure your system for publishing.

When working with sfp and managing artifacts, it’s required  to use a private NPM registry. This allows for more control over package distribution, increased security, and custom package management. Here are some popular NPM compatible private registries, including instructions on how to configure NPM to use them:

#### GitHub Packages

GitHub Packages acts as a hosting service for npm packages, allowing for seamless integration with existing GitHub workflows. For configuration details, visit [GitHub's guide on configuring NPM for use with GitHub Packages](https://docs.github.com/en/packages/guides/configuring-npm-for-use-with-github-packages).

#### GitLab NPM Registry

GitLab offers a fully integrated npm registry within its continuous integration/continuous deployment (CI/CD) pipelines. To configure NPM to interact with GitLab’s registry, refer to [GitLab's NPM registry documentation](https://docs.gitlab.com/ee/user/packages/npm\_registry/).

#### Azure Artifacts

Azure Artifacts provides a npm feed that's compatible with NPM, enabling package hosting, sharing, and version control. Note: The link provided previously was incorrect. Azure Artifacts information can be found here: [Azure Artifacts documentation](https://docs.microsoft.com/en-us/azure/devops/artifacts/npm/npmrc?view=azure-devops).

#### JFrog Artifactory

JFrog Artifactory offers a robust solution for managing npm packages, with features supporting binary repository management. For setting up Artifactory to work with NPM, refer to [JFrog’s npm Registry documentation](https://www.jfrog.com/confluence/display/JFROG/npm+Registry).

#### MyGet

MyGet provides package management support for npm, among other package managers, facilitating the hosting and management of private npm packages. For specifics on utilizing NPM with MyGet, check out [MyGet’s NPM support documentation](https://docs.myget.org/docs/reference/myget-npm-support).

### Utilising publish command

* Each of these registries offers its own advantages, and the choice between them should be based on your project’s needs and existing infrastructure.
* Follow the instructions on your npm registry to generate .[npmrc](https://docs.npmjs.com/cli/v7/configuring-npm/npmrc) file with the correct URL and access token (which has the permission to publish into your registry.
* Utilize the parameters in sfp publish and provide the npmrc file along with activating npm

```
  --npm                             Upload artifacts to a pre-authenticated private npm registry
  --npmrcpath=<value>               Path to .npmrc file used for authentication to registry. If left blank, defaults to home
                                    directory
  --npmtag=<value>                  Add an optional distribution tag to NPM packages. If not provided, the 'latest' tag is set
                                    to the published version.
  --scope=<value>                   (required for NPM) User or Organisation scope of the NPM package
```

### Tagging an artifact

sfp's publish command provides you with various options to tag the published artifacts in version control. Please note that these tags are utilized by sfp's build command to determine which packages are to be built when used with `diffcheck`flag

<pre><code><strong>  --gittag                          Tag the current commit ID with an annotated tag containing the package name and version -
</strong>                                    does not push tag
  --gittagage=&#x3C;value>               Specifies the number of days,for a tag to be retained,any tags older the provided number
                                    will be deleted
  --gittaglimit=&#x3C;value>             Specifies the minimum number of  tags to be retained for a package
  --pushgittag                      Pushes the git tags created by this command to the repo, ensure you have access to the repo
</code></pre>

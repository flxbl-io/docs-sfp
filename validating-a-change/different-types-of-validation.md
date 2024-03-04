# Different types of validation

sfp has a comprehensive collection of validation techniques one can adopt according to requirements of your projects.   Read on more about various validation modes to achieve your objective

### Validate Modes

<table><thead><tr><th>Mode</th><th>Description</th><th width="220">Flag</th></tr></thead><tbody><tr><td>Individual</td><td>Ignore packages installed in scratch org, identify list of changed packages from PR/Merge Request, and validate each of the changed packages (respecting any dependencies) using thorough mode.</td><td><code>--mode=individual</code></td></tr><tr><td>Fast Feedback</td><td>Skip package dependencies and code coverage and selective test class executions, install only changed components, and ignore changes in package descriptors</td><td><code>--mode=fastfeedback</code></td></tr><tr><td>Thorough (Default)</td><td>Include package dependencies, code coverage, all test classes during full package deployments</td><td><code>--mode=thorough</code></td></tr><tr><td>Fast Feedback Release Config</td><td>Extension of fast feedback mode but filtered using release configuration file that defines list of packages to validate with only changed packages ending up being used to validate against the scratch org</td><td><code>--mode=ff-release-config</code><br><br><code>--releaseconfig=&#x3C;value></code></td></tr><tr><td>Thorough Release Config</td><td>Extension of thorough default mode but filtered using release configuration file that defines list of packages to validate with only changed packages ending up being used to validate against the scratch org</td><td><code>--mode=thorough-release-config</code><br><br><code>--releaseconfig=&#x3C;value></code></td></tr></tbody></table>

### Sequence of Activities

The following are the list of steps that are orchestrated by the **validate** command\
\
**If used with pools**

* Fetch a scratch org  from the provided pools in a sequential manner or&#x20;
* Authenticate to the Scratch org using the [auth URL](https://developer.salesforce.com/docs/atlas.en-us.sfdx\_cli\_reference.meta/sfdx\_cli\_reference/cli\_reference\_auth\_sfdxurl.htm) fetched from the [Scratch Org Info Object](https://developer.salesforce.com/docs/atlas.en-us.object\_reference.meta/object\_reference/sforce\_api\_objects\_scratchorginfo.htm)

**If used against a provided org**

* Authenticate to to the provided target org\

* Build packages that are changed by comparing the tags in your repo against the packages installed in the target
*   For each of the packages (internally calls the Install Command)\
    \
    **Thorough Mode (Default)**\


    * Deploy all the built packages as [source packages](broken-reference) / [data packages](broken-reference) (unlocked packages are installed as source package)
    * Trigger Apex Tests if there are any apex test in the package
    * Validate test coverage of the package depending on the type of the package (source packages: each class needs to have 75% or more, unlocked packages: packages as whole need to have 75% or more)

    \
    **Fast Feedback Mode**\


    * Deploy only changed metadata components for built packages as [source packages](broken-reference) / [data packages](broken-reference)
    * Trigger selective Apex Tests based on impact analysis of the changes in the package
    * Skip coverage calculations
    * Skip deployment of package if the descriptor is changed
    * Skip deployment of top level packages direct dependency on the package containing changed components



    **Individual Mode**\


    * Ignore packages that are installed in the scratch org (basically eliminate the requirement of using a pooled org)
    * Compute changed packages by observing the diff of Pull/Merge Request
    * Validate each of the changed packages (install any dependencies) using Thorough Mode

    \
    **Fast Feedback Release Config Mode**\


    * Inherit Fast Feedback Mode Features but filtered using a release configuration file containing list of packages to focus validation on
    * Deploy only change packages in the list by comparing against what is installed in the org



    **Thorough Release Config Mode**\


    * Inherit Thorough Mode Features but filtered using a release configuration file containing list of packages to focus validation on
    * Deploy only change packages in the list by comparing against what is installed in the org

{% hint style="info" %}
Use of default "thorough" mode is still recommended in the pipeline with fast feedback to ensure coverage computation and scripts added to the descriptor files are correctly working and deployable in an empty org.
{% endhint %}

{% hint style="info" %}
By default, all the apex tests are triggered in parallel, with an automated retry, where any tests that fail to execute due to SOQL locks etc are retried synchronously. You can override this behaviour using `--disableparalleltesting` which will trigger tests every time in synchronous mode.
{% endhint %}

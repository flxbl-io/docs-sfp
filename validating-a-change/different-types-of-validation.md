# Different types of validation

sfp provides validation techniques to verify changes in your Salesforce projects before merging. The validate command supports two primary modes to suit different validation needs.

### Validation Modes

<table><thead><tr><th>Mode</th><th>Description</th><th width="220">Flag</th></tr></thead><tbody><tr><td>Thorough (Default)</td><td>Include package dependencies, code coverage, all test classes during full package deployments. This is the recommended mode for comprehensive validation.</td><td><code>--mode=thorough</code></td></tr><tr><td>Individual</td><td>Ignore packages installed in scratch org, identify list of changed packages from PR/Merge Request, and validate each of the changed packages (respecting any dependencies) using thorough validation rules.</td><td><code>--mode=individual</code></td></tr></tbody></table>

### Release Config Filtering

Both validation modes support filtering packages using a release configuration file through the `--releaseconfig` flag. When provided, only packages defined in the release config that have changes will be validated. This is useful for:
- Large monorepos with multiple domains
- Focusing validation on specific package groups
- Reducing validation time by limiting scope

```bash
# Validate with release config filtering
sfp validate org --targetorg myorg --mode thorough --releaseconfig config/release.yml
```

### Evolution of Validation Modes

#### Why Fast Feedback Was Deprecated

Fast Feedback mode was initially introduced to provide quicker validation by:
- Installing only changed components instead of full packages
- Running selective tests based on impact analysis
- Skipping coverage calculations
- Skipping packages with only descriptor changes

However, this mode was **deprecated and removed** because:

1. **Complexity vs. Value**: The mode introduced significant complexity in determining what to test versus what to synchronize, while the time savings were inconsistent.

2. **Improved Alternative Approach**: The validation logic was enhanced to automatically differentiate between:
   - **Packages to synchronize**: Already validated packages from upstream changes (deployed but not tested)
   - **Packages to validate**: Packages with changes introduced by the current PR (deployed and tested)
   
   This automatic differentiation provides the speed benefits of fast feedback without requiring a separate mode.

3. **Better Options Available**: 
   - Use `--ref` and `--baseRef` flags to specify exact comparison points
   - Use `--releaseconfig` to limit validation scope
   - Use `--skipTesting` when tests aren't needed
   - Use `individual` mode for isolated package validation

#### Currently Deprecated Modes

- **Fast Feedback** (`--mode=fastfeedback`) - Removed in favor of automatic synchronization logic
- **Fast Feedback Release Config** (`--mode=ff-release-config`) - Use `--releaseconfig` with standard modes instead
- **Thorough Release Config** (`--mode=thorough-release-config`) - Use `--mode=thorough --releaseconfig` instead

> **Note**: The current validation intelligently handles synchronized vs. validated packages automatically when you provide `--ref` and `--baseRef` flags, achieving faster feedback without a separate mode.

### Sequence of Activities

The following steps are orchestrated by the **validate** command:

#### Initial Setup

**When using pools:**
* Fetch a scratch org from the provided pools in a sequential manner
* Authenticate to the scratch org using the [auth URL](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_auth_sfdxurl.htm) fetched from the [Scratch Org Info Object](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_scratchorginfo.htm)

**When using a provided org:**
* Authenticate to the provided target org

#### Package Processing

1. **Identify packages to validate:**
   - Build packages that are changed by comparing the tags in your repo against the packages installed in the target
   - If `--releaseconfig` is provided, filter packages based on the release configuration

2. **For each package to validate:**

   **Thorough Mode (Default):**
   * Deploy all the built packages as source packages / data packages (unlocked packages are installed as source packages)
   * Trigger Apex Tests if there are any apex tests in the package
   * Validate test coverage of the package depending on the type:
     - Source packages: Each class needs to have 75% or more coverage
     - Unlocked packages: Package as a whole needs to have 75% or more coverage

   **Individual Mode:**
   * Ignore packages that are installed in the scratch org (eliminates the requirement of using a pooled org)
   * Compute changed packages by observing the diff of Pull/Merge Request
   * Validate each of the changed packages individually
   * Install any dependencies required for each package
   * Apply thorough validation rules (deployment, testing, coverage)

### Additional Options

#### Test Execution
By default, all apex tests are triggered in parallel with automated retry. Tests that fail due to SOQL locks or other transient issues are automatically retried synchronously. You can override this behavior:
- `--disableparalleltesting`: Forces all tests to run synchronously
- `--skipTesting`: Skip test execution entirely (use with caution)

#### Coverage Requirements
- Default coverage threshold: 75%
- Configure custom threshold: `--coveragepercent <value>`
- Coverage is validated per class for source packages and per package for unlocked packages

{% hint style="info" %}
**Best Practice**: Use "thorough" mode for comprehensive validation before merging to ensure all packages are properly tested and deployable. For faster feedback during development, consider using "individual" mode or filtering with release configs.
{% endhint %}

{% hint style="warning" %}
Skipping tests with `--skipTesting` bypasses critical quality checks. Only use this option in development environments or when you're certain the changes don't require test validation.
{% endhint %}

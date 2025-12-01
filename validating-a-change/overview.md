# Overview

**validate** command helps you to test ((deployability, apex tests, coverage)   a change made  to your configuration / code against a target org. This command is typically triggered as part of your Pull Request (PR) or Merge process, to ensure the correctness of configuration/code, before being merged into your **main** branch.<br>

{% hint style="warning" %}
sfp validates a change by deploying the changed packages into the target org. This is different from 'check only' deployment in other CI/CD solutions.
{% endhint %}

\
validate can either utilise a scratch org from a tagged pool prepared earlier using the [prepare](broken-reference/)[ ](https://github.com/dxatscale/dxatscale-guide/blob/april-22/projects/sfpowerscripts/orchestrator/broken-reference/README.md)command or one could use the a target org for its purpose

```
// An example where validate is utilized against a pool 
// of earlier prepared scratch orgs with label review
 
sfp validate pool  -p review \
                   -v devhub  \
                   --diffcheck
```

```
// An example where validate is utilized against a target org 
sfp validate org -o ci \
                 -v devhub  \
                 --installdeps
```

```
// An example with branch references for intelligent synchronization
sfp validate org -o ci \
                 -v devhub \
                 --ref feature-branch \
                 --baseRef main
```

```
// An example with release config for domain-limited validation
sfp validate org -o ci \
                 -v devhub \
                 --releaseconfig config/release-sales.yml
```

\
\
**validate pool / validate org** command runs the following checks with the options to enable additional features such as dependency and impact analysis:

* Checks accuracy of metadata by deploying the metadata to an org
* Triggers Apex Tests
* Validates Apex Test Coverage of each package (default: 75%)
* Toggle between different modes for validation:
  * **Thorough** _(Default)_ - Comprehensive validation with full deployments and all tests
  * **Individual** - Validates changed packages individually, ideal for PRs
* Automatically differentiates between:
  * Packages to synchronize (upstream changes)
  * Packages to validate (PR changes)
* \[optional] - **AI-Assisted Error Analysis** - Intelligent error analysis and fix suggestions ([learn more](ai-assisted-error-analysis.md))
* \[optional] - Limit validation scope using release configurations (`--releaseconfig`)
* \[optional] - Validate dependencies between packages for changed components
* \[optional] - Disable diff check while validating (`--diffcheck`)
* \[optional] - Disable parallel testing of apex tests (`--disableparalleltesting`)
* \[optional] - Skip test execution entirely (`--skipTesting`)
* \[optional] - Execute custom [validation scripts](validation-scripts.md) for setup/cleanup workflows

# Overview

**validate** command helps you to validate a change made to your configuration / code against a target org.  This command is typically triggered as part of your Pull Request (PR) or Merge process, to ensure the correctness of configuration/code, before being merged into your **main** branch. \
\
validate can either utilise  a scratch org from a tagged pool prepared earlier using the [prepare](broken-reference)[ ](https://github.com/dxatscale/dxatscale-guide/blob/april-22/projects/sfpowerscripts/orchestrator/broken-reference/README.md)command or one could use the a target org for its purpose

```
// An example where validate is utilized against a pool 
// of earlier prepared scratch orgs with label review
 
sfp validate pool  -p review \
                   -v devhub  \
                   --diffcheck
```

```
// An example where validate is utilized against a target org 
sfp validate org -u  ci \
                 -v devhub  \
                 --installdeps  \
```

\
\
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

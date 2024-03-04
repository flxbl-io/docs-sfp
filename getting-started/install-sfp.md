# Install sfp

## A. Install sfp in your local machine

```
npm i -g @flxblio/sfp
```

## B. Version Check

```
sfp --version
@flxblio/sfp/36.0.10 darwin-arm64 node-v20.3.1
```

## C. Command List

```
sfp commands

Command                         Summary
 ─────────────────────────────── ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 apextests trigger               Triggers Apex unit test in an org. Supports test level RunAllTestsInPackage, which optionally allows validation of individual class code coverage
 artifacts fetch                 Fetch sfp artifacts from a NPM compatible registry using a release definition file
 artifacts promote               Promotes artifacts predominantly for unlocked packages with code coverage greater than 75%
 artifacts query                 Fetch details about artifacts installed in a target org
 build                           Build artifact(s) of your packages in the current project
 changelog generate              Generates release changelog, providing a summary of artifact versions, work items and commits introduced in a release. Creates a release definition based on art…
 commands                        list all the commands
 dependency expand               Expand the dependency list in sfdx-project.json file for each package, fix the gap of dependencies from its dependent packages
 dependency install              Install all the external dependencies of a given project
 dependency shrink               Shrink the dependency list in sfdx-project.json file for each package, remove duplicate dependencies that already exist in its dependent packages
 deploy                          Installs artifact(s) from a given directory to a target org
 flow activate                   Activate the flow on a target org
 flow cleanup                    Cleanup inactive flows on a target org
 flow deactivate                 Deactivate the flow on a target org
 help                            Display help for @flxblio/sfp.
 impact package                  Figures out impacted packages of a project, due to a change from the last known tags
 impact releaseconfig            Figures out impacted release configurations of a project, due to a change,from the last known tags
 install                         Installs artifact(s) from a given directory to a target org
 metrics report                  Report a custom metric to any sfp supported metric provider
 orchestrator build              Build artifact(s) of your packages in the current project
 orchestrator deploy             Installs artifact(s) from a given directory to a target org
 orchestrator prepare            Prepare a pool of scratchorgs with all the packages upfront, so that any incoming change can be validated in an optimized manner
 orchestrator promote            Promotes artifacts predominantly for unlocked packages with code coverage greater than 75%
 orchestrator publish            Publish packages to a NPM Compatible artifact registry
 orchestrator quickbuild         Build artifact(s) of your packages in the current project without dependency validation for unlocked packages
 orchestrator release            Release a set of artifact(s) as defined by a release definition into a target org
 orchestrator validate           Validate a change in your project repository against a scratch org prepared by the prepare command
 orchestrator validateagainstorg Validate a change in your project repository against a provided org
 pool delete                     Deletes the pooled scratch orgs from the Scratch Org Pool
 pool fetch                      Gets an active/unused scratch org from the scratch org pool
 pool list                       Retrieves a list of active scratch org and details from any pool. If this command is run with -m|--mypool, the command will retrieve the passwords for the pool …
 pool metrics publish            Publish metrics about scratch org pools to your observability platform, via StatsD or direct APIs for supported platforms
 pool org delete                 Deletes a particular scratch org in the pool, This command is to be used in a pipeline with correct permissions to delete any active scratch org record or to be…
 pool prepare                    Prepare a pool of scratchorgs with all the packages upfront, so that any incoming change can be validated in an optimized manner
 prepare                         Prepare a pool of scratchorgs with all the packages upfront, so that any incoming change can be validated in an optimized manner
 profile merge                   Merge changes made in the profile directly in the org to the profile files in the local project
 profile reconcile               Reconcile profiles in the local directory only with the attributes that are available in the target org
 profile retrieve                Retrieve profiles from the salesforce org with all its associated permissions. Common use case for this command is  to migrate profile changes from a integratio…
 publish                         Publish packages to a NPM Compatible artifact registry
 quickbuild                      Build artifact(s) of your packages in the current project without dependency validation for unlocked packages
 release                         Release a set of artifact(s) as defined by a release definition into a target org
 releasedefinition generate      Generates release definition based on the artifacts at the specified head of source branch/commit ref
 repo patch                      Generate a dynamic branch with the packages patched to the contents as mentioned in the release config file
 validate                        Validate a change in your project repository against a scratch org prepared by the prepare command
 validateAgainstOrg              Validate a change in your project repository against a provided org
```

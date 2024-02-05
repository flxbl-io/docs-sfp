# Orchestrator

```
sfp orchestrator --help
Orchestrate packages from a monorepo through its lifecycle, driven by descriptors in your sfdx-project.json

USAGE
  $ @flxblio/sfp orchestrator:COMMAND

COMMANDS
  orchestrator:build               Build all packages (unlocked/source/data) in a repo in parallel, respecting the dependency
                                   of each packages and generate artifacts to a provided directory
  orchestrator:deploy              Deploy packages from the provided aritfact directory, to a given org, using the order and
                                   configurable flags provided in sfdx-project.json
  orchestrator:prepare             Prepare a pool of scratchorgs with all the packages upfront, so that any incoming change
                                   can be validated in an optimized manner
  orchestrator:promote             Promotes validated unlocked packages with code coverage greater than 75%
  orchestrator:publish             Publish packages to an artifact registry, using a user-provided script that is responsible
                                   for authenticating & uploading to the registry.
  orchestrator:quickbuild          Build packages (unlocked/source/data) in a repo in parallel, without validating depenencies
                                   or coverage in the case of unlocked packages
  orchestrator:release             Release a  collection of artifacts as defined in the release definition file
  orchestrator:validate            Validate the incoming change against an earlier prepared scratchorg
  orchestrator:validateAgainstOrg  Validate the incoming change against target org
```

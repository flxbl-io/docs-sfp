# Deploying Artifacts

The `deploy` command streamlines the deployment process by eliminating the need to script individual package deployments. It automatically determines which packages to deploy and their deployment sequence based on the project configuration file (sfdx-project.json) contained within the packages.&#x20;

`sfp deploy -u <target-org> --arifactdir <path to artifact directory>` \
\
When provided with a directory of artifacts and a target organization, the `deploy` command will efficiently deploy the packages to the target organization as dictated by the order specified in the leading artifact's sfdx-project.json

{% hint style="info" %}
For the deploy command to work, the artifacts must be created from the same source repository
{% endhint %}

`deploy` command operates on artifacts, as artifacts are immutable,  and any deployment related properties need to be activated during the [build](../build-artifacts.md) command, Any deployment related attributes that are not part of the artifact this is built will not be activated during the executions of the command

---
description: >-
  This page provides an explanation of the alwaysDeploy and
  skipIfAlreadyInstalled commands in deployment pipelines,
---

# Understanding alwaysDeploy and skipIfAlreadyInstalled in Deployment Pipelines

## Overview

The `alwaysDeploy` attribute for packages is designed to always deploy a package, regardless of other conditions. However, it only runs if the package is available in the collection of artifacts to be deployed. It then deploys the package, regardless of whether the package was previously installed or not.

On the other hand, the `skipIfAlreadyInstalled` attribute allows the deployment process to skip the installation of a package if it is already installed.

## Interaction between `alwaysDeploy` and `skipIfAlreadyInstalled`

The `alwaysDeploy` attribute should work when `skipIfAlreadyInstalled` is set to true. However, it's important to note that `alwaysDeploy` only deploys a package if it was built in the previous step. Therefore, if your requirement is to always deploy a package, regardless of whether it was changed or not, `alwaysDeploy` will not solve your requirement.

## The Install Command

The `install` command is based on the existing artifact files in the artifacts directory. The `--artifacts --release-config` parameter filters artifacts  available for usage. The same applies for `alwaysDeploy`; if the artifact is not available in the provided directory, it won't install because there is no such artifact available. In contrast, the `release` command first fetches the artifacts (based on the config file) then installs them.

In summary, the flag "alwaysDeploy" could be better described as: _If the artifact is available, then deploy it_.

## Workaround for Always Deploying a Package while using install command

&#x20;If you need to always deploy a package ensure you fetch the artifact into the artifact directory, so sfp's command will pick up the artifact as part of the collection

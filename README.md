# Overview

Looking to explore new ways of managing deployments and releases on Salesforce. &#x20;



## Assumption

* No Pipelines will be addressed here.  All deployments, validations, will stick to the use of command line.&#x20;
* Using GitHub Online will be used for repository and NPM registry

## Challenge

1. Install and use sfp to replace the workflow of sf cli to manage your metadata and deploy across 2 environments.

## Takeaways

* Artifact-Based Release
* Seamless Process



## Key Differences

* Piggy-back on sfdx-project.json file&#x20;



High Level Existing Strategies Concepts

* Org-based Branching Approaches
* Shared Development Sandboxes
* Scratch Orgs (Includes Org Shapes, Snapshots (beta)
* Path to Deployment (DEV > TEST > PROD)
* Delta Deployments
* Full Deployments


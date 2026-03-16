---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/
---

# Overview

**sfp** is a purpose-built CLI tool for modular Salesforce development and release management. **sfp** streamlines and automates the build, test, and deployment processes of Salesforce metadata, code, and data. It extends **sf cli** functionalities, focusing on artifact-driven development to support #flxbl Salesforce project development.

**sfp** contains features including a  server which extends your DevHub , environment management, authentication services, and facilitating collaboration within team.

## Key Aspects of sfp:

* **Built with codified process**: sfp is derived from extensive experience in modular Salesforce implementations. By embracing the #FLXBL framework, it streamlines the process of creating a well-architected, composable Salesforce Org, eliminating time-consuming efforts usually spent on re-inventing fundamental processes.
* **Artifact-Centric Approach**: **sfp** packages Salesforce code and metadata into artifacts with deployment details, ensuring consistent deployments and simplified version management across environments.
* **Best-in-Class Mono Repo Support**: Offers robust support for mono repositories, facilitating streamlined development, integration, and collaboration.
* **Support for Multiple Package Types**: **sfp** accommodates various Salesforce package types with streamlined commands, enabling modular development, independent versioning, and flexible deployment strategies.
* **Orchestrate Across Entire Lifecycle**: **sfp** provides an extensive set of functionality across the entire lifecycle of your Salesforce development.
* **End-to-End Observability**: **sfp** is built with comprehensive metrics emitted on every command, providing unparalleled visibility into your ALM process.
* **Centralized Server (sfp-pro)**: A backend server that provides environment management, authentication, webhooks, and API access for team-based workflows.

<div data-full-width="false"><figure><img src=".gitbook/assets/concept (2).png" alt=""><figcaption><p>A Salesforce Project (in a git repository)</p></figcaption></figure></div>

## Commands

```
sfp
├── ai - Commands to manage AI Providers
│   └── test
├── apextests - Manage apex tests in a package or org
│   ├── resume
│   └── trigger
├── artifacts - Manage artifacts for a project
│   ├── fetch 
│   ├── list 
│   ├── promote 
│   └── query
├── auth - Authenticate with the SFP server
│   ├── clear 
│   ├── display 
│   ├── list 
│   └── login 
├── config - Manage CLI configuration settings
│   ├── get
│   ├── list
│   ├── set
│   └── unset
├── dependency - Manage the dependencies of a project
│   ├── expand
│   ├── explain
│   ├── install
│   └── shrink
├── dev - Commands to manage developers in your org
│   ├── create
│   └── grant
├── flow - Manage Salesforce flows in your org
│   ├── activate
│   ├── cleanup
│   └── deactivate
├── flows - Run and manage long-running flows (sandbox operations, privilege elevation, etc.)
│   ├── approve 
│   ├── cancel 
│   ├── list 
│   ├── reject 
│   ├── run
│   ├── types 
│   └── watch 
├── impact - Utilities to help you understand the impact of various components
│   ├── apex
│   ├── package
│   └── releaseconfig
├── metrics - Report, query and display metrics from sfp server
│   ├── display 
│   ├── query 
│   └── report
├── org - Commands to manage orgs (authenticate, open, list)
│   ├── list
│   ├── login 
│   └── open
├── package - Commands to manage packages in your org
│   ├── create - Create diff/data/source/unlocked packages
│   │   ├── data
│   │   ├── diff
│   │   ├── source
│   │   └── unlocked
│   └── install
├── pool - Build and manage scratch org or sandbox pools
│   ├── delete 
│   ├── extend 
│   ├── fetch 
│   ├── list 
│   ├── probe
│   ├── scratch - Build and manage scratch org pools
│   │   └── init
│   └── unassign 
├── profile - Commands to manage profiles in your project or org
│   ├── merge
│   ├── reconcile
│   └── retrieve
├── project - Commands to manage your project
│   ├── analyze 
│   ├── pull
│   ├── push
│   └── report
├── releasecandidate - Manage release candidates
│   ├── abort 
│   ├── fetch 
│   ├── finalize 
│   ├── generate 
│   ├── list 
│   ├── status 
│   └── update 
├── releaseconfig - Probe and manage release configurations
│   └── probe
├── repo - Manage your project repository
│   ├── clone 
│   ├── list 
│   ├── patch 
│   └── visualize 
├── review-envs - Acquire and manage review environments
│   ├── acquire 
│   ├── assign 
│   ├── extend 
│   ├── list 
│   ├── release 
│   ├── rules - List and test pool assignment rules
│   │   ├── list
│   │   └── match 
│   └── unassign 
├── sandbox - Commands to manage sandboxes
│   ├── create
│   ├── delete-with-cleanup 
│   ├── delete
│   ├── list
│   ├── login 
│   ├── update
│   └── validate
├── scratch - Commands to manage scratch orgs
│   ├── delete
│   └── login 
├── user - Commands to manage users in your org
│   ├── deactivate
│   ├── freeze
│   └── unfreeze
├── validate - Validate a change in your project repository
│   ├── org 
│   └── pool 
├── build 
├── install
├── publish 
├── quickbuild 
├── release 
├── update

```

### Server Adminstration Commands

```
*🔧 Admin Commands*

└── 📁 *server* - Commands for administrators to manage sfp server
    ├── 📁 *application-token* - Manage application tokens
    │   ├── `create` 🌐
    │   ├── `list` 🌐
    │   └── `revoke` 🌐
    ├── `audit` 🌐
    ├── 📁 *builds* - Manage builds
    │   └── `list` 🌐
    ├── 📁 *callback* - Send callback notifications (GitHub, Slack)
    │   └── `post` 🌐
    ├── 📁 *doc-store* - Manage document store
    │   ├── `add` 🌐
    │   ├── 📁 *collection*
    │   │   ├── `add` 🌐
    │   │   ├── `delete` 🌐
    │   │   ├── `get` 🌐
    │   │   ├── `list` 🌐
    │   │   └── `update` 🌐
    │   ├── `delete` 🌐
    │   ├── `get` 🌐
    │   └── `update` 🌐
    ├── 📁 *environment* - Manage environments
    │   ├── `activate-by-sandbox` 🌐
    │   ├── `activate` 🌐
    │   ├── `compare` 🌐
    │   ├── `create` 🌐
    │   ├── `deactivate-by-sandbox` 🌐
    │   ├── `deactivate` 🌐
    │   ├── `delete-by-sandbox` 🌐
    │   ├── `delete` 🌐
    │   ├── `find` 🌐
    │   ├── `get` 🌐
    │   ├── `list` 🌐
    │   ├── `lock` 🌐
    │   ├── `unlock` 🌐
    │   └── `update` 🌐
    ├── `health` 🌐
    ├── `init` 🌐
    ├── 📁 *integration* - Manage integrations
    │   ├── `create` 🌐
    │   └── `get` 🌐
    ├── 📁 *key-value* - Manage key-value store
    │   ├── `add` 🌐
    │   ├── `delete` 🌐
    │   ├── `get` 🌐
    │   └── `update` 🌐
    ├── `logs`
    ├── 📁 *org* - Manage Salesforce org registrations
    │   ├── `delete` 🌐
    │   ├── `get-default-devhub` 🌐
    │   ├── `register-sandbox` 🌐
    │   ├── `register` 🌐
    │   └── `update` 🌐
    ├── 📁 *pool* - Manage scratch org and sandbox pools
    │   ├── `cleanup` 🌐
    │   ├── 📁 *config* - Manage pool configurations
    │   │   ├── `create` 🌐
    │   │   ├── `delete` 🌐
    │   │   ├── `get` 🌐
    │   │   ├── `list` 🌐
    │   │   └── `update` 🌐
    │   ├── `monitor` 🌐
    │   ├── `provision` 🌐
    │   ├── `replenish` 🌐
    │   └── `status` 🌐
    ├── 📁 *project* - Manage projects
    │   ├── `create` 🌐
    │   ├── `get` 🌐
    │   ├── `list` 🌐
    │   └── `update` 🌐
    ├── 📁 *repository* - Manage repository authentication
    │   ├── `auth-token` 🌐
    │   └── `npmrc` 🌐
    ├── 📁 *review-envs* - Manage review environment rules
    │   └── 📁 *rules* - Create, update, delete pool assignment rules
    │       ├── `create` 🌐
    │       ├── `delete` 🌐
    │       └── `update` 🌐
    ├── `scale`
    ├── 📁 *slack* - Manage Slack integration
    │   ├── `delete` 🌐
    │   ├── `list` 🌐
    │   ├── `manifest`
    │   ├── `setup` 🌐
    │   ├── `status` 🌐
    │   ├── `test` 🌐
    │   └── `update` 🌐
    ├── `start` 🌐
    ├── `status`
    ├── `stop`
    ├── `update` 🌐
    ├── 📁 *user* - Manage users
    │   ├── `add` 🌐
    │   └── `delete` 🌐
    └── 📁 *webhook* - Manage webhooks
        ├── `delete` 🌐
        ├── `get` 🌐
        ├── `list` 🌐
        ├── `register` 🌐
        ├── `trigger-event` 🌐
        └── `update` 🌐
```


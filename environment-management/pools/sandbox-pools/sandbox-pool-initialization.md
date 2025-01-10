---
icon: ring-diamond
---

# Sandbox Pool Initialization

|              | sfp-pro       | sfp (community) |
| ------------ | ------------- | --------------- |
| Availability | ✅             | ❌               |
| From         | September '24 |                 |

The `sfp pool sandbox init` command is used to create and initialize a pool of Salesforce sandboxes.



### Usage

```
sfp pool sandbox init -f <path/to/config-file> -v <devhub-alias> -r <owner/repo>
```

### Flags

* `-f, --poolconfig=<path>`: Path to the sandbox pool configuration file (default: 'config/sandbox-pool-config.json')
* `-v, --targetdevhubusername=<devhub-alias>`: Alias of the target Dev Hub org
* `-r, --repo=<owner/repo>`: GitHub repository in the format owner/repo

### Configuration File

The configuration file should be a JSON file containing an array of pool configurations. Each configuration should include:

```json
[
  {
    "pool": "DEV",
    "count": 3,
    "sourceSB": "production",
    "branch": "develop",
    "defaultExpirationHours": 24,
    "extendedExpirationHours": 24,
    "averageOrgCreationTime": 2
  }
]
```

* `pool`: Name of the sandbox pool (will be converted to uppercase)
* `count`: Number of sandboxes to create for this pool
* `sourceSB`: Source sandbox name (use 'production' for creating from scratch)
* `branch`: Git branch associated with this pool
* `defaultExpirationHours`: (Optional) Default expiration time in hours (default: 24)
* `extendedExpirationHours`: (Optional) Extended expiration time in hours (default: 24)
* `averageOrgCreationTime`: (Optional) Average time in hours for sandbox creation (default: 2)

### Process

1. Reads the configuration file
2. Authenticates with GitHub
3. Creates sandboxes for each pool configuration
4. Sets up GitHub repository variables for tracking sandboxes

### Example

```
sfp pool sandbox init -f config/my-sandbox-pools.json -v my-devhub -r myorg/myrepo
```

This command initializes sandbox pools as defined in `my-sandbox-pools.json`, using the Dev Hub 'my-devhub', and creates corresponding variables in the GitHub repository 'myorg/myrepo'.



Please note that for this feature to work, you need to use GitHub/GitLab token and have atleast maintainer access

```
GITHUB 
-------------------------
EXPORT GITHUB=1
EXPORT GITHUB_TOKEN=<YOUR_GITHUB_TOKEN> // GITHUB TOKEN NEEDS REPO SCOPE

GITLAB
-------------------------------
EXPORT  GITLAB=1
EXPORT GITLAB_TOKEN=<YOUR_GITLAB_TOKEN> //  GITLAB TOKEN NEEDS API SCOPE
```

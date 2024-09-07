---
icon: ring-diamond
---

# Monitor Sandbox Pools

|              | sfp-pro      | sfp (community) |
| ------------ | ------------ | --------------- |
| Availability | ✅            | ❌               |
| From         | September 24 |                 |

The `sfp sandbox monitor` command is used to monitor the status of sandboxes in pools, activate new sandboxes, handle expirations, and manage deletions. This command is designed to be run as a continuous cron job.

### Usage

```
sfp sandbox monitor -v <devhub-alias> -r <owner/repo> -f <path/to/config-file> [<path/to/config-file>...]
```

### Flags

* `-v, --targetdevhubusername`: Alias of the target Dev Hub org (required)
* `-r, --repo`: Repository in the format owner/repo (required)
* `-f, --configfile`: Path to the sandbox pool configuration file(s) (required, can be specified multiple times)

### Continuous Monitoring

This command should be set up as a cron job to run at regular intervals (e.g., every 15-30 minutes). This ensures:

1. Prompt activation of newly created sandboxes
2. Timely identification and marking of expired sandboxes
3. Deletion of marked sandboxes to free up resources
4. Maintenance of a healthy and efficient sandbox pool

### Processes

#### Sandbox Activation

* Identifies sandboxes in 'InProgress' state
* Attempts to activate them
* Updates their status in GitHub variables

#### Expiration Checking

* Checks sandboxes against their expiration times
* Considers default expiration, extended expiration, and average creation time
* Marks expired sandboxes in GitHub variables

#### Sandbox Deletion

* Identifies sandboxes marked as 'Expired'
* Attempts to delete them from the Salesforce org
* Removes corresponding GitHub variables
* Logs results of the deletion process

### Expiration Times

* **Default Expiration**: 24 hours (configurable)
* **Extended Expiration**: Additional 24 hours (configurable)
* **Average Creation Time**: 2 hours (configurable)

These times can be customized in the pool configuration file.

### Example

```
sfp sandbox monitor -v my-devhub -r myorg/myrepo -f config/pool-config-1.json config/pool-config-2.json
```

This command monitors sandbox pools defined in both configuration files, using the Dev Hub 'my-devhub', and updates statuses in the 'myorg/myrepo' GitHub repository.

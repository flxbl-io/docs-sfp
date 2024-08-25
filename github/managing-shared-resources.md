# Managing Shared Resources

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards). You also need an equivalent APP\_ID & PRIVATE\_KEY in your environment variable to access these commands.
{% endhint %}

sfp provides commands which  allow you to coordinate access to shared resources across multiple processes or machines using a distributed locking mechanism. The locks are stored in a GitHub repository, and the commands provide an easy-to-use interface for acquiring, releasing, and waiting for locks.

## Commands

### 1.  Request a lock on a resource

Command:

```bash
$ sfp gh resource enqueue --repository owner/repo --resource resource-name --leasefor 1800
```

Description: This command is used to acquire a lock on a specific resource. It returns a ticket ID representing the lock.

Options:

* `--repository`: Specifies the GitHub repository where the lock information will be stored. Defaults to the value of the `GITHUB_REPOSITORY` environment variable.
* `--resource`: Specifies the name of the resource to be locked. Defaults to `'gh-vars'`.
* `--leasefor`: Specifies the lease duration for the lock in seconds. Defaults to 900 seconds (15 mins)

### 2. Wait for a Lock

Command:

```bash
$ sfp gh resource wait --repository owner/repo --resource resource-name --ticketid ticket-id --timeout 1800
```

Description: This command is used to wait for a lock to be acquired. It will block until the lock is successfully acquired or the specified timeout is reached.

Options:

* `--repository` and `--resource`: Same as in the `Lock` command.
* `--ticketid`: Specifies the ticket ID returned by the `Lock` command, representing the lock to wait for.
* `--timeout`: Specifies the maximum time in seconds to wait for the lock. Defaults to 900 seconds (15 mins).

### 3. Unlock a Resource

Command:

```bash
$ sfp gh resource dequeue --repository owner/repo --resource resource-name --ticketid ticket-id
```

Description: This command is used to release a lock on a specific resource for the particular ticket

Options:

* `--repository` and `--resource`: Same as in the `Lock` command.
* `--ticketid`: Specifies the ticket ID associated with the lock to be released.

## Usage Scenarios

1. **Exclusive Access to a Shared Resource:**
   * Use the `enqueue` command to request a lock on a shared resource before accessing it.
   * Use the `wait` command to wait for the lock to be acquired before proceeding with the tasks.
   * After finishing the operations, use the `dequeue` command to release the lock, allowing other processes to access the resource.
2. **Coordinating Dependent Tasks:**
   * Use the `enqueue` command to acquire locks on multiple resources required for a set of dependent tasks.
   * Use the `wait` command to wait for the locks to be acquired before proceeding with the tasks.
   * After completing the tasks, use the `dequeue` command to release the locks.
3. Preventing Concurrent Modifications:
   * Use the `enqueue` command to acquire a lock before modifying a shared resource.
   * Use the `wait` command to wait for the lock to be acquired before proceeding with the tasks.
   * Perform the modifications while holding the lock to ensure exclusive access.
   * Use the `dequeue` command to release the lock once the modifications are complete.

## Considerations

* Choose a unique and descriptive name for the `--resource` option to avoid conflicts with other locks.
* Specify an appropriate lease duration (`--leasefor`) based on the expected time required to perform the desired operations.
* Always release locks using the `dequeue` command after finishing the operations to avoid holding locks unnecessarily.
* Handle exceptions and errors appropriately when using the commands, such as retrying with a longer timeout or gracefully failing.
* Be mindful of GitHub's API rate limits, especially when dealing with a high volume of lock requests.

By following these usage guidelines and consideations, you can effectively utilize the provided commands to coordinate access to shared resources and ensure proper synchronization across multiple processes or machines.

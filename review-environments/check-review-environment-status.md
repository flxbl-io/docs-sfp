---
icon: ring-diamond
---

# Check Review Environment Status



{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

The `sfp reviewenv check` command allows you to check the status of review environments assigned to a specific pull request/issue. This command is typically used in a CI/CD workflow to determine if a pull request already has an environment assigned and to get details about that environment.

#### Usage

```
sfp reviewenv check --repository <owner/repo> --issue <issue> [--pool <pool>] [--poolType <type>] [--branch <branch>]
```

#### Options

* `-r, --repository`: The repository path that stores the pool lock (default: current repo). This is used to coordinate environment assignment across multiple pull requests.
* `-i, --issue`: The pull request number to check for assigned environments (required). This links the environment to a specific pull request.
* `-p, --pool`: The name of the pool to filter by. Use this to only check for environments from a specific pool.
* `-p, --poolType`: The type of the pool to filter by, either `sandbox` or `scratchorg`. Use this to only check for environments of a certain type.
* `-b, --branch`: The pull request branch to filter by. Use this to only check for environments assigned to a specific branch.

#### How it works in a GitHub Pull Request Workflow

1. When a pull request is opened or updated, a GitHub Actions workflow is triggered.
2. The workflow uses the `sfp reviewenv check` command to determine if the pull request already has a review environment assigned.
3. The command searches the repository's stored environment data (in GitHub repository variables) for any environments associated with the specified pull request number.
4. If an assigned environment is found, the command outputs the environment details, including:
   * Environment name or username
   * Environment type (sandbox or scratch org)
   * Pool the environment belongs to
   * Associated pull request branch
   * Environment expiration date
5. The workflow can then use this information to determine if it needs to fetch a new environment (if none assigned) or if it can reuse the existing assigned environment.


---
icon: ring-diamond
---

# Fetch a Review Environment

{% hint style="info" %}
The commands are only available in sfp-pro (August 24 onwards) and currently limited to GitHub. Using these commands requires an equivalent APP\_ID & PRIVATE\_KEY in your environment variable.
{% endhint %}

The `sfp reviewenv fetch` command allows you to fetch a review environment (scratch org or sandbox) from a preconfigured pool during a pull request workflow. Review environments are ephemeral environments used to validate deployments, run tests, and perform acceptance testing in a flxbl project

Review environments provide several key benefits in a CI/CD workflow:

1. **Isolation**: Each pull request gets its own isolated environment, preventing conflicts between different features or bug fixes.
2. **Reproducibility**: Review environments are created from a consistent pool configuration, ensuring all changes are tested in a predictable and reproducible manner.
3. **Validation**: Deploying to a review environment allows you to catch any deployment issues early in the development process.
4. **Testing**: Automated tests can be run against the review environment to verify the changes work as expected and haven't introduced any regressions.
5. **Collaboration**: Review environments provide a shared space for team members to perform acceptance testing, provide feedback, and sign off on changes before merging.
6. **Compliance**: Review environments can be used to enforce governance policies and ensure all changes adhere to organizational standards before being merged into the main branch.

#### Usage

```
sfp reviewenv fetch --repository <owner/repo> --pool <pool> --poolType <type> --branch <branch> --issue <issue> [--devhubAlias <alias>] [--wait <minutes>] [--leaseFor <minutes>]
```

#### Options

* `-r, --repository`: The repository path that stores the pool lock (default: current repo). This is used to coordinate environment assignment across multiple pull requests.
* `-p, --pool`: The name of the pool to fetch the review environment from (required). Pools are predefined sets of environments.
* `-t, --poolType`: The type of the pool, either `sandbox` or `scratchorg` (required). This specifies whether to fetch a sandbox or a scratch org.
* `-b, --branch`: The pull request branch to fetch the environment for (required). This is used to isolate the changes.
* `-i, --issue`: The pull request number to assign the environment to (required). This links the environment to a specific pull request.
* `--devhubAlias`: The DevHub alias associated with the pool (default: `devhub`). This is the DevHub org used to create scratch orgs.
* `-w, --wait`: Time in minutes to wait for an environment to become available (default: 20). If no environments are immediately available, the command will wait up to this long for one to free up.
* `-l, --leaseFor`: Time in minutes to lease the environment for (default: 20). This sets how long the environment will be assigned to the pull request before automatically freeing it up.

#### How it works in a GitHub Pull Request Workflow

1. When a pull request is opened or updated,  trigger a github workflow with command configured
2. The workflow uses the `sfp reviewenv fetch` command to request a review environment from the specified pool.
3. The command checks if an environment is already assigned to the pull request. If not, it locks the pool and fetches the next available environment.
4. Once an environment is fetched, it is marked as "InUse" and assigned to the pull request number.
5. The workflow then deploys the pull request changes to the review environment.
6. Automated tests are run against the review environment to validate the changes.
7. The workflow updates the pull request with a link to the review environment, allowing reviewers to perform manual testing and provide feedback.
8. Once the pull request is closed (either merged or declined), another workflow is triggered to expire the environments

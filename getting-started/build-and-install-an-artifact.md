---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/getting-started/build-and-install-an-artifact
---

# Build & Install an Artifact

Now that sfp is installed and connected to your server, let's walk through the core commands.

### A. Build an artifact for a package

The build command generates a zipped artifact file for each package defined in your `sfdx-project.json`. The artifact contains the metadata and source code at the point of build creation.

1. Authenticate to your DevHub locally if you haven't already:

```bash
sf org login web --alias devhub --set-default-dev-hub
```

2. Open a terminal within your Salesforce project directory and run:

```bash
sfp build -v devhub
```

The `-v` flag specifies your DevHub alias, which is required for building packages.

You will see logs with details of your package creation:

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption><p>Build Outputs</p></figcaption></figure>

2. A new `artifacts` folder will be generated containing a zipped artifact file for each package.\
   \
   For example:\
   `package-name_sfpowerscripts_artifact_1.0.0-1.zip`

    <figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>artifacts folder</p></figcaption></figure>

### B. Install the artifact to a target org

You can install artifacts to any org that is accessible. If the org is registered with your sfp server, you can authenticate through the server:

```bash
# Authenticate to an environment via the server
sfp server environment get \
  --name UAT \
  --repository your-org/your-repo \
  --auth-type accessToken \
  --authenticate
```

Or authenticate directly using sf CLI:

```bash
sf org login web --alias targetOrg
```

Then install:

```bash
sfp install -o <TargetOrgAlias>
```

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption><p>Install Outputs</p></figcaption></figure>

Navigate to your target org and confirm that the packages are installed with the expected changes.

{% hint style="info" %}
Depending on the type of packages, [sfp will issue the equivalent test classes](../concepts/supported-package-types/) within the package directory and it could result in failures during installation. Fix the issues in your code and repeat till you get a successful installation. If your packages don't have sufficient test coverage, you may need to use all tests in the org. Refer to [optimized installation](../building-artifacts/configuring-installation-behaviour-of-a-package/optimized-installation.md).
{% endhint %}

# Dealing with Long Build Times in Salesforce

Ocassionally due to release windows or  on packages that have excessive dependencies, the build time will for an unlocked package will be excessive, this section deals on the possible solutions available

## Possible Solutions

1. **Wait for Salesforce Updates**: The issue should improve once the system is updated to next release for your DevHub
2. **Utilize Scratch Org Snapshots**: This is particularly useful for packages with many managed dependencies. However, it's worth noting that there are still some bugs affecting package creation in the Closed Beta of this feature.
3. **Switch to Source Packages Temporarily**: This can be done by renaming the package aliases in `sfdx-project.json`. However, dependencies to other source packages need to be removed for each source package, as source packages cannot be listed as dependencies for other source packages.

```json
{
  "packageAliases": {
    "myPackage": "04t..."
  }
}
```


# Building an artifact individually

An artifact can be build individually for a package, sfp will determine the [type of a package ](../concepts/identifying-types-of-a-package.md) and the corresponding api's will be invoked

```
// Sample sfdx project json 
{
  "packageDirectories": [
    {
      "path": "./src-env-specific-pre",
      "package": "src-env-specific-pre",
      "versionNumber": "1.0.0.0",
    },
    {
      "path": "./src/frameworks/feature-mgmt",
      "package": "feature-mgmt",
      "versionNumber": "1
    }
  ]
}
```



```
// Build a single package
sfp build -v devhub --branch=main -p feature-mgmt
```

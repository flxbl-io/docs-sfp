# Controlling aspects of the build

### **Ignoring Packages from being built**

Using the `ignoreOnStage:[ "build" ]` property on a package, causes the particular package to be skipped by the **build** command. Similarly you can use `ignoreOnStage:[ "quickbuild" ]` to skip packages in the quickbuild stage.

```
{
  "packageDirectories": [
    {
      "path": "./src-env-specific-pre",
      "package": "src-env-specific-pre",
      "versionNumber": "1.0.0.0",
      "ignoreOnStage": [
        "build"
      ]
    },
    {
      "path": "./src/frameworks/feature-mgmt",
      "package": "feature-mgmt",
      "versionNumber": "1
    }
  ]
}
```

### Build a collection of packages together

In certain scenarios, it's necessary to build a new version of a package when any package in the collection undergoes changes. This can be accomplished by utilizing the `buildCollection` attribute in the `sfdx-project.json` file.

To ensure that a new version of a package is built whenever any package in a specified collection undergoes a change, you can use the `buildCollection` attribute in the `sfdx-project.json` file. Below is an example illustrating how to define a collection of packages that should be built together.

<pre class="language-json"><code class="lang-json">{
  "packageDirectories": [
    {
      "path": "core",
      "package": "core-package",
      "versionName": "Core 1.0",
      "versionNumber": "1.0.0.NEXT",
      "default": true,
      "buildCollection": [
        "core-package",
        "featureA-package",
        "featureB-package"
      ]
    },
    {
      "path": "features/featureA",
      "package": "featureA-package",
      "versionName": "Feature A 1.0",
      "versionNumber": "1.0.0.NEXT"
       "<a data-footnote-ref href="#user-content-fn-1">buildCollection</a>": [
        "core-package",
        "featureA-package",
        "featureB-package"
      ]
    },
    {
      "path": "features/featureB",
      "package": "featureB-package",
      "versionName": "Feature B 1.0",
      "versionNumber": "1.0.0.NEXT",
      "buildCollection": [
        "core-package",
        "featureA-package",
        "featureB-package"
      ]
    }
  ],
}
</code></pre>

In this example, the `buildCollection` is set to include "core-package", "featureA-package", and "featureB-package". This configuration means that when any one of these packages is updated and a build command is issued, all packages listed under `buildCollection` will be built together, ensuring consistency and compatibility within the collection.

### Overriding the default .forceignore file

Sometimes, due to certain platform errors, some metadata components need to be ignored during **build**. sfp offer you an easy mechanism which allows to switch .forceignore files depending on the operation.

Add this entry to your sfdx-project.json and as in the example below, mention the path to different files that need to be used for different stages

```
 "plugins": {
        "sfpowerscripts": {
            "ignoreFiles": {
                "build": "forceignores/.buildignore"
            }
        }
```



[^1]: Use of buildCollection


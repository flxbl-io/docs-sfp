# Building a collection of packages together

| Attribute     | Type  | Description                                                 | Package Types Applicable                                                               |
| ------------- | ----- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| ignoreOnStage | array | Ignore a package from being processed by a particular stage | <ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul> |



\
In certain scenarios, it's necessary to build a new version of a package when any package in the collection undergoes changes. This can be accomplished by utilizing the `buildCollection` attribute in the `sfdx-project.json` file.

To ensure that a new version of a package is built whenever any package in a specified collection undergoes a change, you can use the `buildCollection` attribute in the `sfdx-project.json` file. Below is an example illustrating how to define a collection of packages that should be built together.

<pre><code>{
  "packageDirectories": [
    {
      "path": "core",
      "package": "core-package",
      "versionName": "Core 1.0",
      "versionNumber": "1.0.0.NEXT",
      "default": true,
      "<a data-footnote-ref href="#user-content-fn-1">buildCollection</a>": [
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
       "<a data-footnote-ref href="#user-content-fn-2">buildCollection</a>": [
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
      <a data-footnote-ref href="#user-content-fn-3">"buildCollection": [</a>
        "core-package",
        "featureA-package",
        "featureB-package"
      ]
    }
  ],
}
</code></pre>

[^1]: buildCollection attribute to build packages together

[^2]: buildCollection attribute to build packages together

[^3]: buildCollection attribute to build packages together

# Ignoring packages from being built

| Attribute     | Type  | Description                                                 | Package Types Applicable                                                               |
| ------------- | ----- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| ignoreOnStage | array | Ignore a package from being processed by a particular stage | <ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul> |

\
\
Using the `ignoreOnStage:[ "build" ]` property on a package, causes the particular package to be skipped by the **build** command. Similarly you can use `ignoreOnStage:[ "quickbuild" ]` to skip packages in the quickbuild stage.

<pre><code>{
  "packageDirectories": [
    {
      "path": "./src-env-specific-pre",
      "package": "src-env-specific-pre",
      "versionNumber": "1.0.0.NEXT",
      "<a data-footnote-ref href="#user-content-fn-1">ignoreOnStage</a>": [
       "build"
      ]
    },
    {
      "path": "./src/frameworks/feature-mgmt",
      "package": "feature-mgmt",
      "versionNumber": "1.0.0.NEXT"
    }
  ]
}
</code></pre>

[^1]: Add ignoreOnStage as an additional attribute to ignore a package from being build

# Use of multiple config file in build command

The `configFile` flag in the `build` command is used for features and settings of the scratch org used to validate your unlocked package. It is optional and if not passed in, it will assume the default one which is none.

Typically, all packages in the same repo share the same scratch org definition. Therefore, you pass in the definition that you are using to build your scratch org pool and use the same to build your unlocked package.

```bash
sfp build --configFile <path-to-config-file> -v <devhub>
```

However, there is an option to use multiple definitions.

For example, if you have two packages, package 1 is dependent on `SharedActivities`, and package 2 is not. You would pass a scratch org definition file to package 1 via `scratchOrgDefFilePaths` in `sfdx-project`. Package 2 would be using. the default `definitionFile`.

<pre class="language-json"><code class="lang-json">{
  "packageDirectories": [
    {
      "path": "package1",
      "default": true,
    },
    {
      "path": "package2",
      "default": false
    }
  ],
  "plugins": {
    "sfpowerscripts":{
      "<a data-footnote-ref href="#user-content-fn-1">scratchOrgDefFilePaths</a>":{
        "enableMultiDefinitionFiles": true,
        "packages": {
          "package1":"scratchOrgDef/package1-def.json"
        }
      }
    }
  }
}
</code></pre>

[^1]: add scratchOrgDefFilePaths for multiple configurations

# Selective ignoring of components from being built

Sometimes, due to certain platform errors, some metadata components need to be ignored during **build** (especially for unlocked packages) while the same being required for other commands like [validat](../../command-guide/advanced/validate.md)[e ](../../command-guide/advanced/validate.md). sfp offer you an easy mechanism which allows to switch .forceignore files depending on the operation.

Add this entry to your sfdx-project.json and as in the example below, mention the path to different files that need to be used for different stages

<pre><code> {
  "packageDirectories": [
    {
      "path": "core",
      "package": "core-package",
      "versionName": "Core 1.0",
      "versionNumber": "1.0.0.NEXT",
      "default": true,
    }
  ],
  "plugins": {
        "sfpowerscripts": {
            "ignoreFiles": {
               <a data-footnote-ref href="#user-content-fn-1"> "build": "forceignores/.buildignore"</a>
                "validate": ".forceignore"
            }
   }
 }
</code></pre>

[^1]: build now uses .buildignore file from forcrceignores directory

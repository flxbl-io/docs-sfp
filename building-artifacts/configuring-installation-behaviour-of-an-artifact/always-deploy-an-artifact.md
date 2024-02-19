# Always deploy an artifact



| Attribute    | Type    | Description                                                                                                                                             | Package Types Applicable                                                               |
| ------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| alwaysDeploy | boolean | Deploys package, even if it's installed already in the org. The artifact has to be present in the artifact directory for this particular option to work | <ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul> |



To ensure that an artifact is always deployed, irrespective the same version of the artifact is previously deployed to the org, you can utlize  **`alwaysDeploy`** as a property added to your package,&#x20;



<pre><code>{
  "packageDirectories": [
    {
      "path": "src-env-specific-pre",
      "package": "env-specific-pre",
      "versionDescription": "Environment related settings",
      "versionNumber": "4.7.0.NEXT",
      <a data-footnote-ref href="#user-content-fn-1">"alwaysDeploy":true</a>
    },
     ...
   ]
}
</code></pre>



[^1]: add alwaysDeploy to your package descriptors

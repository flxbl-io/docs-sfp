# Org-Dependent Unlocked Packages

Org-dependent unlocked packages, a variation of unlocked packages, allow you to create packages that depend on unpackaged metadata in the target org. Org dependent package is very useful in the context of  orgs that have lots of metadata and is struggling with understanding the dependency while building a '[unlocked package](unlocked-packages.md)'

Org dependent packages significantly enhance the efficiency of #flxbl projects who are already on scratch org based development. By allowing installation on a clean slate, these packages validate dependencies upfront, thereby reducing the additional validation time often required by unlocked packages.

## Org-Dependent Unlocked Package and Test Coverage

Org-dependent unlocked packages bypass the test coverage requirements, enabling installation in production without standard validation. This differs significantly from metadata deployments, where each Apex class deployed must meet a 75% coverage threshold or rely on the org's overall test coverage. While beneficial for large, established orgs, this approach should be used cautiously.

To address this, sfpowerscripts incorporates a default test coverage validation for org-dependent unlocked packages during the validation process. To disable this test coverage check during validation, additional attributes must be added to the package directory in the `sfdx-project.json` file.



<pre><code>// Org dependent unlocked package with additonal attributes

  "packageDirectories": [
    {
      "path": "src/core/core-crm",
      "package": "core-crm",
      "versionNumber": "4.7.0.NEXT",
      <a data-footnote-ref href="#user-content-fn-1">"skipTesting": true</a>,
      "<a data-footnote-ref href="#user-content-fn-2">skipCoverageValidation</a>": true
    },
     ...
   ]
}
</code></pre>

\
\




[^1]: Add a skip testing attribute to a package to skip testing while using sfp validate

[^2]: Add skipCoverage validation attribute to skip coverage validation of a package during exectuion of validate comand

# Pre/Post Deployment Script

<table><thead><tr><th width="229">Attribute</th><th>Type</th><th>Description</th><th>Package Types Applicable</th></tr></thead><tbody><tr><td>preDeploymentScript</td><td>string</td><td>Run an executable script before deploying an artifact. Users need to provide a path to the script file</td><td><p></p><ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul><p></p></td></tr><tr><td>postDeploymentScript</td><td>string</td><td>Run an executable script after deploying an package. Users need to provide a path to the script file</td><td><p></p><ul><li>unlocked </li><li>org-dependent unlocked</li><li>source</li><li>diff</li></ul></td></tr></tbody></table>

\
\
In some situations, you might need to execute a pre/post deployment script to do manipulate the data before or after being deployed to the org. **sfp** allow you to provide a path to a shell script (Mac/Unix) / batch script (on Windows).

The scripts are called with the following parameters. In your script you can refer to the parameters using [positional parameters.](https://linuxcommand.org/lc3\_wss0120.php)

Please note scripts are copied into the [artifacts](broken-reference) and are not executed from version control. sfpowerscripts only copies the script mentioned by this parameter and do not copy any additional files or dependencies. Please ensure pre/post deployment scripts are independent or should be able to download its dependencies

| Position | Value                                                                                                                               |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1        | Name of the Package                                                                                                                 |
| 2        | Username of the target org where the package is being deployed                                                                      |
| 3        | Alias of the target org where the package is being deployed                                                                         |
| 4        | Path to the working directory that has the contents of the package                                                                  |
| 5        | Path to the package directory. One would need to combine parameter 4 and 5 to find the absolute path to the contents of the package |







<pre><code>// Sample package 

{
  "packageDirectories": [
      {    
      "path": "src/data-package-cl",
      "package": "data-package-cloudlending",
      "type": "data",
      "versionNumber": "2.0.10.NEXT",
      "<a data-footnote-ref href="#user-content-fn-1">preDeploymentScript</a>": "scripts/enableEmailDeliverability.sh"
<strong>      "<a data-footnote-ref href="#user-content-fn-2">postDeploymentScript</a>": "scripts/pushData.sh"
</strong>    }

</code></pre>





{% hint style="warning" %}
Please note the script has to be completely independent and should not have dependency on a file in the version control, as scripts are executed within the context of an artifact.
{% endhint %}



```
// A script to enable email deliverablity to All

#!/bin/bash

export SF_DISABLE_DNS_CHECK=true

# Create a temporary file
temp_file="$(mktemp)"

# Write the anonymous Apex code to the temp file
cat << EOT >> "$temp_file"
{
  "$schema": "https://raw.githubusercontent.com/amtrack/sfdx-browserforce-plugin/master/src/plugins/schema.json",
  "settings": {
    "emailDeliverability": {
      "accessLevel": "All email"
    }
  }
}
EOT


if [ "$3" = 'ci' ]; then
# Execute the browserforce configuration to this org
  sf browserforce:apply  -f "$temp_file" --target-org $3
fi

# Clean up by removing the temporary file
rm "$temp_file"
```



[^1]: Use this attribute to trigger a pre deployment script



[^2]: Attribute to enable postDeploymentScript


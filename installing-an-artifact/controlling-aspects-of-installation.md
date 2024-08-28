# Controlling Aspects of Installation

### Skip artifacts if they are already installed

<pre><code>// Command to deploy set of artifacts to devhub
sfp install -u devhub --artifactdir artifacts --<a data-footnote-ref href="#user-content-fn-1">skipifalreadyinstalled</a>          
</code></pre>

By using the `skipifalreadyinstalled` option with the deploy command, you can prevent the reinstallation of an artifact that is already present in the target organization.

### **Install artifacts to an org baselined on an another org**



<pre><code>// Command to deploy with baseline
sfp install -u qa \
           --artifactdir artifacts \
           --skipifalreadyinstalled --<a data-footnote-ref href="#user-content-fn-2">baselineorg</a> devhub
</code></pre>

\
The `--baselineorg` parameter allows you to specify the alias or username of an org against which to check whether the incoming package versions have already been installed and form a deployment plan.This overrides the default behaviour which is to compare against the deployment target org. This is an optional feature which allows to ensure each org's are updated with the same installation across every org's in the path to production.



[^1]: Add this flag to skip the installation of an artifact if its already installed

[^2]: Add baseline org parameter to use the behaviour of the devhub


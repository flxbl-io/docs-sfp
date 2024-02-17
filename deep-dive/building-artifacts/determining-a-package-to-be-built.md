# Determining a package to be built

sfp's build commands process the packages in  the order as mentioned in your  sfdx-project.json. The commands also read your dependencies property, and then when triggered, will wait till all its dependencies are resolved, before triggering the equivalent package creation command depending on the type of the package.

sfp's build command  is  equipped with a diffcheck functionality, which is enabled when one utilizes **diffcheck** flag, A comparison (using git diff) is made between the latest source code and the previous version of the package published by the '[publish](../publish-artifact.md)' command. If any difference is detected in the **package directory**, **package version** or **scratch org definition file** (applies to unlocked packages only), then the package will be created - otherwise, it is skipped.\
\
&#x20;For eg: provided the followings packages in sfdx-project.json along with its dependencies

**Scenario 1 : Build All**



<figure><img src="../../.gitbook/assets/build-all-packages.png" alt=""><figcaption></figcaption></figure>

1. Trigger creation of package A
2. Once A is completed, trigger creation of package B & C \*\*,\*\*using the version of A, created in step 1
3. Once C is completed, trigger creation of package

**Scenario 2 : Build with diffCheck enabled on a package with no dependencies**

<figure><img src="../../.gitbook/assets/build-with-diffcheck-nodeps.png" alt=""><figcaption></figcaption></figure>

In this scenario, where only a single package has changed and **diffCheck** is enabled, the build command will only trigger the creation of Package B

1. Trigger creation of package A
2. Once A is completed, trigger creation of package B & C \*\*,\*\*using the version of A, created in step 1
3. Once C is completed, trigger creation of package

**Scenario 3 : Build with diffCheck enabled on changes in multiple packages**



<figure><img src="../../.gitbook/assets/build-with-diff-check-multiple-changes.png" alt=""><figcaption></figcaption></figure>

In this scenario, where there are changes in multiple packages, say B & C, the build command will trigger these packages in parallel, as their dependent package A has not changed (hence fulfilled). Please note even though there is a change in C, package D will not be triggered, unless there is an explicit version change of version number (major.minor.patch) of package D

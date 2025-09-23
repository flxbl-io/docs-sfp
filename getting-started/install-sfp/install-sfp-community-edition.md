# Install sfp community edition

##

### A. Install sfp in your local machine

```
npm i -g @flxbl-io/sfp
```

### B. Check sfp version

```
sfp --version
@flxbl-io/sfp/37.0.0 darwin-arm64 node-v20.3.1
```

{% hint style="warning" %}
sfp requires node-gyp for its dependencies. If you are facing issues during installation, with node-gyp, please follow the instructions here [https://www.npmjs.com/package/node-gyp](https://www.npmjs.com/package/node-gyp)
{% endhint %}

### C. Validate Installation

```
sfp commands

Command                         Summary
 ─────────────────────────────── ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 apextests trigger               Triggers Apex unit test in an org. Supports test level RunAllTestsInPackage, which optionally allows validation of individual class code coverage
 artifacts fetch                 Fetch sfp artifacts from a NPM compatible registry using a release definition file
 artifacts promote               Promotes artifacts predominantly for unlocked packages with code coverage greater than 75%
 artifacts query                 Fetch details about artifacts installed in a target org
 build                           Build artifact(s) of your packages in the current project
 ...
```

##

---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/building-artifacts/overview
---

# Overview

sfp cli features an intiutive command to build artifacts of all your packages in your project directory. The 'build' command automatically detects the type of package and builds an artifact individually for each package

```
sfp build -v <devhub_name> --branch <value> 
```

By default, the behaviour of sfp's build command is to build a new version of all packages in your project directory and and creates it associated artifact.

sfp's build command is also equipped with an ability to selectively build only packages that are changed. Read more on how sfp determines a package to be built on the subsequent sections.

```
sfp build -v <devhub_name> --branch <value>  --diffcheck
```

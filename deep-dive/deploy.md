# Deploy

## Skipping installation of an already installed package

This functionality only works provided, the target org has sfpowerscripts-artifact' (04t1P000000ka9mQAA) package installed. You need to install the package to every target org (including your production environment). The command for installing this package is as follows

```
sf package install --package 04t1P000000ka9mQAA -o <org> -w 10
```

If your prefer to install a package from your own DevHub rather than this package, you could do by building a package from the source provided at the [URL](https://github.com/Accenture/sfpowerscripts/tree/develop/prerequisites/sfpowerscripts-artifact). Once this package is built, you can instruct sfp cli to use this package by passing in the the environment variable SFPOWERSCRIPTS\_ARTIFACT\_UNLOCKED\_PACKAGE

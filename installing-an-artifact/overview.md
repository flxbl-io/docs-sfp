# Overview

Given a directory of artifacts and a target org, the **deploy** command will deploy the artifacts to the target org according to the sequence defined in the project configuration file.

```
// Command to install set of artifacts to devhub
sfp install -u devhub --artifactdir artifacts
```

### Sequence of Activities

The deploy command runs through the following steps

* Reads all the sfp artifacts provided through the artifact directory
* Unzips the artifacts and finds the latest sfdx-project.json.ori to determine the deployment order, if this particular file is not found,  it utilizes sfdx-project.json on the repo
* Read the  installed packages in the target org utilizing the records in SfpowerscriptsArtifacts2\_\_c
* Install each artifact  from the provided artifact directory to the target org based on the deployment order  respecting the attributes configured for each package


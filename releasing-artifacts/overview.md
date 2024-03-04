# Overview

sfp provides two commands to install artifacts into a target org, **sfp install** and **sfp release**. While **sfp install** installs a set of artifacts from a provided directory into a target org, sfp release allows you to install a defined collection of artifacts from an artifact registry directly into a target org, providing you with the consistency required in a pipeline.\
\
sfp release is a combination of  predominantly fetch and install commands, and the sequence can be explained with the below image

<figure><img src="../.gitbook/assets/sfp release.png" alt=""><figcaption><p>Sequence of steps undertaken by release command</p></figcaption></figure>

The release command is invoked by using the command sfp release with the inputs as shown below

```
  sfp release -p path/to/releasedefinition.yml \
              -u myorg --npm --scope myscope \
              --generatechangelog
```

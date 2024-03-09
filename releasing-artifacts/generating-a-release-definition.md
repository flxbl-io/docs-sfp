# Generating a release definition

In a high velocity project operating on a trunk such as a #flxbl project  and with substantial number of packages, manually generating release definition can be a chore. This can eased by using generating the release definition file automatically after every publish command. \
\
One can utilise [release config ](../configuring-a-project/release-config.md)along with release definition generate command to automate the process of generating release definitions.&#x20;

```

 sfp releasedefinition:generate -b releasedefns  \
                                -c  main  \
                                -d releasedefns_directory \
                                -f  ${{inputs.releaseconfig}} \
                                -n ${{env.RELEASE_NAME}}
                                
```

The above command will generate a release definition file based on the head of the branch 'main', one can also provide a commit ref ( by providing a commit id to the  `-c` or `--gitref`  flag  ) to utilize the latest artifacts on the particular commit. The generated release definition is then written to a directory called **releasedefns\_directory** and  pushed to a branch called **releasedefns**

One can utlilze the `--nopush` flag, if the intent is only to create a releasedefinition locally and do not push the same to the git repository


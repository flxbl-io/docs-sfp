# Troubleshooting Unlocked Packages Build Failure Due to Code Coverage

This page provides information on a common issue encountered when building unlocked packages in Salesforce that do not contain any Apex code but still fail due to code coverage requirements.

## Possible Causes and Solutions

1. **`unpackagedMetadata` or `apexTestAccess` properties**: Check if you are using the `unpackagedMetadata` or `apexTestAccess` properties on the package in `sfdx-project.json`. In this case, these properties were not used.
2. **Snapshot build org**: If you are using a snapshot build org and have deployed Apex classes to the initial scratch org that you made a snapshot of, then Salesforce will execute Apex tests (`RunAllLocalTests`). However, Salesforce probably wouldn't run the apex tests at all if there were none in the package you were verifying.
3. **Creating a class and testing it**: As a workaround, you can create a class and test it. This seems to be a bug, but it's not confirmed.


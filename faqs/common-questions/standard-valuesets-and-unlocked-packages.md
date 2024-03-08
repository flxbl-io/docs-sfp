# Standard ValueSets and unlocked packages

When integrating record types and standard value sets in Salesforce, particularly in the context of unlocked packages, teams may encounter issues with picklist values not being correctly associated with their respective record types upon installation. This article provides a solution to ensure a smooth deployment process that maintains the integrity of picklist assignments within record types.

**Problem Statement:** In situations where standard value sets are stored in an unpackaged directory and associated with record types in an unlocked package, the expected picklist values may not be properly applied upon installation. This results in all possible picklist options being displayed, contrary to the intended configuration.

**Observations:**

* This issue does not affect all record types within a package, as some may correctly reflect the selected picklist values.
* Manually setting standard picklist fields and referencing them in the record type does not prevent the issue; upon installation, all values become visible if not explicitly defined.
* Removing the reference from the record type within the package causes Salesforce to default to displaying all picklist values.

**Solution:** The sequence of deployment plays a crucial role in addressing this challenge. It is essential to deploy standard value sets prior to installing the unlocked package. To facilitate this, a preliminary package (referred to as `env-specific-pre`) containing the necessary components can be created. This package should include the standard value sets, ensuring they are in place before the main unlocked package is introduced to the org.

**Implementation Steps:**

1. Create a preliminary package named `env-specific-pre` or a similar identifiable name.
2. Include the standard value sets in this package, ideally in a dedicated directory (e.g., `src-env-specific-pre/standardValueSets`).
3. Ensure that this package is deployed to the Salesforce org before the installation of the main unlocked package.
4. Modify the `seedMetadata` configuration in the unlocked packages to reference the path of the standard value sets, as shown below:

```json
"seedMetadata": {
    "path": "src-env-specific-pre/standardValueSets"
}
```


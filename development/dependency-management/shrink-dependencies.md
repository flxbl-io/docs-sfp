# Shrink Dependencies

|              | sfp-pro | sfp (community) |
| ------------ | ------- | --------------- |
| Availability | ✅       | ✅               |

The `sfp dependency:shrink` command optimizes your project's dependency declarations by removing redundant transitive dependencies from your `sfdx-project.json`. This results in a cleaner project configuration with only the necessary direct dependencies declared for each package.

#### Usage

```bash
# Create a shrunk version of the project configuration
sfp dependency:shrink

# Overwrite the existing sfdx-project.json with shrunk dependencies
sfp dependency:shrink --overwrite
```

#### Flags

| Flag                           | Description                                                                  | Required |
| ------------------------------ | ---------------------------------------------------------------------------- | -------- |
| `-o`, `--overwrite`            | Overwrites the existing sfdx-project.json file with the shrunk configuration | No       |
| `-v`, `--targetdevhubusername` | Username or alias of the target Dev Hub org                                  | Yes      |
| `--loglevel`                   | Logging level (trace, debug, info, warn, error, fatal)                       | No       |

#### Behavior

* Without `--overwrite`, creates a new file at `./project-config/sfdx-project.min.json`
* With `--overwrite`, backs up existing `sfdx-project.json` to `./project-config/sfdx-project.json.bak` and overwrites the original
* Removes transitive dependencies that are already covered by direct dependencies
* Preserves external dependency mappings

### External Dependencies Configuration

External dependencies can be configured in your `sfdx-project.json` file using the `plugins.sfp` section. This is particularly useful for managed packages or packages from other Dev Hubs that your project depends on.

#### Configuration Format

```json
{
  "packageDirectories": [...],
  "plugins": {
    "sfp": {
      "externalDependencyMap": {
        "package-name": [
          {
            "package": "04tXXXXXXXXXXXXXXX",
            "versionNumber": "1.0.0.LATEST"
          }
        ]
      }
    }
  }
}
```

#### Example

```json
{
  "plugins": {
    "sfp": {
      "externalDependencyMap": {
        "trigger-framework": [
          {
            "package": "0H1000XRTCam",
            "versionNumber": "1.0.3.LATEST"
          }
        ]
      }
    }
  }
}
```

#### Notes

* External dependencies must be defined with their 04t IDs (subscriber package version IDs)
* The `versionNumber` can use `.LATEST` to automatically use the latest version that matches the major.minor.patch pattern
* External dependencies are preserved during both shrink and expand operations
* These dependencies are automatically included when calculating transitive dependencies

# Explain Dependencies

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ✅               |
| From         | November 24 | December 24     |

The `sfp dependency:explain` command helps you understand the dependencies between packages in your project. It can analyze either a specific package's dependencies or all package dependencies in the project, showing both direct and transitive dependencies.

#### Usage

```bash
# Explain dependencies for all packages
sfp dependency:explain

# Explain dependencies for a specific package
sfp dependency:explain -p <package-name>
```

#### Flags

| Flag              | Description                                            | Required |
| ----------------- | ------------------------------------------------------ | -------- |
| `-p`, `--package` | Name of the package to analyze dependencies for        | No       |
| `--json`          | Format output as JSON                                  | No       |
| `--loglevel`      | Logging level (trace, debug, info, warn, error, fatal) | No       |

## Understanding the Output

When analyzing dependencies, the command provides information about:

* Direct dependencies: Dependencies explicitly declared in the package's configuration
* Transitive dependencies: Dependencies that are required by your direct dependencies
* For transitive dependencies, the command shows which packages contribute to requiring that dependency

#### JSON Output Structure

When using the `--json` flag, the command returns data in the following structure:

```json
{
  "packages": [
    {
      "name": "package-name",
      "dependencies": [
        {
          "name": "dependency-name",
          "version": "version-number",
          "type": "direct|transitive",
          "contributors": ["package-names"] // Only present for transitive dependencies
        }
      ]
    }
  ]
}
```

#### Examples

Analyze all package dependencies:

```bash
sfp dependency:explain
```

Analyze dependencies for a specific package:

```bash
sfp dependency:explain -p my-package
```

Get dependencies in JSON format:

```bash
sfp dependency:explain -p my-package --json
```

#### Notes

* The command requires a valid SFDX project configuration
* Dependencies are resolved based on the information in your `sfdx-project.json` file
* Transitive dependency resolution helps identify indirect dependencies that might not be immediately obvious from the project configuration

# Expand Dependencies

|              | sfp-pro | sfp (community) |
| ------------ | ------- | --------------- |
| Availability | ✅       | ✅               |

The `sfp dependency:expand` command enriches your project's dependency declarations by automatically adding all transitive dependencies to your `sfdx-project.json`. This ensures that each package explicitly declares all its dependencies, both direct and indirect, making the dependency graph complete and explicit.

### What It Does

When you run `sfp dependency:expand`, it:

1. **Analyzes all packages** in your project to identify their direct dependencies
2. **Resolves transitive dependencies** by recursively finding dependencies of dependencies
3. **Updates each package** to include both direct and transitive dependencies
4. **Maintains proper ordering** ensuring dependencies are listed in topological order
5. **Preserves external dependencies** defined in your project configuration

### Why Use Expand

Expanding dependencies is useful for:

* **Explicit dependency management**: Makes all dependencies visible in the project configuration
* **Build optimization**: Helps build tools understand the complete dependency graph
* **Troubleshooting**: Easier to identify dependency-related issues when all dependencies are explicit
* **CI/CD pipelines**: Ensures all required dependencies are known upfront

### Usage

```bash
# Create an expanded version of the project configuration
sfp dependency:expand

# Overwrite the existing sfdx-project.json with expanded dependencies
sfp dependency:expand --overwrite
```

### Flags

| Flag                           | Description                                                                   | Required |
| ------------------------------ | ----------------------------------------------------------------------------- | -------- |
| `-o`, `--overwrite`            | Overwrites the existing sfdx-project.json file with the expanded configuration | No       |
| `-v`, `--targetdevhubusername` | Username or alias of the target Dev Hub org                                   | Yes      |
| `--loglevel`                   | Logging level (trace, debug, info, warn, error, fatal)                        | No       |

### Behavior

* Without `--overwrite`, creates a new file at `./project-config/sfdx-project.exp.json`
* With `--overwrite`, backs up existing `sfdx-project.json` to `./project-config/sfdx-project.json.bak` and overwrites the original
* Adds all transitive dependencies to each package's dependency list
* Maintains topological ordering of dependencies
* Handles version conflicts by selecting the highest version required

### Example: Before and After

#### Before Expansion

```json
{
  "packageDirectories": [
    {
      "path": "packages/feature-a",
      "package": "feature-a",
      "dependencies": [
        { "package": "core-package" }
      ]
    },
    {
      "path": "packages/feature-b",
      "package": "feature-b",
      "dependencies": [
        { "package": "feature-a" }
      ]
    },
    {
      "path": "packages/core-package",
      "package": "core-package",
      "dependencies": [
        { "package": "base-package" }
      ]
    },
    {
      "path": "packages/base-package",
      "package": "base-package",
      "dependencies": []
    }
  ]
}
```

#### After Expansion

```json
{
  "packageDirectories": [
    {
      "path": "packages/feature-a",
      "package": "feature-a",
      "dependencies": [
        { "package": "base-package" },     // Transitive dependency added
        { "package": "core-package" }       // Direct dependency
      ]
    },
    {
      "path": "packages/feature-b",
      "package": "feature-b",
      "dependencies": [
        { "package": "base-package" },     // Transitive dependency added
        { "package": "core-package" },     // Transitive dependency added
        { "package": "feature-a" }         // Direct dependency
      ]
    },
    {
      "path": "packages/core-package",
      "package": "core-package",
      "dependencies": [
        { "package": "base-package" }      // Direct dependency
      ]
    },
    {
      "path": "packages/base-package",
      "package": "base-package",
      "dependencies": []
    }
  ]
}
```

### Relationship with Shrink

The `expand` and `shrink` commands are complementary:

* **Expand**: Adds all transitive dependencies, making them explicit
* **Shrink**: Removes redundant transitive dependencies, keeping only direct ones

Typical workflow:
1. Use `expand` during development to understand full dependency graphs
2. Use `shrink` before committing to maintain clean, minimal dependency declarations

### Version Conflict Resolution

When multiple packages require different versions of the same dependency, `expand` automatically selects the highest version that satisfies all requirements. For example:

* Package A requires `core@1.0.0`
* Package B requires `core@1.2.0`
* Result: Both will get `core@1.2.0`

### External Dependencies

External dependencies (managed packages from other Dev Hubs) defined in your `externalDependencyMap` are:
* Automatically included in the expansion process
* Preserved with their version specifications
* Added to packages that transitively depend on them

### Notes

* The command validates all dependencies exist in the project
* Circular dependencies are detected and reported as errors
* External dependencies must be properly configured in the `plugins.sfp.externalDependencyMap` section
* The expanded configuration can become large for projects with many packages

### See Also

* [Shrink Dependencies](shrink-dependencies.md) - Remove redundant transitive dependencies
* [Explain Dependencies](explain-dependencies.md) - Understand package dependencies
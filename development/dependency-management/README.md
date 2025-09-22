# Dependency Management

sfp provides powerful commands to manage and understand package dependencies in your Salesforce projects. These tools help you maintain clean dependency declarations, troubleshoot dependency issues, and optimize your build processes.

### Available Commands

| Command | Description |
|---------|-------------|
| [`sfp dependency:expand`](expand-dependencies.md) | Add all transitive dependencies to make them explicit |
| [`sfp dependency:shrink`](shrink-dependencies.md) | Remove redundant transitive dependencies for cleaner configuration |
| [`sfp dependency:explain`](explain-dependencies.md) | Analyze and understand package dependencies |

### Understanding Dependencies

In Salesforce DX projects, packages can depend on other packages. These dependencies come in two forms:

* **Direct Dependencies**: Dependencies explicitly declared in a package's configuration
* **Transitive Dependencies**: Dependencies of your dependencies (indirect dependencies)

For example, if Package A depends on Package B, and Package B depends on Package C, then:
- Package A has a direct dependency on Package B
- Package A has a transitive dependency on Package C

### Dependency Management Workflow

A typical dependency management workflow involves:

1. **Development Phase**: Use `sfp dependency:expand` to make all dependencies explicit during development, helping identify potential issues early

2. **Analysis**: Use `sfp dependency:explain` to understand dependency relationships and identify unnecessary dependencies

3. **Cleanup**: Use `sfp dependency:shrink` before committing to maintain minimal, clean dependency declarations

4. **Build Optimization**: Expanded dependencies help build tools understand the complete dependency graph for optimized builds

### External Dependencies

External dependencies are packages from outside your project, typically:
- Managed packages from AppExchange
- Packages from other Dev Hub organizations
- Third-party components

These are configured in your `sfdx-project.json` under `plugins.sfp.externalDependencyMap`:

```json
{
  "plugins": {
    "sfp": {
      "externalDependencyMap": {
        "external-package-name": [
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

### Best Practices

1. **Keep dependencies minimal**: Only declare direct dependencies in your source control
2. **Use expand for analysis**: Temporarily expand dependencies to understand the full graph
3. **Validate regularly**: Run dependency commands in CI/CD to catch issues early
4. **Document external dependencies**: Clearly document why each external dependency is needed
5. **Version carefully**: Use specific versions for production, `.LATEST` for development

### Common Issues and Solutions

#### Circular Dependencies
If packages depend on each other in a circular manner, the dependency commands will report an error. Refactor your packages to break the circular dependency.

#### Missing Dependencies
If a package references components from another package without declaring the dependency, add the missing dependency to the package's configuration.

#### Version Conflicts
When different packages require different versions of the same dependency, sfp will use the highest compatible version. Consider standardizing versions across your project.

### See Also

* [Package Dependencies](../../concepts/dependency-management.md) - Conceptual overview of dependencies
* [Transitive Dependency Resolution](../../concepts/transitive-dependency-resolution.md) - How sfp resolves dependencies
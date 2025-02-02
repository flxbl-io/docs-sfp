---
icon: ring-diamond
---

# Duplicate Check

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | ❌               |
| From         | January 25 |                 |

\
The duplicate check functionality helps identify duplicate metadata components across your Salesforce project. This analysis is crucial for maintaining clean code organization and preventing conflicts in your deployment process.

### Overview

Duplicate checking scans your project's metadata components and identifies:

* Components that exist in multiple packages
* Components in aliasified packages
* Components in unclaimed directories (not associated with a package)

### How It Works

The duplicate checker:

1. Scans all specified directories for metadata components
2. Creates a unique identifier for each component based on type and name
3. Identifies components that appear in multiple locations
4. Provides detailed reporting with context about each duplicate

### Configuration

Duplicate checking can be configured through several flags in the project:analyze command:

```bash
sfp project:analyze --fail-on duplicates --fail-on-unclaimed
```

#### Key Configuration Options

| Option                 | Description                                    | Default |
| ---------------------- | ---------------------------------------------- | ------- |
| `--show-aliasfy-notes` | Show information about aliasified packages     | true    |
| `--fail-on-unclaimed`  | Fail when duplicates are in unclaimed packages | false   |

### Understanding Results

The duplicate check provides three types of findings:

1. **Direct Duplicates**: Same component in multiple packages
2. **Aliasified Components**: Components in aliasified packages (typically expected)
3. **Unclaimed Components**: Components in directories (such as unpacakged or src/temp) not associated with packages

#### Sample Output

```markdown
# Duplicate Analysis Report

## Aliasified Package Information
- Note: Package "env-specific" is aliasified - duplicates are allowed.

## Component Analysis

### ❌ CustomObject: Account.Custom_Field__c (Duplicate Component)
- `src/package1/objects/Account.object` 
- `src/package2/objects/Account.object`

### ⚠️ CustomLabel: Common_Label (Aliasified Component)
- `src/env-specific/qa/labels/Custom.labels` (aliasified)
- `src/env-specific/prod/labels/Custom.labels` (aliasified)
```

### Understanding Indicators

The analysis uses several indicators to help you understand the results:

* ❌ Indicates a problematic duplicate that needs attention
* ⚠️ Indicates a duplicate that might be intentional (e.g., in aliasified packages)
* _(aliasified)_ Marks components in aliasified packages
* _(unclaimed)_ Marks components in unclaimed directories

### Best Practices

1. **Regular Scanning**: Run duplicate checks regularly during development
2. **Clean Package Structure**: Keep each component in its appropriate package
3. **Proper Package Configuration**: Ensure all directories are properly claimed in sfdx-project.json
4. **Documented Exceptions**: Document cases where duplication is intentional

### Common Scenarios and Solutions

#### 1. Legitimate Duplicates

Some components may need to exist in multiple packages. In these cases:

* Use aliasified packages when appropriate
* Document the reason for duplication
* Consider creating a shared package for common components

#### 2. Unclaimed Directories

If you find components in unclaimed directories:

1. Add the directory to sfdx-project.json
2. Assign it to an appropriate package
3. Re-run the analysis to verify the fix

#### 3. Aliasified Package Duplicates

Duplicates in aliasified packages are often intentional:

* Used for environment-specific configurations
* Different versions of components for different contexts
* Not considered errors by default

### Integration with CI/CD

{% hint style="info" %}
Integration is limited only to GitHub at the monent, The command needs GITHUB\_APP\_PRIVATE\_KEY and  GITHUB\_APP\_ID to be set in an env variable for the results to be reported as a GitHub check
{% endhint %}

When integrating duplicate checking in your CI/CD pipeline:

1.  **Configure Failure Conditions**:

    ```bash
    sfp project:analyze --fail-on duplicates --fail-on-unclaimed --output-format github
    ```
2.  **Generate Reports**:

    ```bash
    sfp project:analyze --report-dir ./reports --output-format markdown
    ```
3. **Review Results**:
   * Use generated reports for tracking
   * Address critical duplicates
   * Document accepted duplicates

### Troubleshooting

1. **False Positives**
   * Verify package configurations in sfdx-project.json
   * Check if components should be in aliasified packages
   * Ensure directories are properly claimed
2. **Missing Components**
   * Verify scan scope (package, domain, or path)
   * Check file permissions
   * Validate metadata format


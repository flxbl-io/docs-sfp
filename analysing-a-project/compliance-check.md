---
icon: ring-diamond
---

# Compliance Check

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | ❌               |
| From         | October 25 |                 |

\
The compliance check functionality ensures your Salesforce metadata adheres to organizational standards and best practices. This feature helps maintain code quality, security, and consistency across your Salesforce project by enforcing configurable rules.

### Overview

Compliance checking provides a comprehensive framework for:

* Enforcing coding standards and best practices
* Preventing security vulnerabilities like hardcoded IDs and URLs
* Maintaining API version consistency
* Validating metadata field values against organizational policies
* Generating detailed violation reports with remediation guidance

### How It Works

The compliance checker:

1. Loads rules from your configuration file (defaults to `config/compliance-rules.yaml`)
2. Scans metadata components using Salesforce's ComponentSet API
3. Applies rules based on metadata type and field specifications
4. Evaluates content and field values using configurable operators
5. Reports violations with file locations, severity levels, and helpful messages

### Configuration

Compliance checking is configured through YAML files that define rules and their enforcement:

```bash
sfp project:analyze --compliance-rules config/compliance-rules.yaml --fail-on compliance
```

#### Configuration File Structure

Create a compliance rules file using the generate command:

```bash
sfp project:analyze --generate-compliance-config
```

This creates `config/compliance-rules.yaml` with sample rules:

```yaml
# SFP Compliance Rules Configuration
extends: default
rules:
  - id: no-hardcoded-ids
    enabled: true
    _comment: Enable this rule to prevent hardcoded Salesforce IDs
  - id: no-hardcoded-urls
    enabled: false
    _comment: Enable this rule to prevent hardcoded Salesforce URLs
  - id: profile-no-modify-all
    enabled: false
    _comment: Enable this rule to prevent profiles with Modify All Data permission
  - id: custom-api-version
    name: Minimum API Version Check
    enabled: true
    metadata:
      - ApexClass
      - ApexTrigger
    field: ApexClass.apiVersion
    operator: greater_or_equal
    value: 59.0
    severity: warning
    message: Component should use API version 59.0 or higher
```

#### Built-in Rules

The system includes several built-in rules that can be enabled:

| Rule ID                | Description                                  | Metadata Types           | Default |
| ---------------------- | -------------------------------------------- | ------------------------ | ------- |
| `no-hardcoded-ids`     | Prevents hardcoded 15/18 character IDs      | ApexClass, ApexTrigger, Flow | Disabled |
| `no-hardcoded-urls`    | Prevents hardcoded Salesforce URLs          | ApexClass, ApexTrigger, Flow | Disabled |
| `profile-no-modify-all`| Prevents Modify All Data permission         | Profile                  | Disabled |

#### Custom Rules

Define custom rules to match your specific organizational requirements:

```yaml
rules:
  - id: minimum-api-version
    name: Enforce Minimum API Version
    description: All components must use API version 59.0 or higher
    enabled: true
    metadata:
      - ApexClass
      - ApexTrigger
    field: ApexClass.apiVersion
    operator: greater_or_equal
    value: 59.0
    severity: warning
    message: Component should use API version 59.0 or higher
```

#### Rule Configuration Options

| Field        | Description                              | Required | Values                           |
| ------------ | ---------------------------------------- | -------- | -------------------------------- |
| `id`         | Unique identifier for the rule           | Yes      | String                           |
| `name`       | Human-readable rule name                 | No       | String                           |
| `description`| Detailed rule description                | No       | String                           |
| `enabled`    | Whether the rule is active               | No       | true/false (default: false)     |
| `metadata`   | Metadata types to check                  | Yes      | Array of metadata type names     |
| `field`      | Field path to evaluate                   | Yes      | Dot-notation path or "_content"  |
| `operator`   | Comparison operator                      | Yes      | See operators table below        |
| `value`      | Expected value for comparison            | Yes      | String, Number, Boolean          |
| `severity`   | Violation severity level                 | Yes      | error, warning, info             |
| `message`    | Custom violation message                 | No       | String                           |

#### Supported Operators

| Operator           | Description                              | Example                    |
| ------------------ | ---------------------------------------- | -------------------------- |
| `equals`           | Field equals expected value              | `apiVersion` equals `59.0` |
| `not_equals`       | Field does not equal expected value      | `status` not_equals `Inactive` |
| `contains`         | Field contains substring                 | Content contains `hardcoded` |
| `not_contains`     | Field does not contain substring         | Content not_contains `TODO` |
| `greater_than`     | Numeric field is greater than value      | `apiVersion` > `58.0`      |
| `less_than`        | Numeric field is less than value         | `apiVersion` < `60.0`      |
| `greater_or_equal` | Numeric field is greater than or equal   | `apiVersion` >= `59.0`     |
| `less_or_equal`    | Numeric field is less than or equal      | `apiVersion` <= `59.0`     |
| `regex`            | Field matches regular expression         | Content matches `[a-zA-Z0-9]{15}` |

### Field Path Specifications

#### XML Field Paths

For XML metadata files, use dot notation to specify field paths:

```yaml
# For ApexClass metadata XML
field: ApexClass.apiVersion

# For nested fields
field: ApexClass.packageVersions.majorNumber

# For Profile permissions
field: Profile.userPermissions.ModifyAllData
```

#### Content Analysis

Use the special `_content` field to analyze file content:

```yaml
field: _content
operator: regex
value: '[a-zA-Z0-9]{15}|[a-zA-Z0-9]{18}'  # Salesforce ID pattern
```

### Understanding Results

The compliance check provides detailed violation reports with multiple output formats:

#### Console Output

```
📋 Compliance Check Results
═══════════════════════════

Rules Checked: 3/5

Found 5 violations:

❌ Errors (2):
  • src/classes/MyClass.cls-meta.xml:3
    Component should use API version 59.0 or higher
    Rule: Minimum API Version Check

⚠️  Warnings (3):
  • src/classes/Controller.cls:15
    Hardcoded Salesforce IDs found - use Custom Settings, Custom Metadata, or SOQL queries instead
    Rule: No Hardcoded Salesforce IDs
```

#### Violation Details

Each violation includes:

* **File Path**: Exact location of the violation
* **Line Number**: Specific line where the issue occurs (when applicable)
* **Rule Name**: Which rule was violated
* **Severity**: Error, Warning, or Info level
* **Message**: Descriptive explanation and remediation guidance
* **Actual Value**: The found value that triggered the violation
* **Expected Value**: What the rule expected to find

### Integration with CI/CD

{% hint style="info" %}
Integration is limited only to GitHub at the moment. The command needs GITHUB\_APP\_PRIVATE\_KEY and GITHUB\_APP\_ID to be set in environment variables for results to be reported as GitHub checks.
{% endhint %}

When integrating compliance checking in your CI/CD pipeline:

1. **Enforce Compliance Standards**:

   ```bash
   sfp project:analyze --compliance-rules config/compliance-rules.yaml --fail-on compliance
   ```

2. **Generate Reports**:

   ```bash
   sfp project:analyze --compliance-rules config/compliance-rules.yaml --report-dir ./reports --output-format markdown
   ```

3. **GitHub Actions Integration**:

   ```yaml
   - name: Run Compliance Check
     run: |
       sfp project:analyze --compliance-rules config/compliance-rules.yaml --fail-on compliance --output-format github
     env:
       GITHUB_APP_PRIVATE_KEY: ${{ secrets.GITHUB_APP_PRIVATE_KEY }}
       GITHUB_APP_ID: ${{ secrets.GITHUB_APP_ID }}
   ```

### Scoping Compliance Checks

Use the same scoping options as other analysis commands:

#### By Package

```bash
sfp project:analyze --compliance-rules config/compliance-rules.yaml -p core,utils
```

#### By Domain

```bash
sfp project:analyze --compliance-rules config/compliance-rules.yaml -d sales
```

#### By Source Path

```bash
sfp project:analyze --compliance-rules config/compliance-rules.yaml -s ./force-app/main/default
```

### Output Formats

The compliance checker supports multiple output formats:

* **Console**: Human-readable terminal output with color coding
* **Markdown**: Detailed reports suitable for documentation
* **JSON**: Machine-readable format for integration with other tools
* **GitHub**: Special format for GitHub Checks API integration

### Common Compliance Scenarios

#### API Version Management

Ensure all components use recent API versions:

```yaml
- id: minimum-api-version
  enabled: true
  metadata: [ApexClass, ApexTrigger]
  field: ApexClass.apiVersion
  operator: greater_or_equal
  value: 59.0
  severity: warning
```

#### Security Hardening

Prevent security vulnerabilities:

```yaml
- id: no-hardcoded-credentials
  enabled: true
  metadata: [ApexClass]
  field: _content
  operator: regex
  value: '(password|secret|key)\s*=\s*["\'][^"\']+["\']'
  severity: error
  message: Remove hardcoded credentials - use Named Credentials or Custom Settings
```

#### Profile Security

Restrict dangerous permissions:

```yaml
- id: no-modify-all-data
  enabled: true
  metadata: [Profile]
  field: Profile.userPermissions.ModifyAllData
  operator: equals
  value: true
  severity: error
  message: Profile should not have Modify All Data permission
```

### Logging and Debugging

Use different log levels to control output verbosity:

```bash
# Basic progress information
sfp project:analyze --compliance-rules config/compliance-rules.yaml --loglevel info

# Detailed debugging information
sfp project:analyze --compliance-rules config/compliance-rules.yaml --loglevel debug
```

Debug logging shows:
- Rules being processed
- Files being scanned
- Field extraction details
- Rule evaluation results

### Troubleshooting

#### Rule Not Triggering

1. **Verify Field Path**: Ensure the field path matches the XML structure
2. **Check Metadata Types**: Confirm the rule applies to the correct metadata types
3. **Validate Operators**: Ensure the operator logic matches your expectations
4. **Enable Debug Logging**: Use `--loglevel debug` to see detailed evaluation

#### False Positives

1. **Refine Rule Conditions**: Adjust operators or values to be more specific
2. **Scope Rules Appropriately**: Use metadata type filters to target specific components
3. **Create Exceptions**: Disable rules for specific packages or paths when needed

#### Performance Issues

1. **Scope Analysis**: Use package, domain, or path filters to reduce scope
2. **Optimize Rules**: Complex regex patterns can slow down content analysis
3. **Exclude Large Files**: Consider excluding generated or vendor files

### Configuration Examples

#### Organization Standards

```yaml
extends: default
rules:
  # Security Rules
  - id: no-hardcoded-ids
    enabled: true
  - id: no-hardcoded-urls
    enabled: true

  # API Version Standards
  - id: minimum-api-version
    name: Enforce API Version 59+
    enabled: true
    metadata: [ApexClass, ApexTrigger]
    field: ApexClass.apiVersion
    operator: greater_or_equal
    value: 59.0
    severity: warning

  # Profile Security
  - id: profile-no-modify-all
    enabled: true
    severity: error
```

#### Development Standards

```yaml
extends: default
rules:
  # Code Quality
  - id: no-system-debug
    name: Remove System.debug Statements
    enabled: true
    metadata: [ApexClass]
    field: _content
    operator: contains
    value: System.debug
    severity: warning
    message: Remove System.debug statements before production

  # Test Class Requirements
  - id: test-class-naming
    name: Test Class Naming Convention
    enabled: true
    metadata: [ApexClass]
    field: _content
    operator: regex
    value: '@IsTest.*class.*Test'
    severity: info
    message: Test classes should follow naming convention with 'Test' suffix
```
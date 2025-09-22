---
icon: ring-diamond
---

# Overview

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | ❌               |
| From         | January 25 |                 |

\
The project analysis command helps you analyze your Salesforce project for potential issues and provides detailed reports in various formats. This command is particularly useful for identifying issues such as duplicate components, compliance violations, hardcoded IDs and URLs, and other code quality concerns.

### Usage

```bash
sfp project:analyze [flags]
```

### Common Use Cases

The analyze command serves several key purposes:

1. Runs various available linters across the project&#x20;
2. Generating comprehensive analysis reports
3. Integration with CI/CD pipelines for automated checks

### Available Flags

| Flag                   | Description                                               | Required | Default  |
| ---------------------- | --------------------------------------------------------- | -------- | -------- |
| `--package, -p`        | The name of the package to analyze                        | No       | -        |
| `--domain, -d`         | The domain to analyze                                     | No       | -        |
| `--source-path, -s`    | The path to analyze                                       | No       | -        |
| `--exclude-linters`    | Comma-separated list of linters to exclude                | No       | \[]      |
| `--fail-on`            | Linters that should cause command failure if issues found | No       | \[]      |
| `--show-aliasfy-notes` | Show notes for aliasified packages                        | No       | true     |
| `--fail-on-unclaimed`  | Fail when duplicates are found in unclaimed packages      | No       | false    |
| `--output-format`      | Output format (markdown, json, github)                    | No       | markdown |
| `--report-dir`         | Directory for analysis reports                            | No       | -        |
| `--compliance-rules`   | Path to compliance rules YAML file                       | No       | config/compliance-rules.yaml |
| `--generate-compliance-config` | Generate sample compliance rules configuration    | No       | false    |

### Scoping Analysis

The command provides three mutually exclusive ways to scope your analysis:

1.  **By Package**: Analyze specific packages

    ```bash
    sfp project:analyze -p core,utils
    ```
2.  **By Domain**: Analyze all packages in a domain

    ```bash
    sfp project:analyze -d sales
    ```
3.  **By Source Path**: Analyze a specific directory

    ```bash
    sfp project:analyze -s ./force-app/main/default
    ```

### Output Formats

The command supports multiple output formats:

* **Markdown**: Human-readable documentation format
* **JSON**: Machine-readable format for integration with other tools
* **GitHub**: Special format for GitHub Checks API integration

### GitHub Integration

When running in GitHub Actions, the command automatically:

1. Creates GitHub Check runs for each analysis
2. Adds annotations to the code for identified issues
3. Provides detailed summaries in the GitHub UI

### Examples

1.  Basic analysis of all packages:

    ```bash
    sfp project:analyze
    ```
2.  Analyze specific packages with JSON output:

    ```bash
    sfp project:analyze -p core,utils --output-format json
    ```
3.  Analyze with strict validation:

    ```bash
    sfp project:analyze --fail-on duplicates --fail-on-unclaimed
    ```
4.  Generate reports in a specific directory:

    ```bash
    sfp project:analyze --report-dir ./analysis-reports
    ```
5.  Generate compliance configuration:

    ```bash
    sfp project:analyze --generate-compliance-config
    ```
6.  Run compliance checks with custom rules:

    ```bash
    sfp project:analyze --compliance-rules config/compliance-rules.yaml --fail-on compliance
    ```


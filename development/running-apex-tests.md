# Running Apex Tests

The `apextests trigger` command allows you to independently execute Apex tests in your Salesforce org. While the `validate` command automatically runs tests per package during validation, this command gives you direct control over test execution with support for multiple test levels, code coverage validation, and output formats.

## Primary Testing Patterns

sfp follows a **package-centric testing approach** where tests are organized and executed at the package or domain level, rather than running all org tests together. This aligns with how the `validate` command works and provides better isolation and faster feedback.

```bash
# Test a specific package (primary pattern)
sfp apextests trigger -o my-org -l RunAllTestsInPackage -n my-package

# Test all packages in a domain (recommended for domain validation)
sfp apextests trigger -o my-org -l RunAllTestsInDomain -r config/release-config.yaml

# Test multiple packages together
sfp apextests trigger -o my-org -l RunAllTestsInPackage -n package-a -n package-b

# Quick test during development
sfp apextests trigger -o my-org -l RunSpecifiedTests --specifiedtests MyTest
```

## Test Levels

### RunAllTestsInPackage (Recommended)

Runs all tests within specified package(s). This is the **primary testing pattern** in sfp and matches how the `validate` command executes tests. Supports code coverage validation at both package and individual class levels.

```bash
# Single package
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n sales-core

# Multiple packages
sfp apextests trigger -o dev-org -l RunAllTestsInPackage \
  -n sales-core \
  -n sales-ui \
  -n sales-integration
```

This pattern matches how `validate` command executes tests - each package is tested independently with its own test classes. This provides:
- Better test isolation and faster feedback
- Package-level code coverage validation
- Clear attribution of test failures to specific packages
- Parallel test execution per package (when enabled)

### RunAllTestsInDomain (Recommended for Domain Validation)

Runs tests for all packages defined in a domain from your release config. This is the **recommended pattern for validating entire domains** and matches how you would validate a domain for release.

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInDomain \
  -r config/release-config-sales.yaml
```

This executes tests for each package in the domain sequentially, providing comprehensive domain validation. Use this when:
- Validating changes across a domain before release
- Testing related packages together as a unit
- Performing end-to-end domain validation

### RunSpecifiedTests

Runs specific test classes or methods. Useful for rapid iteration during active development.

```bash
# Run specific test classes
sfp apextests trigger -o dev-org -l RunSpecifiedTests \
  --specifiedtests AccountTest,ContactTest

# Run specific test methods
sfp apextests trigger -o dev-org -l RunSpecifiedTests \
  --specifiedtests AccountTest.testCreate,ContactTest.testUpdate
```

Use during development for quick feedback cycles when working on specific features.

### RunApexTestSuite

Runs all tests in a test suite defined in your org.

```bash
sfp apextests trigger -o dev-org -l RunApexTestSuite \
  --apextestsuite QuickTests
```

Useful for running pre-defined test groups or smoke test suites.

### RunLocalTests

Runs all tests in your org except those from managed packages. This is the default test level in Salesforce but **not the recommended pattern in sfp**.

```bash
sfp apextests trigger -o dev-org -l RunLocalTests
```

**Note:** While this is the Salesforce default, sfp recommends package-level or domain-level testing for better isolation and faster feedback. Use this only when you specifically need to run all org tests together, such as for compliance requirements or full org validation.

### RunAllTestsInOrg

Runs all tests in your org, including managed packages. Rarely used due to long execution time.

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInOrg
```

Use only for complete org validation scenarios.

## Code Coverage Validation

### Individual Class Coverage

Validates that each Apex class in the package meets the minimum coverage threshold. Every class must meet or exceed the specified percentage.

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage \
  -n my-package \
  -c \
  -p 80
```

**Coverage threshold:**
- Default: 75%
- Adjustable with `-p` flag
- Applied per class, not as an average

**Output includes:**
- List of all classes with their coverage percentages
- Classes that meet the threshold
- Classes that fail to meet the threshold
- Overall package coverage percentage

### Package Coverage

Validates that the overall package coverage meets the minimum threshold. The average coverage across all classes must meet or exceed the specified percentage.

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage \
  -n my-package \
  --validatepackagecoverage \
  -p 75
```

**Coverage calculation:**
- Aggregates coverage across all classes in package
- Calculated as: (total covered lines / total lines) * 100
- Only classes with Apex code count toward coverage

### Coverage vs No Coverage

Running tests without coverage flags still executes tests but doesn't fetch or validate coverage data:

```bash
# Run tests without coverage data
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package

# Run tests with coverage fetching and validation
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -c
```

**Note:** Fetching coverage data adds time to test execution, so only use it when needed.

## Output Formats

> **Note:** The dashboard output format is a new feature introduced in the November 2025 release of sfp-pro.

### Raw Format (Default)

Standard Salesforce API output with JUnit XML and JSON results. This is the default format.

```bash
sfp apextests trigger -o dev-org -l RunLocalTests
```

**Generates:**
- `.testresults/test-result-<testRunId>.json` - Raw Salesforce test results
- `.testresults/test-result-<testRunId>-junit.xml` - JUnit XML format
- `.testresults/test-result-<testRunId>-coverage.json` - Coverage data (if coverage enabled)
- `.testresults/testresults.md` - Markdown summary

### Dashboard Format

> **Available in:** sfp-pro November 2024 release and later

Structured JSON format optimized for dashboards, metrics systems, and reporting tools. Unlike the raw Salesforce API output, the dashboard format provides enriched, pre-processed data that's ready for consumption by external systems.

```bash
# Generate dashboard format
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package \
  --outputformat dashboard \
  --environment dev
```

**Generates all raw format files plus:**
- `.testresults/<testRunId>/dashboard.json` - Structured test results
- `.testresults/<testRunId>/testresults.md` - Enhanced markdown summary
- `.testresults/latest.json` - Symlink to latest dashboard result

#### Dashboard JSON Schema

The `dashboard.json` file contains a comprehensive test execution report:

```json
{
  "environment": "dev",
  "timestamp": "2025-11-24T10:30:00.000Z",
  "duration": 125000,
  "testExecutionTime": 120000,
  "commandTime": 115000,
  "repository": "https://github.com/myorg/myrepo",
  "commitSha": "abc123def",
  "branch": "main",

  "summary": {
    "totalTests": 150,
    "passed": 145,
    "failed": 5,
    "skipped": 0,
    "passingRate": 96.67,
    "overallCoverage": 82.5,
    "coveredLines": 8250,
    "totalLines": 10000,
    "outcome": "Failed"
  },

  "coverage": {
    "overallCoverage": 82.5,
    "totalLines": 10000,
    "coveredLines": 8250,
    "classes": [
      {
        "name": "AccountService",
        "id": "01p...",
        "coverage": 95.5,
        "totalLines": 200,
        "coveredLines": 191,
        "status": "pass"
      }
    ],
    "uncoveredClasses": ["LegacyHelper"],
    "belowThreshold": [
      {
        "name": "OldProcessor",
        "coverage": 65.0,
        "threshold": 75
      }
    ]
  },

  "testCases": [
    {
      "id": "07M...",
      "name": "AccountService.testCreateAccount",
      "className": "AccountService",
      "methodName": "testCreateAccount",
      "time": 250,
      "status": "passed"
    }
  ],

  "topFailingTests": [
    {
      "name": "ContactTest.testValidation",
      "className": "ContactTest",
      "methodName": "testValidation",
      "failureMessage": "System.AssertException: Expected 5, but got 3"
    }
  ],

  "metadata": {
    "testRunId": "707...",
    "orgId": "00D...",
    "username": "dev@example.com",
    "package": "sales-core",
    "testLevel": "RunAllTestsInPackage"
  }
}
```

#### How Dashboard Output is Used

**For Metrics and Observability:**
```bash
# Run tests and extract metrics
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package \
  --outputformat dashboard --json > results.json

# Extract key metrics
PASSING_RATE=$(jq -r '.result.summary.passingRate' results.json)
COVERAGE=$(jq -r '.result.summary.overallCoverage' results.json)

# Push to your metrics backend
curl -X POST https://metrics.example.com/api/tests \
  -d @.testresults/latest.json
```

**For Test History Tracking:**
```bash
# The latest.json symlink always points to most recent result
jq -r '.summary.passingRate' .testresults/latest.json

# Track coverage trends
jq -r '.coverage.belowThreshold[] | "\(.name): \(.coverage)%"' \
  .testresults/latest.json
```

#### Dashboard vs Raw Format

| Feature | Raw Format | Dashboard Format |
|---------|-----------|------------------|
| **Output** | Salesforce API response | Processed, enriched data |
| **Structure** | Flat, verbose | Hierarchical, organized |
| **File Location** | `.testresults/` root | `.testresults/<testRunId>/` |
| **Coverage** | Separate file | Integrated in JSON |
| **Latest Symlink** | No | Yes (`latest.json`) |
| **Metadata** | Limited | Environment, repo, commit |
| **Use Case** | Salesforce tooling | External systems, dashboards |
| **Exit on Failure** | Yes (exit code 1) | No (exit code 0) |

#### Non-Blocking Dashboard Mode

In dashboard mode, test failures don't cause the command to exit with error code 1, allowing you to collect test results even when tests fail:

```bash
# Command succeeds even if tests fail
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package \
  --outputformat dashboard

echo $?  # Always 0 in dashboard mode
```

This is useful for:
- Collecting metrics regardless of test outcome
- Generating reports without blocking pipelines
- Archiving test history across passing and failing runs
- Trend analysis and test reliability tracking

### Both Format

> **Available in:** sfp-pro November 2025 release and later

Generates both raw and dashboard formats in a single execution.

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package \
  --outputformat both \
  --environment ci
```

**When to use:**
- Maintaining compatibility while adopting dashboard format
- Comprehensive test result archiving

## Output Directory Structure

After running tests, sfp creates a `.testresults` directory:

```
.testresults/
├── test-result-<testRunId>-junit.xml      # JUnit XML format
├── test-result-<testRunId>.json           # Raw Salesforce test results
├── test-result-<testRunId>-coverage.json  # Code coverage data
├── testresults.md                         # Markdown summary
├── <testRunId>/                           # Dashboard format directory
│   ├── dashboard.json                     # Structured test results
│   └── testresults.md                     # Enhanced markdown summary
└── latest.json                            # Symlink to latest dashboard result
```

## Troubleshooting

### Tests Timeout

Control how long the command waits for tests to complete:

```bash
# Wait up to 120 minutes
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -w 120

# Wait indefinitely (no timeout) - useful for very large test suites
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -w 0

# Omitting the flag also waits indefinitely
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package
```

**Wait time behavior:**
- **Omit `-w` flag**: Wait indefinitely (no timeout)
- **`-w 0`**: Wait indefinitely (no timeout)
- **`-w <minutes>`**: Wait up to specified minutes before timing out

For most scenarios, omitting the wait time or using 0 is recommended to avoid premature timeouts on large test suites.

### Parallel vs Serial Execution

Some test classes interfere with each other when run in parallel. Configure serial execution in your package descriptor:

```json
{
  "path": "src/my-package",
  "package": "my-package",
  "testSynchronous": true
}
```

Or use the synchronous flag (if supported):

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage \
  -n my-package -s
```

### Coverage Validation Failures

See which classes failed coverage requirements:

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage \
  -n my-package -c --loglevel debug
```

The debug output shows:
- Each class and its coverage percentage
- Which classes passed/failed threshold
- Overall package coverage

### Tests Not Found

If no tests are executed:

1. Check that test classes exist in the package:
   ```bash
   # Verify package contents
   sfp build -d mydevhub -n my-package --loglevel debug
   ```

2. Ensure test classes follow naming conventions:
   - Class name ends with `Test`
   - Methods are annotated with `@isTest`

3. Verify test classes are in the correct package directory

### Mixed Results with Retries

sfp automatically retries failed tests in serial mode. This is normal behavior to handle flaky tests that fail in parallel execution:

1. First run: Tests execute in parallel
2. If failures occur: Failed tests retry in serial mode
3. Final results: Combines both runs, removes duplicates

## Additional Options

### Wait Time Control

Control test execution timeout behavior:

```bash
# Wait indefinitely (recommended for large test suites)
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -w 0

# Wait up to 120 minutes
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -w 120

# Omitting -w also waits indefinitely
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package
```

**Options:**
- Omit `-w`: Wait indefinitely
- `-w 0`: Wait indefinitely
- `-w <minutes>`: Wait specified minutes before timeout

### Specifying API Version

Override the API version for the test run:

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package --apiversion 60.0
```

### Git Metadata

Include git information in test results:

```bash
sfp apextests trigger -o dev-org -l RunLocalTests \
  --commitsha abc123def \
  --repourl https://github.com/myorg/myrepo
```

This metadata appears in:
- Dashboard JSON output
- Markdown summaries
- Test reports

### Custom Environment Name

Specify environment name for dashboard format:

```bash
sfp apextests trigger -o dev-org -l RunLocalTests \
  --outputformat dashboard \
  --environment "dev-feature-branch"
```

Defaults to the target org alias if not specified.
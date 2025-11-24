# Apex Tests

## `@flxbl-io/sfp apextests`

Manage apex tests in a package or an org

* `@flxbl-io/sfp apextests trigger`

### `@flxbl-io/sfp apextests trigger`

Triggers Apex unit tests in an org with support for multiple test levels, code coverage validation, and flexible output formats. This command is essential for validating your Salesforce code changes during development and CI/CD pipelines.

> **New in November 2024:** Dashboard output format (`--outputformat dashboard`) provides structured JSON optimized for metrics collection and reporting tools.

```
USAGE
  $ @flxbl-io/sfp apextests trigger -o <value> [--loglevel
    trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL] [--apiversion <value>] [-l
    RunSpecifiedTests|RunApexTestSuite|RunLocalTests|RunAllTestsInOrg|RunAllTestsInPackage|RunAllTestsInDomain]
    [-n <value>] [-r <value>] [-c] [--validatepackagecoverage] [--specifiedtests <value>]
    [--apextestsuite <value>] [-p <value>] [-w <value>] [--outputformat raw|dashboard|both]
    [--environment <value>] [--commitsha <value>] [--repourl <value>]

FLAGS
  -c, --validateindividualclasscoverage  Validate that individual classes have a coverage greater than the minimum
                                         required percentage coverage, only available when test level is
                                         RunAllTestsInPackage
  -l, --testlevel=<option>               [default: RunLocalTests] The test level of the test that need to be executed
                                         when the code is to be deployed
                                         <options: RunSpecifiedTests|RunApexTestSuite|RunLocalTests|RunAllTestsInOrg|
                                         RunAllTestsInPackage|RunAllTestsInDomain>
  -n, --package=<value>                  Name of the package(s) to run tests. Can be specified multiple times.
                                         Required when test level is RunAllTestsInPackage
  -o, --targetusername=<value>           (required) Username or alias of the target org
  -p, --coveragepercent=<value>          [default: 75] Minimum required percentage coverage, when validating code
                                         coverage
  -r, --releaseconfig=<value>            Path to release config file. Required when test level is RunAllTestsInDomain
  -w, --waittime=<value>                 Wait time for command to finish in minutes. Use 0 or omit for indefinite wait
      --apextestsuite=<value>            comma-separated list of Apex test suite names to run
      --apiversion=<value>               Override the api version used for api requests made by this command
      --commitsha=<value>                Git commit SHA for tracking test execution in reports
      --environment=<value>              Environment name for dashboard format (defaults to target org alias)
      --loglevel=<option>                [default: info] logging level for this command invocation
                                         <options: trace|debug|info|warn|error|fatal|TRACE|DEBUG|INFO|WARN|ERROR|FATAL>
      --outputformat=<option>            [default: raw] Output format for test results
                                         <options: raw|dashboard|both>
      --repourl=<value>                  Repository URL for linking test results
      --specifiedtests=<value>           comma-separated list of Apex test class names or IDs and, if applicable, test
                                         methods to run
      --validatepackagecoverage          Validate that the package coverage is greater than the minimum required
                                         percentage coverage, only available when test level is RunAllTestsInPackage

DESCRIPTION
  Triggers Apex unit tests in an org with support for multiple test levels, code coverage validation, and flexible
  output formats.

  Test Levels:
  • RunLocalTests - Runs all tests in your org that are not from managed packages (default)
  • RunAllTestsInOrg - Runs all tests in your org, including managed packages
  • RunAllTestsInPackage - Runs all tests in specified package(s)
  • RunAllTestsInDomain - Runs all tests for packages in a release config domain
  • RunSpecifiedTests - Runs specific test classes or methods
  • RunApexTestSuite - Runs all tests in a test suite

  Output Formats:
  • raw - Standard Salesforce API output (default) - generates JUnit XML and JSON results
  • dashboard - Structured JSON format optimized for dashboards and reporting tools (November 2024+)
  • both - Generates both raw and dashboard formats (November 2024+)

  The command automatically creates a .testresults directory with:
  • Test run results in JSON format
  • JUnit XML for CI/CD integration
  • Code coverage data (when coverage validation is enabled)
  • Dashboard-formatted JSON (when using dashboard or both output formats)
  • Markdown summary report
  • Symlinks to latest test results

EXAMPLES
  # Run all local tests in an org
  $ sfp apextests trigger -o scratchorg -l RunLocalTests

  # Run tests for a specific package with individual class coverage validation
  $ sfp apextests trigger -o scratchorg -l RunAllTestsInPackage -n my-package -c

  # Run tests for multiple packages
  $ sfp apextests trigger -o scratchorg -l RunAllTestsInPackage -n package1 -n package2

  # Run tests for all packages in a domain (from release config)
  $ sfp apextests trigger -o scratchorg -l RunAllTestsInDomain -r config/release-config.yaml

  # Run specific test classes
  $ sfp apextests trigger -o scratchorg -l RunSpecifiedTests --specifiedtests PaymentTest,InvoiceTest

  # Run test suite
  $ sfp apextests trigger -o scratchorg -l RunApexTestSuite --apextestsuite MySuite

  # Run with package coverage validation (75% minimum)
  $ sfp apextests trigger -o scratchorg -l RunAllTestsInPackage -n my-package --validatepackagecoverage -p 75

  # Run with dashboard output format for reporting
  $ sfp apextests trigger -o scratchorg -l RunLocalTests --outputformat dashboard --environment dev

  # Run with both output formats in CI/CD
  $ sfp apextests trigger -o scratchorg -l RunLocalTests \
    --outputformat both \
    --environment ci \
    --commitsha $CI_COMMIT_SHA \
    --repourl https://github.com/myorg/myrepo

  # Get JSON output for parsing in scripts
  $ sfp apextests trigger -o scratchorg -l RunLocalTests --outputformat dashboard --json

  # Wait indefinitely for large test suites
  $ sfp apextests trigger -o scratchorg -l RunAllTestsInPackage -n my-package -w 0

  # Set specific wait time
  $ sfp apextests trigger -o scratchorg -l RunAllTestsInPackage -n my-package -w 120
```

## Output Directory Structure

The command creates a `.testresults` directory with the following structure:

```
.testresults/
├── test-result-<testRunId>-junit.xml      # JUnit XML format
├── test-result-<testRunId>.json           # Raw Salesforce test results
├── test-result-<testRunId>-coverage.json  # Code coverage data
├── <testRunId>/                           # Dashboard format directory
│   ├── dashboard.json                     # Structured test results
│   └── testresults.md                     # Markdown summary
└── latest.json                            # Symlink to latest dashboard result
```

## Understanding Test Levels

### RunLocalTests
Runs all tests in your org except those from managed packages. This is the default and recommended for most development scenarios.

```bash
sfp apextests trigger -o dev-org -l RunLocalTests
```

### RunAllTestsInPackage
Runs all tests within specified package(s). Ideal for package-focused development and supports coverage validation.

```bash
# Single package
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n sales-core

# Multiple packages
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n sales-core -n sales-ui
```

### RunAllTestsInDomain
Runs tests for all packages in a domain defined in your release config. Perfect for domain-driven development.

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInDomain -r config/release-config.yaml
```

### RunSpecifiedTests
Runs specific test classes or methods. Useful for quick validation during development.

```bash
# Run specific test classes
sfp apextests trigger -o dev-org -l RunSpecifiedTests --specifiedtests AccountTest,ContactTest

# Run specific test methods
sfp apextests trigger -o dev-org -l RunSpecifiedTests --specifiedtests AccountTest.testCreate,ContactTest.testUpdate
```

## Code Coverage Validation

### Individual Class Coverage
Validates that each Apex class meets the minimum coverage threshold:

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -c -p 80
```

### Package Coverage
Validates overall package coverage percentage:

```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package --validatepackagecoverage -p 75
```

## Dashboard Output Format

The dashboard format provides structured data optimized for reporting tools and dashboards:

```bash
sfp apextests trigger -o dev-org -l RunLocalTests --outputformat dashboard --environment production
```

Dashboard JSON includes:
- Test execution summary (passed, failed, skipped)
- Code coverage details per class
- Individual test case results with timing
- Environment and metadata information
- Top failing tests

## CI/CD Integration

### Using with GitHub Actions

```yaml
- name: Run Apex Tests
  run: |
    sfp apextests trigger \
      --targetusername ${{ secrets.ORG_ALIAS }} \
      --testlevel RunLocalTests \
      --outputformat both \
      --environment ci \
      --commitsha ${{ github.sha }} \
      --repourl ${{ github.repository }} \
      --json > test-results.json

- name: Upload Test Results
  uses: actions/upload-artifact@v3
  with:
    name: apex-test-results
    path: .testresults/
```

### Using with JSON Output

```bash
# Capture output for parsing
RESULT=$(sfp apextests trigger -o dev-org -l RunLocalTests --outputformat dashboard --json)

# Extract test run ID
TEST_RUN_ID=$(echo $RESULT | jq -r '.result.testRunId')

# Extract summary
PASSED=$(echo $RESULT | jq -r '.result.summary.passedPackages')
FAILED=$(echo $RESULT | jq -r '.result.summary.failedPackages')
```

## Troubleshooting

### Tests Timeout
Increase wait time for long-running tests:
```bash
sfp apextests trigger -o dev-org -l RunLocalTests -w 120
```

### Parallel vs Serial Execution
Some test classes require serial execution. Configure in your package descriptor:
```json
{
  "testSynchronous": true
}
```

### Coverage Validation Failures
Check which classes failed coverage:
```bash
sfp apextests trigger -o dev-org -l RunAllTestsInPackage -n my-package -c --loglevel debug
```

_See code:_ [_src/commands/apextests/trigger.ts_](https://github.com/flxbl-io/sfp/blob/latest/src/commands/apextests/trigger.ts)

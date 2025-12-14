---
icon: sparkles
---

# AI Assisted Architecture Analysis

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | ❌               |
| From         | October 25 | Not Available   |

\
The AI-powered review functionality provides intelligent architecture and code quality analysis during pull request reviews. This feature automatically analyzes changed files using advanced language models to provide contextual insights about architectural patterns, Flxbl framework compliance, and potential improvements.

### Overview

The architecture analysis performs real-time analysis of pull request changes to:

* Analyze architectural patterns and design consistency
* Identify alignment with Flxbl framework best practices
* Suggest improvements based on changed files context
* Provide severity-based insights (info, warning, concern)
* Generate actionable recommendations

### How It Works

The AI assisted architecture analyzer integrates into the `project:analyze` command and:

1. **Detects PR Context**: Automatically identifies when running in a pull request environment
2. **Analyzes Changed Files**: Focuses analysis on modified files only (up to 10 files for token optimization)
3. **Applies AI Analysis**: Uses configured AI provider to analyze architectural patterns
4. **Reports Findings**: Generates structured insights without failing the build (informational only)
5. **Creates GitHub Checks**: Posts results as GitHub check annotations when running in CI

### Prerequisites

{% hint style="danger" %}
OpenCode is currently only supported on OSX or Linux runtimes. It's not supported for Windows platforms.
{% endhint %}

{% hint style="info" %}
This feature is exclusive to sfp-pro and not available in the community edition.
{% endhint %}

For complete setup instructions, see [Configuring LLM Providers](../getting-started/configuring-llm-providers.md).

### Configuration

The architecture analyzer is configured through a YAML configuration file at `config/ai-architecture.yaml`:

```yaml
# Enable/disable AI architecture analysis
enabled: true

# AI Provider Configuration (optional - auto-detects if not specified)
provider: anthropic  # Options: anthropic, openai, google
model: claude-4-sonnet-xxxxx # Optional - uses provider defaults if not specified

# Architectural Patterns to Check
patterns:
  - singleton
  - factory
  - repository
  - service-layer

# Architecture Principles
principles:
  - separation-of-concerns
  - single-responsibility
  - dependency-inversion

# Focus Areas for Analysis
focusAreas:
  - security
  - performance
  - maintainability
  - testability

# Additional Context Files (optional)
contextFiles:
  - ARCHITECTURE.md
  - docs/patterns.md
```

#### Minimal Configuration

For quick setup, create a minimal configuration:

```yaml
enabled: true
```

The linter will auto-detect available AI providers and use sensible defaults.

### AI Provider Setup

For detailed provider configuration, see [Configuring LLM Providers](../getting-started/configuring-llm-providers.md).

#### Quick Reference

| Provider                | Default Model         | Setup Command                             |
| ----------------------- | --------------------- | ----------------------------------------- |
| Anthropic (Recommended) | claude-4-5            | `sfp ai auth --provider anthropic --auth` |
| OpenAI                  | gpt-5                 | `sfp ai auth --provider openai --auth`    |
| Amazon Bedrock          | claude-4-sonnet-xxxxx | Configure AWS credentials                 |

The linter auto-detects providers in this priority:

1. Environment variables (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, etc.)
2. Configuration in `ai-architecture.yaml`

### Usage in Pull Requests

#### Automatic PR Detection

When running in GitHub Actions or with PR environment variables:

```bash
# Automatically detects PR context and analyzes only changed files
sfp project:analyze

# Explicitly exclude AI linter if needed
sfp project:analyze --exclude-linters architecture
```

#### Manual Changed Files Specification

For local testing or custom CI environments:

```bash
# Manually specify changed files
sfp project:analyze --changed-files "src/classes/MyClass.cls,src/lwc/myComponent/myComponent.js"
```

### Understanding Results

The AI linter provides structured insights without failing builds:

#### Insight Types

* **Pattern**: Architectural patterns observed or missing
* **Concern**: Potential issues requiring attention
* **Suggestion**: Improvement recommendations
* **Alignment**: Framework compliance observations

#### Severity Levels

* **Info**: Informational observations
* **Warning**: Areas needing attention
* **Concern**: Significant architectural considerations

#### Sample Output

```markdown
📐 Architecture Analysis Results
════════════════════════════════

✅ Analysis Complete (AI-powered by anthropic/claude-4-sonnet)

## Summary
Analyzed 5 changed files focusing on architectural patterns and Flxbl compliance.

## Key Insights

### ⚠️ Service Layer Pattern (Warning)
File: src/classes/AccountController.cls
Description: Direct SOQL queries in controller violates service layer pattern.
Consider moving data access logic to a dedicated service class.

### ℹ️ Dependency Management (Info)
File: src/classes/OrderService.cls
Description: Good use of dependency injection pattern for testability.
This aligns well with Flxbl framework principles.

### ⚠️ Error Handling (Concern)
File: src/classes/PaymentProcessor.cls:45
Description: Missing comprehensive error handling for external callouts.
Implement try-catch blocks with proper logging and user feedback.

## Recommendations
1. Extract data access logic to service layer classes
2. Implement centralized error handling strategy
3. Consider adding unit tests for new service methods
4. Document architectural decisions in ARCHITECTURE.md
```

### Integration with CI/CD

{% hint style="info" %}
AI linter results are informational only and never fail the build. This ensures PR checks remain stable even if AI providers are unavailable.
{% endhint %}

#### GitHub Actions Integration

```yaml
- name: Run Project Analysis with AI Linter
  run: |
    sfp project:analyze --output-format github
  env:
    ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
    # GitHub context automatically detected
```

#### Handling Rate Limits

The linter gracefully handles API limitations:

* **Rate Limits**: Skips analysis with informational message
* **Timeouts**: 60-second timeout protection
* **Token Limits**: Analyzes up to 10 files, content limited to 5KB per file
* **Failures**: Never blocks PR merge (informational only)

### Best Practices

#### 1. Configure Focus Areas

Tailor analysis to your team's priorities:

```yaml
focusAreas:
  - security        # For compliance-critical projects
  - performance     # For high-volume applications
  - maintainability # For long-term projects
```

#### 2. Add Context Files

Provide architectural documentation for better analysis:

```yaml
contextFiles:
  - ARCHITECTURE.md
  - docs/coding-standards.md
  - docs/patterns.md
```

#### 3. Use with Other Linters

Combine with other analysis tools for comprehensive coverage:

```bash
# Run all linters including AI analysis
sfp project:analyze --fail-on duplicates,compliance

# AI linter provides insights, others enforce rules
```

#### 4. Token Optimization

For large PRs, the linter automatically:

* Limits to 10 most relevant files
* Truncates file content to 5KB
* Focuses on text-based source files

### Troubleshooting

#### AI Provider Not Detected

```bash
# Check available providers
echo $ANTHROPIC_API_KEY
echo $OPENAI_API_KEY
```

#### Analysis Skipped

Common reasons and solutions:

1. **Not Enabled**: Set `enabled: true` in `config/ai-architecture.yaml`
2. **No Provider**: Configure API keys or authenticate with `sfp ai auth`
3. **Rate Limited**: Wait for rate limit reset or use different provider
4. **No Changed Files**: Ensure PR context is properly detected

#### Debugging

Enable debug logging for detailed information:

```bash
sfp project:analyze --loglevel debug
```

This shows:

* Provider detection process
* Changed files identified
* API calls and responses
* Error details if analysis fails

### Limitations

1. **Binary Files**: Skips non-text files
2. **Build Impact**: Never fails builds (informational only)
3. **Language Support**: Best for Apex, JavaScript, TypeScript, XML

---
icon: sparkles
---

# AI Assisted Insight Report

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | Limited         |
| From         | October 25 | October 25      |

\
The AI-powered report functionality generates comprehensive analysis reports for your Salesforce projects using advanced language models. This feature provides deep insights into code quality, architecture, and best practices specific to the Flxbl/sfp framework.

### Overview

The report generator analyzes your codebase through multiple perspectives:

* Package architecture and design patterns
* Dependencies and coupling between packages
* Code quality and technical debt
* Flxbl best practices compliance
* Security and compliance considerations

### Authentication

Before generating reports, you need to authenticate with an AI provider. The command supports multiple providers including Anthropic, GitHub Copilot, OpenAI, and Google.

#### Setting Up Authentication

```bash
# Authenticate interactively - prompts for API key
sfp ai:auth --provider anthropic --auth

# Check authentication status
sfp ai:auth

# Check specific provider
sfp ai:auth --provider anthropic

# List all supported providers
sfp ai:auth --list
```

The authentication command stores credentials securely in `~/.sfp/ai-auth.json`.

### Basic Usage

#### Package Analysis

```bash
# Analyze single package
sfp project:report --package nextGen

# Analyze multiple packages
sfp project:report --package core --package utils --output core-utils-analysis.md
```

#### Domain Analysis ( sfp-pro only)

```bash
# Analyze all packages in a domain
sfp project:report --domain billing --output billing-analysis.md
```

### Provider-Specific Examples

#### Anthropic (Claude Code)

```bash
# Uses defaults (provider: anthropic, model: claude-sonnet-4-20250514)
sfp project:report --package nextGen --output nextgen-analysis.md

# Specify different model
sfp project:report --model claude-sonnet-4-20250514 --package core
```

#### GitHub Copilot

{% hint style="info" %}
Ensure the corresponding models are activated in GitHub Copilot Settings
{% endhint %}

```bash
# Must specify provider explicitly (uses claude-sonnet-4 by default)
sfp project:report --provider github-copilot --package rate-changes

# With explicit model
sfp project:report --provider github-copilot --model claude-sonnet-4 --domain service
```

### Output Format

Reports are generated in Markdown format with the following structure:

1. **Executive Summary** - High-level findings and recommendations
2. **Package/Domain Overview** - Architecture and design analysis
3. **Dependencies Analysis** - Inter-package relationships
4. **Code Quality Insights** - Technical debt and improvement opportunities
5. **Recommendations** - Prioritized action items

### Troubleshooting

#### No Authentication Found

```bash
# Check current auth status
sfp ai:auth

# If not authenticated, set up credentials
sfp ai:auth --provider anthropic --auth
```

#### Rate Limiting

If you encounter rate limits:

* Reduce `--prompt-count` to lower token usage
* Analyze smaller scopes (single package vs domain)
* Consider using a different model with higher limits

### Cost Considerations

* **Token Usage**: Package analysis typically uses 10-30K tokens, domains 30-80K tokens
* **Models**: Claude Sonnet provides best value, Opus for complex analysis
* **GitHub Copilot**: No additional cost if you have Copilot subscription

### See Also

* [Duplicate Check](duplicate-check.md) - For identifying duplicate components
* [Compliance Check](compliance-check.md) - For rule-based code analysis

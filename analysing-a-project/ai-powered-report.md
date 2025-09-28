---
icon: sparkles
---

# AI Assisted Insight Report

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | 🔶              |
| From         | October 25 | December 25     |

\
The AI-powered report functionality generates comprehensive analysis reports for your Salesforce projects using advanced language models. This feature provides deep insights into code quality, architecture, and best practices specific to the Flxbl framework.

### Overview

The report generator analyzes your codebase through multiple perspectives:

* Package architecture and design patterns
* Dependencies and coupling between packages
* Code quality and technical debt
* Flxbl best practices compliance
* Security and compliance considerations

### Prerequisites

{% hint style="danger" %}
OpenCode is currently only supported on OSX or Linux runtimes. It's not supported for Windows platforms.
{% endhint %}

{% hint style="warning" %}
For sfp (community) users: These features are available in alpha. You can use `npm install -g @flxbl-io/sfp@ai` to access them.
{% endhint %}

For complete setup and configuration instructions, see [Configuring LLM Providers](../getting-started/configuring-llm-providers.md).

#### Quick Setup

```bash
# Install OpenCode CLI
npm install -g opencode-ai

# Configure Anthropic (recommended)
sfp ai auth --provider anthropic --auth

# Verify authentication
sfp ai auth

# Test provider inference
sfp ai check --provider anthropic
```

### Basic Usage

#### Package Analysis

```bash
# Analyze single package
sfp project report --package nextGen

# Analyze multiple packages
sfp project report --package core --package utils --output core-utils-analysis.md
```

#### Domain Analysis ( sfp-pro only)

```bash
# Analyze all packages in a domain
sfp project report --domain billing --output billing-analysis.md
```

### Provider-Specific Examples

#### Anthropic (Recommended)

```bash
# Uses defaults (provider: anthropic, model: claude-sonnet-4-20250514)
sfp project report --package nextGen --output nextgen-analysis.md

# Specify different model (if needed)
sfp project report --model claude-sonnet-4-20250514 --package core
```

#### GitHub Copilot

{% hint style="info" %}
Ensure the corresponding models are activated in GitHub Copilot Settings
{% endhint %}

```bash
# Must specify provider explicitly (uses claude-sonnet-4 by default)
sfp project report --provider github-copilot --package rate-changes

# Uses default model for GitHub Copilot
sfp project report --provider github-copilot --domain service
```

#### Amazon Bedrock

```bash
# Uses default model: anthropic.claude-sonnet-4-20250514-v1:0
sfp project report --provider amazon-bedrock --package core

# Specify different region (if not in environment)
export AWS_REGION=eu-west-1
sfp project report --provider amazon-bedrock --domain billing
```


### Testing Provider Configuration

Before running reports, you can verify your provider setup using the `sfp ai check` command:

```bash
# Test all configured providers
sfp ai check

# Test specific provider with default model
sfp ai check --provider anthropic

# Test Amazon Bedrock (uses default: anthropic.claude-sonnet-4-20250514-v1:0)
sfp ai check --provider amazon-bedrock

# Test GitHub Copilot (uses default: claude-sonnet-4)
sfp ai check --provider github-copilot

```

This command will:
- Verify authentication is configured
- Test model inference capabilities
- Report response time and performance
- Help troubleshoot configuration issues

### Output Format

Reports are generated in Markdown format with the following structure:

1. **Executive Summary** - High-level findings and recommendations
2. **Package/Domain Overview** - Architecture and design analysis
3. **Dependencies Analysis** - Inter-package relationships
4. **Code Quality Insights** - Technical debt and improvement opportunities
5. **Recommendations** - Prioritized action items

### Troubleshooting

#### OpenCode CLI Not Found

If you see an error about OpenCode CLI not being installed:

```bash
# Install OpenCode CLI
npm install -g opencode-ai

# Verify installation
opencode --version
```

#### No Authentication Found

```bash
# Check current auth status
sfp ai auth

# If not authenticated, set up credentials
sfp ai auth --provider anthropic --auth
```

#### Rate Limiting

If you encounter rate limits:

* Reduce `--prompt-count` to lower token usage
* Analyze smaller scopes (single package vs domain)
* Consider using a different model with higher limits

### Cost Considerations

* **Token Usage**: Package analysis typically uses 10-30K tokens, domains 30-80K tokens
* **Models**: Default models are optimized for best value and performance
* **GitHub Copilot**: No additional cost if you have Copilot subscription
* **Amazon Bedrock**: Pay-per-use pricing through AWS, check Bedrock pricing in your region

### See Also

* [Configuring LLM Providers](../getting-started/configuring-llm-providers.md) - Complete setup guide for AI providers
* [AI-Powered PR Linter](ai-pr-linter.md) - Automated PR analysis
* [Duplicate Check](duplicate-check.md) - For identifying duplicate components
* [Compliance Check](compliance-check.md) - For rule-based code analysis

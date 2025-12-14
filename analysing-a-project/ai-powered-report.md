---
icon: sparkles
---

# AI Assisted Insight Report

|              | sfp-pro    | sfp (community) |
| ------------ | ---------- | --------------- |
| Availability | ✅          | ❌               |
| From         | October 25 | Not Available   |

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

For complete setup and configuration instructions, see [Configuring LLM Providers](../getting-started/configuring-llm-providers.md).

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



#### Amazon Bedrock

```bash
# Uses default model: anthropic.claude-sonnet-4-20250514-v1:0
sfp project report --provider amazon-bedrock --package core

# Specify different region (if not in environment)
export AWS_REGION=eu-west-1
sfp project report --provider amazon-bedrock --domain billing
```

### Output Format

Reports are generated in Markdown format with the following structure:

1. **Executive Summary** - High-level findings and recommendations
2. **Package/Domain Overview** - Architecture and design analysis
3. **Dependencies Analysis** - Inter-package relationships
4. **Code Quality Insights** - Technical debt and improvement opportunities
5. **Recommendations** - Prioritized action items

### Troubleshooting

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

# Configuring LLM Providers

|              | sfp-pro                        | sfp (community) |
| ------------ | ------------------------------ | --------------- |
| Availability | ✅                              | 🔶              |
| From         | October 25                     | December 25     |
| Features     | PR Linter, Reports, Error Analysis | Reports Only    |

This guide covers the setup and configuration of Large Language Model (LLM) providers for AI-powered features in sfp:

* [**AI-Powered PR Linter**](../analysing-a-project/ai-pr-linter.md) - sfp-pro only
* [**AI Assisted Insight Reports**](../analysing-a-project/ai-powered-report.md) - Available in both sfp-pro and community (alpha)
* [**AI-Assisted Error Analysis**](../validating-a-change/ai-assisted-error-analysis.md) - Intelligent validation error analysis

These features require OpenCode CLI and an authenticated LLM provider.

{% hint style="danger" %}
OpenCode is currently only supported on OSX or Linux runtimes. It's not supported for Windows platforms.
{% endhint %}

{% hint style="warning" %}
For sfp (community) users: These AI features are available in alpha. You can use `npm install -g @flxbl-io/sfp@ai` to access them.
{% endhint %}

## Prerequisites

### OpenCode CLI Installation

OpenCode CLI is the underlying engine that manages AI interactions for sfp's AI-powered features. It handles provider authentication, model selection, and secure API communication.

#### Installation Methods

**Global Installation (Recommended)**

```bash
# Install OpenCode CLI globally via npm
npm install -g opencode-ai

# Verify installation
opencode --version
```

**Alternative Installation Methods**

```bash
# Using yarn
yarn global add opencode-ai

# Using pnpm
pnpm add -g opencode-ai

# Using Homebrew (macOS/Linux)
brew install opencode-ai
```

For more installation options and troubleshooting, see the [OpenCode documentation](https://opencode.ai/docs#install).

## Supported LLM Providers

sfp currently supports the following LLM providers through OpenCode:

| Provider               | Status            | Recommended | Best For                                                |
| ---------------------- | ----------------- | ----------- | ------------------------------------------------------- |
| **Anthropic (Claude)** | ✅ Fully Supported | ⭐ Yes       | Best overall performance, Flxbl framework understanding |
| **OpenAI**             | ✅ Fully Supported | Yes         | Wide model selection, good performance                  |
| **Amazon Bedrock**     | ✅ Fully Supported | Yes         | Enterprise environments with AWS infrastructure         |

## Provider Configuration

### Anthropic (Claude) - Recommended

Anthropic's Claude models provide the best understanding of Salesforce and Flxbl framework patterns. The default model used is `claude-4-sonnet-xxxxx` which offers optimal balance between performance and cost.

#### Setup Methods

**Method 1: Interactive Authentication (Recommended)**

```bash
# Authenticate with Anthropic
sfp ai auth --provider anthropic --auth

# This will prompt for your API key and store it securely
```

**Method 2: Environment Variable**

```bash
# Add to your shell profile (.bashrc, .zshrc, etc.)
export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxx"
```

**Method 3: Configuration File** Create or edit `config/ai-architecture.yaml`:

```yaml
enabled: true
provider: anthropic
# Model is optional - uses claude-sonnet-4-20250514 by default
```

#### Getting an Anthropic API Key

1. Visit [console.anthropic.com](https://console.anthropic.com)
2. Sign up or log in to your account
3. Navigate to API Keys section
4. Create a new API key for sfp usage
5. Copy the key (starts with `sk-ant-`)

{% hint style="info" %}
**Claude Models Available:**

* `claude-4-sonnet-xxxxx` - Recommended, best balance (default)
* `claude-4-opus-xxxxx` - Most capable, higher cost
{% endhint %}

### OpenAI

OpenAI provides access to GPT models with good code analysis capabilities.

#### Setup Methods

**Method 1: Interactive Authentication**

```bash
sfp ai auth --provider openai --auth
```

**Method 2: Environment Variable**

```bash
export OPENAI_API_KEY="sk-xxxxxxxxxxxxx"
```

**Method 3: Configuration File**

```yaml
# In config/ai-assist.yaml
enabled: true
provider: openai
# Model is optional - uses gpt-5 by default
```

#### Getting an OpenAI API Key

1. Visit [platform.openai.com](https://platform.openai.com)
2. Sign up or log in
3. Go to API Keys section
4. Create a new secret key
5. Copy the key (starts with `sk-`)

### Amazon Bedrock

Amazon Bedrock is ideal for enterprise environments already using AWS infrastructure. It provides access to Claude models through AWS.

#### Setup Methods

**Method 1: AWS Profile**

```bash
# Set both required environment variables
export AWS_BEARER_TOKEN_BEDROCK="your-bearer-token"
export AWS_REGION="us-east-1"

# Both variables must be set for authentication to work
```

**Method 2: AWS Credentials**

```bash
# Authenticate with Amazon Bedrock
sfp ai auth --provider amazon-bedrock --auth

# This will prompt for both Bearer Token and Region
```

**Method 3: Configuration File**

```yaml
# In config/ai-assist.yaml
enabled: true
provider: amazon-bedrock
model: anthropic.claude-sonnet-4-20250514-v1:0  # Default model
```

{% hint style="warning" %}
**Important**: AWS Bedrock requires both `AWS_BEARER_TOKEN_BEDROCK` and `AWS_REGION` environment variables to be set. Authentication will fail if either is missing.
{% endhint %}

{% hint style="warning" %}
**Bedrock Model Access**: Ensure your AWS account has access to the Claude models in Bedrock. You may need to request access through the AWS Console under Bedrock > Model access.
{% endhint %}

#### Regional Considerations

Bedrock automatically handles model prefixes based on your AWS region:

* **US Regions**: Models may require `us.` prefix
* **EU Regions**: Models may require `eu.` prefix
* **AP Regions**: Models may require `apac.` prefix

The OpenCode SDK handles this automatically based on your `AWS_REGION`.

### GitHub Copilot

GitHub Copilot can be used if you have an active subscription with model access enabled.

#### Setup

```bash
# Authenticate with GitHub Copilot
sfp ai auth --provider github-copilot --auth

# Ensure models are enabled in GitHub Settings:
# https://github.com/settings/copilot/features
```

{% hint style="info" %}
GitHub Copilot requires the corresponding models to be activated in your GitHub Copilot Settings. Visit [GitHub Copilot Features](https://github.com/settings/copilot/features) to enable model access.
{% endhint %}

## Configuration File Reference

The AI features are configured through `config/ai-assist.yaml` in your project root:

```yaml
# Enable/disable AI features
enabled: true

# Provider Configuration
provider: anthropic  # anthropic, openai, amazon-bedrock, github-copilot

# Model Configuration (Optional - uses provider defaults if not specified)
# Default models:
# - anthropic: claude-sonnet-4-20250514
# - github-copilot: claude-sonnet-4
# - openai: gpt-5
# - amazon-bedrock: anthropic.claude-sonnet-4-20250514-v1:0
model: claude-sonnet-4-20250514  # Override default model

# Architectural Patterns to Check (for PR Linter)
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

# Additional Context Files
contextFiles:
  - ARCHITECTURE.md
  - docs/patterns.md
  - docs/coding-standards.md
```

## Authentication Management

### Checking Authentication Status

```bash
# Check all providers
sfp ai auth

# Check specific provider
sfp ai auth --provider anthropic

# List all supported providers
sfp ai auth --list
```

### Testing Provider Inference

After configuring authentication, you can verify that providers are working correctly using the `ai check` command:

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

This command performs a simple inference test to verify:
- Authentication is configured correctly
- The provider is accessible
- Model inference is working
- Response time and performance

### Authentication Storage

Credentials are stored securely in `~/.sfp/ai-auth.json` with appropriate file permissions. This file is created automatically when you authenticate.

### Rotating API Keys

```bash
# Re-authenticate to update stored credentials
sfp ai auth --provider anthropic --auth

# Or update environment variable
export ANTHROPIC_API_KEY="sk-ant-new-key-xxxxx"
```

## Usage Priority

When multiple authentication methods are available, sfp uses the following priority:

1. **Environment Variables** - Highest priority, useful for CI/CD
2. **Stored Credentials** - From `~/.sfp/ai-auth.json`
3. **Configuration File** - From `config/ai-assist.yaml`

## Troubleshooting

### OpenCode CLI Not Found

```bash
# Verify installation
which opencode

# If not found, reinstall
npm install -g opencode-ai

# Check npm global bin path is in PATH
npm bin -g
```

### Provider Not Available

```bash
# Check authentication
sfp ai auth --provider anthropic

# Verify environment variables
echo $ANTHROPIC_API_KEY

# For AWS Bedrock - check both required variables
echo $AWS_BEARER_TOKEN_BEDROCK
echo $AWS_REGION

# Check stored credentials exist
ls -la ~/.sfp/ai-auth.json

# Test provider inference
sfp ai check --provider <provider-name>
```

### AWS Bedrock Specific Issues

**Both Environment Variables Required**
```bash
# This will NOT work (missing region)
export AWS_BEARER_TOKEN_BEDROCK="token"

# This will work (both variables set)
export AWS_BEARER_TOKEN_BEDROCK="token"
export AWS_REGION="us-east-1"
```

**Authentication Failed**
- Verify both `AWS_BEARER_TOKEN_BEDROCK` and `AWS_REGION` are set
- Check that your bearer token is valid and not expired
- Ensure your AWS account has access to Claude models in Bedrock

### API Rate Limits

If you encounter rate limits:

* **Anthropic**: Check your usage at [console.anthropic.com](https://console.anthropic.com)
* **OpenAI**: Monitor at [platform.openai.com/usage](https://platform.openai.com/usage)
* **Bedrock**: Check AWS CloudWatch metrics

### Model Not Found

Ensure you're using the correct model identifier:

```yaml
# Correct
model: claude-4-sonnet-xxxxx

```

# Configuring LLM Providers

|              | sfp-pro                            | sfp (community) |
| ------------ | ---------------------------------- | --------------- |
| Availability | ✅                                  | N/A             |
| From         | October 25                         |                 |
| Features     | PR Linter, Reports, Error Analysis |                 |

This guide covers the setup and configuration of Large Language Model (LLM) providers for AI-powered features in sfp:

* [**AI-Powered PR Linter**](../analysing-a-project/ai-pr-linter.md) - sfp-pro only
* [**AI Assisted Insight Reports**](../analysing-a-project/ai-powered-report.md) - Available in both sfp-pro
* [**AI-Assisted Error Analysis**](../validating-a-change/ai-assisted-error-analysis.md) - Intelligent validation error analysis

## Prerequisites

#### Installation Methods

## Supported LLM Providers

sfp currently supports the following LLM providers through OpenCode:

| Provider               | Status            | Recommended | Best For                                                |
| ---------------------- | ----------------- | ----------- | ------------------------------------------------------- |
| **Anthropic (Claude)** | ✅ Fully Supported | ⭐ Yes       | Best overall performance, Flxbl framework understanding |
| **OpenAI**             | ✅ Fully Supported | Yes         | Wide model selection, good performance                  |
| **Amazon Bedrock**     | ✅ Fully Supported | Yes         | Enterprise environments with AWS infrastructure         |
| **GitHub Copilot**     | ✅ Fully Supported | Yes         | Teams with existing Copilot subscriptions, no extra cost |

## Provider Configuration

### Anthropic (Claude) - Recommended

Anthropic's Claude models provide the best understanding of Salesforce and Flxbl framework patterns. The default model used is `claude-sonnet-4-5-20250929` which offers optimal balance between performance and cost.

#### Setup

**Step 1: Environment Variable**

```bash
# Add to your shell profile (.bashrc, .zshrc, etc.)
export ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxxx"

# Verify the configuration works
sfp ai test --provider anthropic
```

**Step 2: Configuration File** Create or edit `config/ai-architecture.yaml`:

```yaml
enabled: true
provider: anthropic
# Model is optional - uses claude-sonnet-4-5-20250929 by default
```

#### Getting an Anthropic API Key

1. Visit [console.anthropic.com](https://console.anthropic.com)
2. Sign up or log in to your account
3. Navigate to API Keys section
4. Create a new API key for sfp usage
5. Copy the key (starts with `sk-ant-`)

{% hint style="info" %}
**Claude Models Available:**

* `claude-sonnet-4-5-20250929` - Recommended, best balance (default)
* `claude-opus-4-0-20250514` - Most capable, higher cost
{% endhint %}

### OpenAI

OpenAI provides access to GPT models with good code analysis capabilities.

#### Setup

**Step 1: Environment Variable**

```bash
export OPENAI_API_KEY="sk-xxxxxxxxxxxxx"

# Verify the configuration works
sfp ai test --provider openai
```

**Step 2: Configuration File**

```yaml
# In config/ai-assist.yaml
enabled: true
provider: openai
# Model is optional - uses gpt-4o by default
```

#### Getting an OpenAI API Key

1. Visit [platform.openai.com](https://platform.openai.com)
2. Sign up or log in
3. Go to API Keys section
4. Create a new secret key
5. Copy the key (starts with `sk-`)

### Amazon Bedrock

Amazon Bedrock is ideal for enterprise environments already using AWS infrastructure. It provides access to Claude models through AWS.

#### Setup

**Step 1: AWS Profile**

```bash
# Set both required environment variables
export AWS_BEARER_TOKEN_BEDROCK="your-bearer-token"
export AWS_REGION="us-east-1"

# Both variables must be set for authentication to work
# Verify the configuration works
sfp ai test --provider amazon-bedrock
```

**STEP 3: Configuration File**

```yaml
# In config/ai-assist.yaml
enabled: true
provider: amazon-bedrock
model: anthropic.claude-sonnet-4-5-20250929-v1:0  # Default model
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

{% hint style="info" %}
**Prerequisites:**
- Active GitHub Copilot subscription (Individual, Business, or Enterprise)
- Models must be enabled in your GitHub Copilot settings
- Visit [GitHub Copilot Features](https://github.com/settings/copilot/features) to enable model access
{% endhint %}

#### Setup Methods

**Method 1: Generate Token Using Script (Recommended)**

sfp includes a helper script to generate Copilot tokens via the GitHub device flow:

```bash
# Run the token generator script
./scripts/get-copilot-token.sh
```

The script will:
1. Request a device code from GitHub
2. Display a URL and verification code
3. Open your browser automatically (on supported systems)
4. Poll for authorization completion
5. Output the OAuth token (`ghu_` prefixed)

After the script completes, set the token:

```bash
# Set the token in your environment
export COPILOT_TOKEN="ghu_xxxxxxxxxxxx"

# Verify the configuration works
sfp ai test --provider github-copilot
```

**Method 2: Environment Variable (CI/CD)**

For CI/CD pipelines, set the `COPILOT_TOKEN` environment variable:

```bash
# Add to your shell profile or CI/CD secrets
export COPILOT_TOKEN="ghu_xxxxxxxxxxxx"
```

In GitHub Actions:

```yaml
jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run AI Analysis
        env:
          COPILOT_TOKEN: ${{ secrets.COPILOT_TOKEN }}
        run: |
          sfp project:analyze --provider github-copilot
```

{% hint style="warning" %}
**Important**: Use `COPILOT_TOKEN` instead of `GITHUB_TOKEN` in CI/CD environments. The `GITHUB_TOKEN` is automatically set by GitHub Actions for repository operations and may conflict with Copilot authentication.
{% endhint %}

#### How Token Exchange Works

sfp automatically handles the OAuth token exchange process:

1. **OAuth Token** (`ghu_` prefix): The token you obtain from device flow authentication
2. **API Token Exchange**: sfp automatically exchanges the OAuth token for a Copilot API token via GitHub's internal API
3. **Transparent Process**: This exchange happens automatically when you use `--provider github-copilot`

```
OAuth Token (ghu_xxx) → Exchange API → Copilot API Token → AI Model Access
```

#### Configuration File

```yaml
# In config/ai-assist.yaml
enabled: true
provider: github-copilot
# Model is optional - uses claude-sonnet-4.5 by default for GitHub Copilot
```

#### Available Models

GitHub Copilot provides access to various models. The default is `claude-sonnet-4.5`:

| Model | Description | Notes |
|-------|-------------|-------|
| `claude-sonnet-4.5` | Claude Sonnet 4.5 | Default, recommended |
| `gpt-4.1` | GPT-4.1 | Alternative option |

{% hint style="info" %}
**Model Naming**: GitHub Copilot uses simplified model names without date suffixes (e.g., `claude-sonnet-4.5` instead of `claude-sonnet-4-20250514`).
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
# - anthropic: claude-sonnet-4-5-20250929
# - openai: gpt-4o
# - github-copilot: gpt-4o
# - amazon-bedrock: anthropic.claude-sonnet-4-5-20250929-v1:0
model: claude-sonnet-4-5-20250929  # Override default model

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

## Testing Provider Configuration


The test command performs a complete health check:
- **Authentication**: Verifies credentials are available
- **Connectivity**: Confirms the provider endpoint is reachable
- **Response**: Validates the model returns a valid response

### Environment Variables Reference

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

* Authentication is configured correctly
* The provider is accessible
* Model inference is working
* Response time and performance

## Usage Priority

When multiple authentication methods are available, sfp uses the following priority:

1. **Environment Variables** - Highest priority, recommended for CI/CD
2. **Configuration File** - From `config/ai-assist.yaml`

## Troubleshooting

### Provider Not Available

```bash

# Verify environment variables
echo $ANTHROPIC_API_KEY

# For AWS Bedrock - check both required variables
echo $AWS_BEARER_TOKEN_BEDROCK
echo $AWS_REGION

# For GitHub Copilot
echo $COPILOT_TOKEN

# Test provider connectivity
sfp ai test --provider <provider-name>
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

* Verify both `AWS_BEARER_TOKEN_BEDROCK` and `AWS_REGION` are set
* Check that your bearer token is valid and not expired
* Ensure your AWS account has access to Claude models in Bedrock

### API Rate Limits

If you encounter rate limits:

* **Anthropic**: Check your usage at [console.anthropic.com](https://console.anthropic.com)
* **OpenAI**: Monitor at [platform.openai.com/usage](https://platform.openai.com/usage)
* **Bedrock**: Check AWS CloudWatch metrics

### Model Not Found

Ensure you're using the correct model identifier for your provider:

```yaml
# Anthropic
model: claude-sonnet-4-5-20250929

# OpenAI
model: gpt-4o

# GitHub Copilot
model: gpt-4o

# Amazon Bedrock
model: anthropic.claude-sonnet-4-5-20250929-v1:0
```

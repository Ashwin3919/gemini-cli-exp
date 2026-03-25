# Original Gemini CLI README

> Everything below is the upstream README from
> [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli),
> preserved in full. All credit to the Google Gemini team.

---

[![Gemini CLI CI](https://github.com/google-gemini/gemini-cli/actions/workflows/ci.yml/badge.svg)](https://github.com/google-gemini/gemini-cli/actions/workflows/ci.yml)
[![Gemini CLI E2E (Chained)](https://github.com/google-gemini/gemini-cli/actions/workflows/chained_e2e.yml/badge.svg)](https://github.com/google-gemini/gemini-cli/actions/workflows/chained_e2e.yml)
[![Version](https://img.shields.io/npm/v/@google/gemini-cli)](https://www.npmjs.com/package/@google/gemini-cli)
[![License](https://img.shields.io/github/license/google-gemini/gemini-cli)](https://github.com/google-gemini/gemini-cli/blob/main/LICENSE)

![Gemini CLI Screenshot](/docs/assets/gemini-screenshot.png)

Gemini CLI is an open-source AI agent that brings the power of Gemini directly
into your terminal. It provides lightweight access to Gemini, giving you the
most direct path from your prompt to our model.

Learn all about Gemini CLI in our [documentation](https://geminicli.com/docs/).

## 🚀 Why Gemini CLI?

- **🎯 Free tier**: 60 requests/min and 1,000 requests/day with personal Google
  account.
- **🧠 Powerful Gemini 3 models**: Access to improved reasoning and 1M token
  context window.
- **🔧 Built-in tools**: Google Search grounding, file operations, shell
  commands, web fetching.
- **🔌 Extensible**: MCP (Model Context Protocol) support for custom
  integrations.
- **💻 Terminal-first**: Designed for developers who live in the command line.
- **🛡️ Open source**: Apache 2.0 licensed.

## 📦 Installation

See
[Gemini CLI installation, execution, and releases](./docs/get-started/installation.md)
for recommended system specifications and a detailed installation guide.

### Quick Install

#### Run instantly with npx

```bash
npx @google/gemini-cli
```

#### Install globally with npm

```bash
npm install -g @google/gemini-cli
```

#### Install globally with Homebrew (macOS/Linux)

```bash
brew install gemini-cli
```

#### Install globally with MacPorts (macOS)

```bash
sudo port install gemini-cli
```

#### Install with Anaconda (for restricted environments)

```bash
conda create -y -n gemini_env -c conda-forge nodejs
conda activate gemini_env
npm install -g @google/gemini-cli
```

## Release Cadence and Tags

See [Releases](./docs/releases.md) for more details.

### Preview

```bash
npm install -g @google/gemini-cli@preview
```

### Stable

```bash
npm install -g @google/gemini-cli@latest
```

### Nightly

```bash
npm install -g @google/gemini-cli@nightly
```

## 📋 Key Features

### Code Understanding & Generation

- Query and edit large codebases
- Generate new apps from PDFs, images, or sketches using multimodal capabilities
- Debug issues and troubleshoot with natural language

### Automation & Integration

- Automate operational tasks like querying pull requests or handling complex
  rebases
- Use MCP servers to connect new capabilities, including
  [media generation with Imagen, Veo or Lyria](https://github.com/GoogleCloudPlatform/vertex-ai-creative-studio/tree/main/experiments/mcp-genmedia)
- Run non-interactively in scripts for workflow automation

### Advanced Capabilities

- Ground your queries with built-in
  [Google Search](https://ai.google.dev/gemini-api/docs/grounding) for real-time
  information
- Conversation checkpointing to save and resume complex sessions
- Custom context files (GEMINI.md) to tailor behavior for your projects

### GitHub Integration

Integrate Gemini CLI directly into your GitHub workflows with
[**Gemini CLI GitHub Action**](https://github.com/google-github-actions/run-gemini-cli):

- **Pull Request Reviews**: Automated code review with contextual feedback and
  suggestions
- **Issue Triage**: Automated labeling and prioritization of GitHub issues based
  on content analysis
- **On-demand Assistance**: Mention `@gemini-cli` in issues and pull requests
  for help with debugging, explanations, or task delegation
- **Custom Workflows**: Build automated, scheduled and on-demand workflows
  tailored to your team's needs

## 🔐 Authentication Options

Choose the authentication method that best fits your needs:

### Option 1: Sign in with Google (OAuth login using your Google Account)

**✨ Best for:** Individual developers as well as anyone who has a Gemini Code
Assist License.

**Benefits:**

- **Free tier**: 60 requests/min and 1,000 requests/day
- **Gemini 3 models** with 1M token context window
- **No API key management** - just sign in with your Google account
- **Automatic updates** to latest models

```bash
gemini
```

```bash
# Set your Google Cloud Project (for paid Code Assist License)
export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"
gemini
```

### Option 2: Gemini API Key

**✨ Best for:** Developers who need specific model control or paid tier access

```bash
# Get your key from https://aistudio.google.com/apikey
export GEMINI_API_KEY="YOUR_API_KEY"
gemini
```

### Option 3: Vertex AI

**✨ Best for:** Enterprise teams and production workloads

```bash
export GOOGLE_API_KEY="YOUR_API_KEY"
export GOOGLE_GENAI_USE_VERTEXAI=true
gemini
```

For Google Workspace accounts and other authentication methods, see the
[authentication guide](./docs/get-started/authentication.md).

## 🚀 Getting Started

### Basic Usage

```bash
# Start in current directory
gemini

# Include multiple directories
gemini --include-directories ../lib,../docs

# Use specific model
gemini -m gemini-2.5-flash

# Non-interactive
gemini -p "Explain the architecture of this codebase"

# Structured output
gemini -p "Explain the architecture of this codebase" --output-format json

# Streaming
gemini -p "Run tests and deploy" --output-format stream-json
```

### Quick Examples

```bash
# Start a new project
cd new-project/
gemini
> Write me a Discord bot that answers questions using a FAQ.md file I will provide

# Analyze existing code
git clone https://github.com/google-gemini/gemini-cli
cd gemini-cli
gemini
> Give me a summary of all of the changes that went in yesterday
```

## 📚 Documentation

### Getting Started

- [**Quickstart Guide**](./docs/get-started/index.md)
- [**Authentication Setup**](./docs/get-started/authentication.md)
- [**Configuration Guide**](./docs/reference/configuration.md)
- [**Keyboard Shortcuts**](./docs/reference/keyboard-shortcuts.md)

### Core Features

- [**Commands Reference**](./docs/reference/commands.md)
- [**Custom Commands**](./docs/cli/custom-commands.md)
- [**Context Files (GEMINI.md)**](./docs/cli/gemini-md.md)
- [**Checkpointing**](./docs/cli/checkpointing.md)
- [**Token Caching**](./docs/cli/token-caching.md)

### Tools & Extensions

- [**Built-in Tools Overview**](./docs/reference/tools.md)
  - [File System Operations](./docs/tools/file-system.md)
  - [Shell Commands](./docs/tools/shell.md)
  - [Web Fetch & Search](./docs/tools/web-fetch.md)
- [**MCP Server Integration**](./docs/tools/mcp-server.md)
- [**Custom Extensions**](./docs/extensions/index.md)

### Advanced Topics

- [**Headless Mode (Scripting)**](./docs/cli/headless.md)
- [**IDE Integration**](./docs/ide-integration/index.md)
- [**Sandboxing & Security**](./docs/cli/sandbox.md)
- [**Trusted Folders**](./docs/cli/trusted-folders.md)
- [**Enterprise Guide**](./docs/cli/enterprise.md)
- [**Telemetry & Monitoring**](./docs/cli/telemetry.md)
- [**Local development**](./docs/local-development.md)

### Troubleshooting & Support

- [**Troubleshooting Guide**](./docs/resources/troubleshooting.md)
- [**FAQ**](./docs/resources/faq.md)
- Use `/bug` command to report issues directly from the CLI.

### Using MCP Servers

Configure MCP servers in `~/.gemini/settings.json` to extend Gemini CLI with
custom tools:

```text
> @github List my open pull requests
> @slack Send a summary of today's commits to #dev channel
> @database Run a query to find inactive users
```

See the [MCP Server Integration guide](./docs/tools/mcp-server.md) for setup
instructions.

## 🤝 Contributing

We welcome contributions! Gemini CLI is fully open source (Apache 2.0), and we
encourage the community to:

- Report bugs and suggest features.
- Improve documentation.
- Submit code improvements.
- Share your MCP servers and extensions.

See our [Contributing Guide](./CONTRIBUTING.md) for development setup, coding
standards, and how to submit pull requests.

Check our [Official Roadmap](https://github.com/orgs/google-gemini/projects/11)
for planned features and priorities.

## 📖 Resources

- **[Official Roadmap](./ROADMAP.md)**
- **[Changelog](./docs/changelogs/index.md)**
- **[NPM Package](https://www.npmjs.com/package/@google/gemini-cli)**
- **[GitHub Issues](https://github.com/google-gemini/gemini-cli/issues)**
- **[Security Advisories](https://github.com/google-gemini/gemini-cli/security/advisories)**

### Uninstall

See the [Uninstall Guide](./docs/resources/uninstall.md) for removal
instructions.

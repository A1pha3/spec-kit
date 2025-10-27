# Installation Guide

## Prerequisites

- **Linux/macOS/Windows** (PowerShell scripts supported on Windows)
- AI coding agent: [Claude Code](https://www.anthropic.com/claude-code), [GitHub Copilot](https://code.visualstudio.com/), [Gemini CLI](https://github.com/google-gemini/gemini-cli), [Cursor](https://cursor.sh/), [Qwen Code](https://github.com/QwenLM/qwen-code), [opencode](https://opencode.ai/), [Windsurf](https://windsurf.com/), [Kilo Code](https://github.com/Kilo-Org/kilocode), [Auggie CLI](https://docs.augmentcode.com/cli/overview), [CodeBuddy CLI](https://www.codebuddy.ai/cli), [Roo Code](https://roocode.com/), [Codex CLI](https://github.com/openai/codex), or [Amazon Q Developer CLI](https://aws.amazon.com/developer/learning/q-developer-cli/)
- [uv](https://docs.astral.sh/uv/) for package management
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

## Installation Methods

### Method 1: One-time Usage with uvx (Recommended for Beginners)

Run directly without installing:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

### Method 2: Persistent Installation (Recommended for Regular Use)

Install once and use everywhere:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

Then use the tool directly:

```bash
specify init <PROJECT_NAME>
specify check
```

To upgrade specify run:

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

## Project Initialization

### Initialize a New Project

```bash
# Basic project initialization
specify init my-project

# Initialize in current directory
specify init .
# or use the --here flag
specify init --here
```

### Specify AI Agent

You can proactively specify your AI agent during initialization:

```bash
# Specify Claude Code
specify init my-project --ai claude

# Specify GitHub Copilot
specify init my-project --ai copilot

# Specify Gemini
specify init my-project --ai gemini

# Specify Cursor
specify init my-project --ai cursor-agent

# Specify Windsurf
specify init my-project --ai windsurf

# Specify Amp
specify init my-project --ai amp
```

### Specify Script Type (Shell vs PowerShell)

All automation scripts have both Bash (`.sh`) and PowerShell (`.ps1`) variants.

Auto behavior:
- Windows default: `ps`
- Other OS default: `sh`

Force a specific script type:

```bash
# Force Bash scripts
specify init my-project --script sh

# Force PowerShell scripts
specify init my-project --script ps
```

### Additional Options

```bash
# Skip Git initialization
specify init my-project --no-git

# Force merge into current (non-empty) directory
specify init . --force

# Enable debug output
specify init my-project --debug

# Use GitHub token for API requests
specify init my-project --github-token ghp_your_token_here

# Skip SSL/TLS verification (not recommended)
specify init my-project --skip-tls
```

## Verification

After initialization, you should see the following commands available in your AI agent:

- `/speckit.constitution` - Create project governing principles
- `/speckit.specify` - Create specifications
- `/speckit.plan` - Generate implementation plans  
- `/speckit.tasks` - Break down into actionable tasks
- `/speckit.implement` - Execute implementation

The `.specify/scripts` directory will contain both `.sh` and `.ps1` scripts.

## Troubleshooting

### Git Credential Manager on Linux

If you're having issues with Git authentication on Linux, you can install Git Credential Manager:

```bash
#!/usr/bin/env bash
set -e
echo "Downloading Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "Installing Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "Configuring Git to use GCM..."
git config --global credential.helper manager
echo "Cleaning up..."
rm gcm-linux_amd64.2.6.1.deb
```

### Common Issues

**Network Connection Problems**:
```bash
# Use offline mode
specify init my-project --offline
```

**Agent Tools Not Detected**:
```bash
# Ignore tool detection
specify init my-project --ai claude --ignore-agent-tools
```

**Script Permission Issues (Linux/macOS)**:
```bash
# Ensure scripts are executable
chmod +x .specify/scripts/*.sh
```

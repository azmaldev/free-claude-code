# 🚀 Free Claude Code Setup with Ollama

> **Use Anthropic's Claude Code agentic workflow — completely free — powered by open-source models via Ollama.**
> No Anthropic subscription. No API key. No paid plan. Just your terminal.

![Architecture Diagram](./architecture.svg)

---

## 📖 Table of Contents

- [What Is This?](#what-is-this)
- [How It Works](#how-it-works)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Cloud Models (Free, No Download)](#cloud-models-free-no-download)
- [Local Models (100% Offline, Free Forever)](#local-models-100-offline-free-forever)
- [How to Switch Models](#how-to-switch-models)
- [Free Alternatives via OpenRouter](#free-alternatives-via-openrouter)
- [Model Comparison Table](#model-comparison-table)
- [Claude Code Tips & Commands](#claude-code-tips--commands)
- [FAQ](#faq)

---

## What Is This?

**Claude Code** is Anthropic's open-source agentic coding tool. It lives in your terminal and can:

- 📂 Read your entire codebase
- ✍️ Write and edit files directly
- ▶️ Run your code and fix errors automatically
- 🧠 Plan and execute multi-step coding tasks
- 🌐 Search the web for documentation

By default it talks to Anthropic's **paid** Claude models. But here's the trick:

**Ollama** built their API to speak the exact same language as Anthropic's API. So you can redirect Claude Code → Ollama → completely free open-source models, and Claude Code has no idea it's not talking to Anthropic anymore.

```
YOU → Claude Code CLI → Ollama → Free Models (cloud or local)
                                 ↙              ↘
                          Cloud Models      Local Models
                      (Ollama's servers)  (Your machine)
                        Free w/ limits     Free forever
```

---

## How It Works

Claude Code is just a CLI tool that sends requests to any server that speaks the Anthropic Messages API format.

Ollama v0.14.0+ added full compatibility with the Anthropic Messages API. This means:

1. You install Claude Code (the free open-source CLI by Anthropic)
2. You install Ollama (the free model runner)
3. You point Claude Code at Ollama instead of Anthropic's servers
4. Claude Code works exactly the same — but powered by free open-source models

**Three environment variables are all it takes:**

```bash
ANTHROPIC_AUTH_TOKEN=ollama   # dummy value, Ollama doesn't need real auth
ANTHROPIC_API_KEY=""          # empty, no real key needed
ANTHROPIC_BASE_URL=http://localhost:11434  # points to your local Ollama
```

---

## Prerequisites

| Requirement | Version | Check |
|---|---|---|
| Node.js | v18 or higher | `node --version` |
| Ollama | v0.14.0 or higher | `ollama --version` |
| OS | Windows 10/11, macOS 11+, Linux | — |
| RAM (cloud models) | 4 GB minimum | — |
| RAM (local models) | 8–16 GB recommended | — |

---

## Installation

### Step 1 — Install Node.js

Download from [nodejs.org](https://nodejs.org) and install. Then verify:

```bash
node --version   # should show v18+
```

### Step 2 — Install Ollama

**Linux:**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**macOS:**
```bash
brew install ollama
```

**Windows** — Download the installer from [ollama.com/download/windows](https://ollama.com/download/windows) and run it. No admin rights needed.

Verify:
```bash
ollama --version
```

> **Windows users with a small C: drive:** Redirect model storage to another drive before downloading any models:
> ```powershell
> [System.Environment]::SetEnvironmentVariable("OLLAMA_MODELS", "D:\ollama\models", "User")
> ```
> Then restart PowerShell.

### Step 3 — Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Verify:
```bash
claude --version
```

---

## Cloud Models (Free, No Download)

Cloud models run on **Ollama's servers** — nothing downloads to your machine. This is the easiest way to get started.

> ⚠️ **Note:** Ollama cloud models have a free usage tier. Heavy usage may require an Ollama account. For unlimited free usage, use local models instead.

### Launch with the easiest command

```bash
ollama launch claude --model glm-5:cloud
```

That's it. One command. Claude Code opens with a free cloud model.

### All available cloud models

| Model | Command | Best For |
|---|---|---|
| GLM-5 | `ollama launch claude --model glm-5:cloud` | Long codebases, complex tasks |
| MiniMax M2.7 | `ollama launch claude --model minimax-m2.7:cloud` | Real-world engineering (top ranked) |
| Kimi K2.5 | `ollama launch claude --model kimi-k2.5:cloud` | Multi-file agentic projects |
| Qwen3.5 | `ollama launch claude --model qwen3.5:cloud` | Code generation, huge context (1M tokens) |
| DeepSeek V3.2 | `ollama launch claude --model deepseek-v3.2:cloud` | Math-heavy projects |
| Gemma 4 | `ollama launch claude --model gemma4:cloud` | Vision + coding (can read images) |
| Devstral Small 2 | `ollama launch claude --model devstral-small-2:cloud` | Fast, European privacy |
| Nemotron 3 Super | `ollama launch claude --model nemotron-3-super:cloud` | Multi-agent reasoning |

Browse all cloud models at [ollama.com/search?c=cloud](https://ollama.com/search?c=cloud)

---

## Local Models (100% Offline, Free Forever)

Local models run entirely on your machine. No internet needed after download. No usage limits. No data leaves your computer.

### Step 1 — Pull a model

Choose based on your available RAM:

| Model | Command | Download Size | Min RAM | Quality |
|---|---|---|---|---|
| qwen2.5-coder:0.5b | `ollama pull qwen2.5-coder:0.5b` | 398 MB | 4 GB | Basic |
| qwen2.5-coder:1.5b | `ollama pull qwen2.5-coder:1.5b` | 986 MB | 4 GB | Light |
| qwen2.5-coder:3b | `ollama pull qwen2.5-coder:3b` | 1.9 GB | 8 GB | Good |
| **qwen2.5-coder:7b** ⭐ | `ollama pull qwen2.5-coder:7b` | 4.7 GB | 16 GB | Recommended |
| qwen2.5-coder:14b | `ollama pull qwen2.5-coder:14b` | 9 GB | 20 GB | Strong |
| qwen3:8b | `ollama pull qwen3:8b` | 5.2 GB | 16 GB | Reasoning |

Models are saved to:
- **Linux / macOS:** `~/.ollama/models/`
- **Windows:** `C:\Users\<you>\.ollama\models\`

> **To change where models save (e.g. to a bigger drive):**
> ```bash
> # Linux / macOS
> export OLLAMA_MODELS=/path/to/big/drive
>
> # Windows PowerShell (permanent)
> [System.Environment]::SetEnvironmentVariable("OLLAMA_MODELS", "D:\ollama\models", "User")
> ```

### Step 2 — Launch Claude Code with local model

```bash
ollama launch claude --model qwen2.5-coder:7b
```

### Manage your local models

```bash
ollama list              # see all downloaded models
ollama rm modelname      # delete a model to free space
ollama pull modelname    # download a model
ollama show modelname    # see model info and size
```

---

## How to Switch Models

Switching models is just changing one word in the launch command. You can switch between cloud and local freely.

### Quick switch examples

```bash
# Using GLM-5 cloud (what most people start with)
ollama launch claude --model glm-5:cloud

# Switch to Kimi K2.5 for a multi-file project
ollama launch claude --model kimi-k2.5:cloud

# Switch to a local model for privacy / offline work
ollama launch claude --model qwen2.5-coder:7b

# Switch to Qwen3.5 for massive context (1M tokens)
ollama launch claude --model qwen3.5:cloud

# Switch to Gemma 4 to analyze an image in your project
ollama launch claude --model gemma4:cloud
```

### Make a shortcut (so you don't retype every time)

**Linux / macOS** — add to your `~/.bashrc` or `~/.zshrc`:
```bash
alias cc='ollama launch claude --model glm-5:cloud'
alias cc-kimi='ollama launch claude --model kimi-k2.5:cloud'
alias cc-local='ollama launch claude --model qwen2.5-coder:7b'
```

Reload: `source ~/.bashrc`

**Windows PowerShell** — run `notepad $PROFILE` and add:
```powershell
function cc { ollama launch claude --model glm-5:cloud }
function cc-kimi { ollama launch claude --model kimi-k2.5:cloud }
function cc-local { ollama launch claude --model qwen2.5-coder:7b }
```

Restart PowerShell. Now just type `cc` to launch.

### Always navigate to your project first

```bash
cd /path/to/your/project
ollama launch claude --model glm-5:cloud
```

Then run `/init` inside Claude Code so it scans your project.

---

## Free Alternatives via OpenRouter

[OpenRouter](https://openrouter.ai) is another way to access free models. It acts as a router between Claude Code and many different AI providers, some of which are free.

### Setup with OpenRouter

1. Create a free account at [openrouter.ai](https://openrouter.ai)
2. Get your API key from the dashboard
3. Configure Claude Code:

```bash
# Linux / macOS
export ANTHROPIC_API_KEY="your-openrouter-key"
export ANTHROPIC_BASE_URL="https://openrouter.ai/api/v1"
export ANTHROPIC_AUTH_TOKEN="your-openrouter-key"

# Then launch claude directly
claude --model openrouter/google/gemma-3-27b-it:free
```

**Windows PowerShell:**
```powershell
$env:ANTHROPIC_API_KEY = "your-openrouter-key"
$env:ANTHROPIC_BASE_URL = "https://openrouter.ai/api/v1"
claude --model openrouter/google/gemma-3-27b-it:free
```

### Free models on OpenRouter (as of April 2026)

| Model | OpenRouter ID |
|---|---|
| Gemma 3 27B | `google/gemma-3-27b-it:free` |
| Llama 3.3 70B | `meta-llama/llama-3.3-70b-instruct:free` |
| DeepSeek V3 | `deepseek/deepseek-chat:free` |
| Mistral 7B | `mistralai/mistral-7b-instruct:free` |
| Qwen3 30B | `qwen/qwen3-30b-a3b:free` |

> Check [openrouter.ai/models?q=free](https://openrouter.ai/models?q=free) for the latest free models list. It updates frequently.

### Ollama vs OpenRouter — which to use?

| | Ollama Cloud | OpenRouter Free |
|---|---|---|
| Setup | Easiest (one command) | Needs account + API key |
| Models | Ollama's curated list | 100+ models from everywhere |
| Free tier | Yes, with limits | Yes, rate limited |
| Local models | ✅ Yes | ❌ No |
| Privacy | Good | Varies by provider |
| **Best for** | Getting started fast | Accessing more model variety |

---

## Model Comparison Table

All models below are free and open-source. Benchmarks from SWE-bench Verified (real GitHub issues) and LiveCodeBench (algorithmic coding) as of April 2026.

| Model | Made By | SWE-bench | LiveCode | Context | Special Strength | Cloud Command |
|---|---|---|---|---|---|---|
| **MiniMax M2.7** 🏆 | MiniMax 🇨🇳 | **80.2%** | — | 1M | Top real-engineering score | `minimax-m2.7:cloud` |
| **Gemma 4** 🆕 | Google 🇺🇸 | — | **80.0%** | 256K | Vision + audio + code | `gemma4:cloud` |
| **GLM-5** | Zhipu AI 🇨🇳 | 77.8% | — | 200K | Long codebase generation | `glm-5:cloud` |
| **Kimi K2.5** | Moonshot 🇨🇳 | 76.8% | 85% | 256K | 100 parallel sub-agents | `kimi-k2.5:cloud` |
| **Qwen3.5** | Alibaba 🇨🇳 | 76.4% | 83.6% | **1M** | Biggest context, tool use | `qwen3.5:cloud` |
| **DeepSeek V3.2** | DeepSeek 🇨🇳 | 74.4% | 86.4% | 128K | Math + reasoning | `deepseek-v3.2:cloud` |
| **Devstral Small 2** | Mistral 🇫🇷 | — | — | 128K | Fast, EU privacy | `devstral-small-2:cloud` |
| **Nemotron 3 Super** | NVIDIA 🇺🇸 | — | — | 1M | Multi-agent tasks | `nemotron-3-super:cloud` |

### How to read benchmarks

- **SWE-bench Verified** — model is given real GitHub bug reports and must fix them. Closest to actual dev work.
- **LiveCodeBench** — algorithmic coding problems. Great for competitive-style tasks.
- Higher % = better. Claude Opus 4.6 (paid) scores 80.8% on SWE-bench for reference.

---

## Claude Code Tips & Commands

Once inside Claude Code (after launching), these are the most useful things to know:

### First thing to do in any project

```
/init
```

This scans your project folder and creates a `CLAUDE.md` file with context about your codebase. Always run this first.

### Useful slash commands

| Command | What it does |
|---|---|
| `/init` | Scan project, set up Claude Code |
| `/help` | Show all available commands |
| `/clear` | Clear conversation history |
| `/compact` | Compress context to save tokens |
| `/model` | Show current model |
| `/exit` | Exit Claude Code |

### Scheduled / automated tasks with `/loop`

```bash
# Check your PRs every 30 minutes
/loop 30m Check my open PRs and summarize their status

# Research news every hour
/loop 1h Find the latest AI news and summarize it

# Check for bugs every 15 minutes
/loop 15m Check for new GitHub issues and triage by priority
```

### Non-interactive / headless mode (for scripts and CI/CD)

```bash
ollama launch claude --model kimi-k2.5:cloud --yes -- -p "explain how this codebase works"
```

The `--yes` flag skips all prompts. Everything after `--` goes directly to Claude Code.

### Web search (built in via Ollama)

Claude Code can search the web automatically when it needs current information. No extra setup needed when using Ollama.

---

## FAQ

**Q: Is this legal / against Anthropic's terms?**
A: Yes, completely fine. Claude Code is open-source software released by Anthropic. Anthropic built it to work with any Anthropic-compatible API. Ollama built that compatibility. You are using both tools exactly as designed.

**Q: Why not just type `claude` instead of `ollama launch claude`?**
A: The `claude` command connects directly to Anthropic's paid servers and asks you to log in. `ollama launch claude` is Ollama launching the same CLI but routing it through free models. They look the same on screen but go to completely different backends.

**Q: Will I lose anything vs the paid version?**
A: You lose access to Anthropic's Claude models (Opus, Sonnet, Haiku). Everything else — the agentic workflow, file reading, code execution, multi-step planning — works exactly the same. The free open-source models are within 5-10% of Claude Opus on most coding benchmarks.

**Q: How do I know which model to pick?**
A: Start with `glm-5:cloud` (it's what most people default to). If you're working on large multi-file projects, try `kimi-k2.5:cloud`. If you need to pass images or designs to Claude Code, use `gemma4:cloud`. For maximum context (huge codebases), use `qwen3.5:cloud`.

**Q: Are cloud models private?**
A: When using Ollama cloud models, your prompts are processed on Ollama's servers. For private/proprietary code, use local models instead — nothing leaves your machine.

**Q: My C: drive is almost full. Where do models save?**
A: By default on Windows: `C:\Users\<you>\.ollama\models\`. Change it before pulling any models:
```powershell
[System.Environment]::SetEnvironmentVariable("OLLAMA_MODELS", "D:\ollama\models", "User")
```

**Q: The model is slow on my laptop. What should I do?**
A: Use a cloud model (`glm-5:cloud`) instead of a local model. Cloud models run on Ollama's servers so your hardware doesn't matter. Local models are only fast if you have a powerful NVIDIA or Apple Silicon GPU.

**Q: Can I use this in VS Code?**
A: Yes. Install the Claude Code VS Code extension and add to your `settings.json`:
```json
"claudeCode.environmentVariables": [
  { "name": "ANTHROPIC_API_KEY", "value": "not-needed" },
  { "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:11434" },
  { "name": "ANTHROPIC_MODEL", "value": "qwen2.5-coder:7b" },
  { "name": "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC", "value": "1" }
]
```

---

## Resources

- [Ollama Documentation](https://docs.ollama.com)
- [Ollama Claude Code Integration Guide](https://docs.ollama.com/integrations/claude-code)
- [All Ollama Cloud Models](https://ollama.com/search?c=cloud)
- [Ollama Model Library](https://ollama.com/library)
- [Claude Code Official Docs](https://docs.anthropic.com/en/docs/claude-code/overview)
- [OpenRouter Free Models](https://openrouter.ai/models?q=free)
- [SWE-bench Leaderboard](https://www.marc0.dev/en/leaderboard)

---

## Contributing

Found a better model? New free provider? Updated benchmark? PRs welcome.

Keep it simple. One doc. No fluff.

---

*Last updated: April 2026 | Models and benchmarks change fast — check the links above for the latest.*

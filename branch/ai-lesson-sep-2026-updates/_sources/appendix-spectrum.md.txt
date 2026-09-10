# Appendix I: The Full Spectrum of AI Coding Tools

:::{warning}
To-Do: This section needs expanding with more tools and/or kept up to date with new trends. A few tools removed their free tier recently so the lists need to be updated.
:::

:::{admonition} Beyond the three scenarios
:class: note

The main course presents three scenarios as a simplified framework for
understanding AI-assisted coding. In reality, the landscape is far more
diverse. This appendix provides a comprehensive overview of the full
spectrum of tools available, helping you explore options beyond what we
covered in the core material.
:::


## Why a spectrum, not categories?

The three scenarios in this course—chat-based, IDE-integrated, and agentic—are
useful teaching abstractions, but real-world tools often blur these boundaries:

- Some IDE plugins have agentic capabilities
- Some terminal agents are more conservative than IDE extensions
- Some tools span multiple categories depending on how you configure them
- New hybrid approaches emerge regularly

Understanding the full spectrum helps you:
- Find tools that match your specific workflow
- Evaluate new tools as they emerge
- Combine tools effectively (many practitioners use 2-3 together)


## A practical taxonomy

Rather than rigid categories, think of AI coding tools along several dimensions:

```
+----------------------+        +----------------------+        +----------------------+
|  Where you type      |        |  Where code changes  |        |  Where changes land  |
|  (UI / surface)      |        |  are produced        |        |  (collaboration)     |
+----------------------+        +----------------------+        +----------------------+
| AI-native editor     |  --->  | IDE plugin agent     |  --->  | PR / repo agent      |
| (Cursor/Windsurf)    |        | (Cline/Continue/...) |        | (issue -> PR)        |
+----------------------+        +----------------------+        +----------------------+
          |                                |                               |
          v                                v                               v
+----------------------+        +----------------------+        +----------------------+
| Terminal / CLI agent |        | Review/governance    |        | Orchestration        |
| (Claude/Codex/aider) |        | (CodeRabbit, etc.)   |        | (multi-agent mgmt)   |
+----------------------+        +----------------------+        +----------------------+
```

### Key dimensions that vary across tools

| Dimension | Range |
|-----------|-------|
| **Context source** | Editor buffer only ↔ Entire repository ↔ PR/issue metadata |
| **Action surface** | Inline suggestions ↔ Multi-file patches ↔ Full branches/PRs |
| **Safety model** | Read-only ↔ Ask before each action ↔ Autonomous execution |
| **Review loop** | Immediate inline ↔ Diff review ↔ PR review with CI |
| **Deployment** | Cloud-only ↔ Hybrid ↔ Fully local |


## Tool categories in detail

### 1. AI-native editors / IDEs

These are full editors where AI agents and chat are first-class citizens of the
interface—not add-ons, but core to the experience.

| Tool | Description | Key Features |
|------|-------------|--------------|
| [Cursor](https://cursor.com) | VS Code fork with deep AI integration | Composer for multi-file edits, Tab for inline completion, Agent mode |
| [Windsurf](https://windsurf.com/windsurf) | AI-first IDE by Windsurf (ex Codeium) | Flows for multi-step tasks, Cascade for autonomous work |

**Best for**: Developers who want AI deeply integrated into navigation,
refactoring, and the entire editing experience, all in one environment.

:::{admonition} Metaphor: Pair programming at the same workbench
:class: tip

Using an AI-native editor feels like having a navigator sitting next to you,
able to see your screen, make suggestions, and take over typing when you
describe what you want.
:::


### 2. IDE plugins and extensions

Extensions that add AI capabilities to existing IDEs (VS Code, JetBrains, etc.).
These range from simple autocomplete to full agent capabilities.

| Tool | IDE | Features | Local Model Support |
|------|-----|----------|---------------------|
| [GitHub Copilot](https://github.com/features/copilot) | VS Code, JetBrains, Neovim | Inline completion, chat, agent mode | No |
| [Continue](https://continue.dev) | VS Code, JetBrains | Chat, editing, agent workflows | Yes (Ollama) |
| [Cline](https://github.com/cline/cline) | VS Code | Agent that edits files and runs commands | Yes (Ollama) |
| [Amazon Q Developer](https://aws.amazon.com/q/developer/) | VS Code, JetBrains | Completion, chat, security scanning | No |
| [Tabnine](https://www.tabnine.com) | Multiple IDEs | Completion, chat | Yes (local models) |
| [Sourcegraph Cody](https://sourcegraph.com/cody) | VS Code, JetBrains | Chat, completion, codebase context | No |
| [JetBrains Junie](https://www.jetbrains.com/junie/) | JetBrains IDEs | Native AI agent for JetBrains | No |

**Best for**: Developers who want to stay in their familiar IDE while adding
AI capabilities incrementally.


### 3. Terminal / CLI agents

Agents you invoke from the command line that operate on local repositories,
run commands, and propose diffs. Often feel like delegating to a teammate.

| Tool | Description | Key Features |
|------|-------------|--------------|
| [Claude Code](https://docs.anthropic.com/claude/docs/claude-code) | Anthropic's terminal agent | Git-aware, sandboxing, permission system |
| [Codex CLI](https://github.com/openai/codex-cli) | OpenAI's terminal agent | Shell integration, code generation |
| [Gemini CLI](https://cloud.google.com/gemini) | Google's terminal agent | Google Cloud integration |
| [aider](https://aider.chat) | Open-source pair programming | Git integration, multiple model support, local models |
| [Open Interpreter](https://openinterpreter.com) | Open-source local agent | Runs code locally, multiple language support |

**Best for**: Developers with shell- and git-centric workflows who prefer
"delegate then review diffs" over constant inline suggestions.

:::{admonition} Metaphor: Delegation to a teammate who can roam the repo
:class: tip

Using a CLI agent feels like giving a task to a colleague: "Add authentication
to this Flask app." They go away, work on it, and come back with changes for
you to review.
:::


### 4. Repository / PR-native cloud agents

Hosted agents that take issues or tasks and return complete branches or pull
requests. They operate asynchronously and integrate with your existing PR workflow.

| Tool | Description | Key Features |
|------|-------------|--------------|
| [GitHub Copilot Workspace](https://githubnext.com/projects/copilot-workspace) | Issue-to-PR automation | Plans changes, creates PRs |
| [GitHub Copilot coding agent](https://github.blog/news-insights/product-news/github-copilot-the-agent-awakens/) | Assignable agent for issues | Works like a team member |
| [Google Jules](https://blog.google/technology/google-labs/jules/) | Asynchronous coding agent | Takes issues, returns PRs |
| [Devin](https://www.cognition-labs.com/devin) | "AI software engineer" | End-to-end task completion |
| [Replit Agent](https://replit.com/agent) | Browser-based agent | Full project generation |

**Best for**: Teams with PR-driven workflows who want issue-to-implementation
automation with CI integration and review gates.


### 5. Review and governance agents

Tools that focus on code review, security scanning, style enforcement, and
policy compliance. They operate on PRs rather than generating code.

| Tool | Description | Key Features |
|------|-------------|--------------|
| [CodeRabbit](https://coderabbit.ai) | AI-powered PR review | Security, style, documentation checks |
| [Codacy](https://www.codacy.com) | Automated code review | Quality gates, security scanning |
| [SonarQube](https://www.sonarsource.com/products/sonarqube/) | Code quality platform | Security, reliability, maintainability |

**Best for**: Teams that want consistent review standards, security enforcement,
and audit trails on all code changes.


### 6. Orchestration and multi-agent management

Dashboards and frameworks for coordinating multiple agents, managing sessions,
or running agents across multiple repositories.

| Tool | Description | Key Features |
|------|-------------|--------------|
| [GitHub Agent HQ](https://github.blog/changelog/2025-01-21-copilot-coding-agent-in-public-preview/) | Dashboard for Copilot agents | Monitor multiple agents |
| [OpenHands](https://github.com/All-Hands-AI/OpenHands) | Open-source agent framework | Multi-agent orchestration |
| [SWE-agent](https://swe-agent.com) | Research agent framework | Academic research on coding agents |

**Best for**: Organizations running multiple agents across projects who need
visibility and coordination.


### 7. Self-hosted and local-first options

For maximum privacy and control, you can run models locally and connect them
to your development environment. This is covered in detail in
{doc}`appendix-local-llms`.

**Key components:**

| Component | Examples | Role |
|-----------|----------|------|
| **Model runner** | Ollama, llama.cpp, LM Studio | Runs the model locally |
| **IDE extension** | Continue, Cline | Connects your editor to the model |
| **Model** | Qwen2.5-Coder, DeepSeek-Coder | The actual LLM |

**Why go local?**
- **Privacy**: Code never leaves your machine
- **Offline**: Works without internet
- **Cost**: No API fees after initial setup
- **Control**: Choose and customize your models

:::{admonition} See Appendix II for complete setup guide
:class: tip

{doc}`appendix-local-llms` provides step-by-step instructions for setting up
a local AI coding assistant with Ollama and Continue in VS Code, including
model recommendations and configuration examples.
:::


## Community techniques and workflows

Beyond the tools themselves, practitioners have developed reusable workflows
for making AI-assisted coding more reliable and safe.

### Context control techniques

| Technique | Description | Used by |
|-----------|-------------|---------|
| **Repo Map** | Generate a compact map of files/symbols for high-signal context | aider |
| **Memory Bank** | Structured folder of persistent project notes the agent reads/updates | Cline users |
| **Rules files** | Conventions in files the tool auto-loads (`.cursorrules`, `CLAUDE.md`) | Cursor, Claude Code |

### Reliability loops

| Technique | Description |
|-----------|-------------|
| **Spec/PRD-first** | Start from a checklist; use it as the anchor throughout |
| **Tests-as-spec** | Encode requirements as failing tests; iterate until green |
| **Micro-commits** | Small commits, frequent review, easy revert/squash |

### Safety and isolation

| Technique | Description |
|-----------|-------------|
| **Approval modes** | Default to "ask before running"; expand autonomy only when bounded |
| **Parallel worktrees** | Run multiple agents in isolated git working copies |
| **Transcript archiving** | Save sessions for auditing, sharing, and debugging |

:::{admonition} The "Ralph loop" (named community pattern)
:class: tip

A workflow pattern for Claude Code and similar agents:

1. Start with a PRD/checklist
2. Run short, fresh agent sessions (one chunk at a time)
3. Run tests/lint after each session, capture errors
4. Feed errors back to a new session
5. Commit progress after each successful chunk
6. Repeat until the PRD is complete

The key insight: **fresh sessions with persisted state** (via files/commits)
often work better than long sessions that accumulate context errors.
:::


## How to choose: A decision guide

Most practitioners end up using 2-3 tools together. Use this guide to find
your starting point.

### Pick an AI-native editor if...

- You want the agent deeply integrated into navigation and refactoring
- You prefer a single environment over mixing tools
- You value fast iteration with inline suggestions plus multi-file edits

### Pick an IDE plugin agent if...

- You already live in VS Code/JetBrains and don't want to switch
- You need strong project-context features plus agent actions
- You want to connect to local models while keeping your familiar IDE

### Pick a terminal/CLI agent if...

- Your workflow is shell- and git-centric
- You prefer "delegate then review diffs" over constant inline pairing
- You want something editor-agnostic (works from any environment)

### Add PR/repo agents and review agents if...

- You work in a PR-driven team and want issue → PR automation
- You want consistent review, security, and style enforcement
- You need a durable audit trail (PRs, CI logs)

### Consider local/self-hosted options if...

- Privacy is a primary concern (sensitive code or data)
- You want to avoid sending code to external services
- You're willing to trade some capability for full control
- Your institution restricts cloud AI services


## Operational recommendations

Regardless of which tools you choose, these defaults serve most practitioners well:

:::{admonition} Recommended operational defaults
:class: warning

1. **Start conservative**: Use approval prompts for commands and sensitive edits
2. **Keep secrets out of context**: Use env vars and secret managers, never paste keys
3. **Prefer small diffs**: Micro-commits and frequent reviews reduce risk
4. **Use CI as referee**: Run tests/lint in every loop; treat failures as feedback
5. **Isolate parallel work**: Worktrees/branches per task; avoid concurrent edits
6. **Archive decisions**: Rules files plus design notes keep future sessions grounded
:::


## Glossary

| Term | Definition |
|------|------------|
| **Agentic coding** | Tools that can plan, edit multiple files, run commands, and iterate toward a goal |
| **FIM completion** | Fill-in-the-middle code completion (insert within existing code) |
| **Local LLM** | A model served on your machine rather than a hosted API |
| **OpenAI-compatible endpoint** | A server that mimics OpenAI's API for client compatibility |
| **Worktree** | Git feature for multiple working directories tied to one repo |


## Comprehensive tool list

This is a consolidated reference of all tools mentioned in this appendix.

### AI-native editors
- [Cursor](https://cursor.com)
- [Windsurf](https://codeium.com/windsurf)

### IDE plugins and agents
- [GitHub Copilot](https://github.com/features/copilot)
- [Continue](https://continue.dev)
- [Cline](https://github.com/cline/cline)
- [Amazon Q Developer](https://aws.amazon.com/q/developer/)
- [Tabnine](https://www.tabnine.com)
- [Sourcegraph Cody](https://sourcegraph.com/cody)
- [JetBrains Junie](https://www.jetbrains.com/junie/)
- [DevoxxGenie](https://plugins.jetbrains.com/plugin/24169-devoxxgenie) (JetBrains)

### Terminal/CLI agents
- [Claude Code](https://docs.anthropic.com/claude/docs/claude-code)
- [Codex CLI](https://github.com/openai/codex-cli)
- [Gemini CLI](https://cloud.google.com/gemini)
- [aider](https://aider.chat)
- [Open Interpreter](https://openinterpreter.com)

### PR/repo agents
- [GitHub Copilot Workspace](https://githubnext.com/projects/copilot-workspace)
- [Google Jules](https://blog.google/technology/google-labs/jules/)
- [Devin](https://www.cognition-labs.com/devin)
- [Replit Agent](https://replit.com/agent)

### Review and governance
- [CodeRabbit](https://coderabbit.ai)

### Orchestration
- [OpenHands](https://github.com/All-Hands-AI/OpenHands)
- [SWE-agent](https://swe-agent.com)

### Self-hosted/local (see {doc}`appendix-local-llms` for details)
- [Ollama](https://ollama.ai)
- [Continue](https://continue.dev)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [LM Studio](https://lmstudio.ai)


## Further reading

- [OpenSSF Guide for AI Code Assistants](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
- [aider documentation](https://aider.chat/docs/) - Excellent resource on CLI agent workflows
- [Cursor documentation](https://cursor.com/features) - AI-native editor patterns
- [Continue documentation](https://continue.dev/docs) - Open-source IDE integration


:::{keypoints}
- The three scenarios in this course are simplifications; real tools exist on a spectrum
- Tools vary along dimensions: context source, action surface, safety model, review loop, deployment
- Most practitioners combine 2-3 tools for different parts of their workflow
- Local/self-hosted options exist for privacy-sensitive work
- Community techniques (rules files, micro-commits, test-driven loops) improve reliability regardless of tool choice
:::

# Scenario III: Full Agentic Code Development

**TODO this changed again in early february and needs updating...**

:::{questions}
- What are agentic coding tools and how do they differ from other approaches?
- What risks come with giving an AI agent access to your system?
- How can I use these tools while maintaining appropriate oversight?
:::

:::{objectives}
- Understand how agentic AI coding tools work
- Recognize the risks of autonomous code modification and command execution
- Learn about sandboxing and permission controls
- Develop strategies for supervising AI agents in coding tasks
:::


## The agentic scenario

In this scenario, you give an AI agent a high-level task, and it autonomously
writes code, executes commands, runs tests, and modifies files to accomplish
the goal. You basically become a project manager rather than a coder; depending
on the level of automation, you might not even see any of the code that is generated and run.


```
+--------+                              +---------------------+
|  You   |   "Add user authentication  |                     |
|        |    to this Flask app"       |    AI Agent         |
|        | --------------------------> |                     |
+--------+                              |  Plans approach     |
    ^                                   |  Reads your files   |
    |                                   |  Writes new code    |
    |  "Done. I've added login,        |  Runs tests         |
    |   registration, and session      |  Installs packages  |
    |   management. Here's what        |  Makes git commits  |
    |   I changed..."                  |                     |
    +---------------------------------- +---------------------+
                                               |
              +--------------------------------+--------------------------------+
              |                |               |               |                |
              v                v               v               v                v
        +---------+     +---------+     +---------+     +---------+     +---------+
        | Read    |     | Write   |     | Execute |     | Install |     | Git     |
        | Files   |     | Files   |     | Commands|     | Packages|     | Commit  |
        +---------+     +---------+     +---------+     +---------+     +---------+
```
**ASCII chart generated with Claude, let's make a nice block diagram**


### Why this is the highest-risk approach

1. **Autonomous execution**: Agent runs commands without per-command approval
2. **System access**: May have access to your shell, files, network
3. **Opaque decision-making**: Hard to predict what the agent will do
4. **Irreversible actions**: Deleted files, pushed commits, installed packages


## Landscape of agentic coding tools

### Claude Code (Anthropic)

**TODO: rewrite with the /plan command and mention skills and /learn commands**

A terminal-based agent that lives in your shell:

```bash
# Install
npm install -g @anthropic-ai/claude-code

# Run
claude
```

**Capabilities:**
- Reads and writes files in your project
- Executes shell commands
- Understands git workflows
- Can work across multiple files

**Safety features:**
- Sandboxing via Bubblewrap (Linux) or Seatbelt (macOS)
- Asks for permission before potentially dangerous operations
- Shows proposed changes before applying

:::{admonition} Practitioner's perspective: A real Claude Code session
:class: tip

Simon Willison documented a real project built with Claude Code—a colophon
page showing the commit history of tools he'd built. The session took 17
minutes and cost $0.61 in API calls.

His prompting sequence:
1. "Build a Python script that checks the commit history for each HTML file
   and extracts any URLs from those commit messages..."
2. [Inspected output, found issues] "It looks like it just got the start of
   the URLs, it should be getting the whole URLs..."
3. "Update the script—I want to capture the full commit messages AND the URLs..."
4. "This is working great. Write me a new script called build_colophon.py..."
5. [Got wrong output] "It's not working right. ocr.html had a bunch of
   commits but there is only one link..."
6. "It's almost perfect, but each page should have the commits displayed in
   the opposite order—oldest first"
7. "One last change—the pages are currently listed alphabetically, let's
   instead list them with the most recently modified at the top"

Key observations:
- Started with vibe-coding (didn't look at the code it wrote)
- Iterated based on observable output
- Gave specific, concrete feedback when things were wrong
- Let the agent handle implementation details

*"I was expecting that 'Test' job, but why were there two separate deploys?
[...] It was time to ditch the LLMs and read some documentation!"*

Even experienced practitioners reach points where they need to take over.
:::

### OpenAI Codex

**REDO** Erased old stuff, need to check the updates.

### Cursor (AI-native IDE)

**TODO: this below is AI generated, I don't have cursor to test this**

A VS Code-based editor with deep AI integration:

**Agent mode:**
- Plans multi-file changes
- Executes changes autonomously
- Runs up to 8 parallel agents
- Shows combined diff views

**Safety features:**
- Sandboxed terminal by default
- Shows changes before committing
- SOC 2 compliant privacy mode

### Other tools

| Tool | Type | Key Characteristic |
|------|------|-------------------|
| Aider | Terminal | Open source, multiple model support |
| GitHub Copilot Workspace | Web | PR-centric workflow |
| Continue | IDE extension | Open source, customizable |


## What can go wrong?

**TODO: add links to reddit and other horror stories**

Agentic tools can cause serious problems if not properly supervised.

### File system risks

```
Potential agent actions:
- Delete important files (rm, unlink)
- Overwrite configurations
- Modify files outside project directory
- Fill disk with generated content
```

**Real example**: Agents have been observed attempting commands like
`rm -rf /` when confused about cleanup tasks.

### Command execution risks

```
Dangerous commands an agent might run:
- curl ... | bash          (arbitrary code execution)
- pip install <package>    (supply chain risk)
- chmod 777 ...            (security weakening)
- ssh, scp                 (network access)
```

### Data exfiltration risks

```
Ways agents could leak data:
- Include in API requests to the AI service
- Execute curl/wget to external URLs
- Write to network-accessible locations
- Include in git commits pushed to public repos
```

### Package installation risks (Hallucination attacks)

AI models sometimes suggest packages that don't exist. Attackers can:
1. Monitor AI suggestions for hallucinated package names
2. Register those packages on PyPI/npm with malicious code
3. Wait for developers to install them

This is called "slopsquatting":

```
AI suggests:    pip install flask-security-utils
Reality:        This package doesn't exist... or does it now?
                An attacker may have registered it with malware
```

:::{callout} Supply chain attacks are real
The 2024 xz-utils backdoor showed how sophisticated supply chain attacks
can be. Agentic tools that install packages without verification increase
this attack surface significantly.
:::


## Permission models and controls

Different tools offer different levels of control:

### Claude Code permission system

**TODO: this needs to be updated**


Claude Code asks for permission before certain operations:

```
Claude wants to run: pip install pandas
Allow? [y/n/always/never]
```

You can configure default permissions:

```json
// .claude/settings.json
{
  "permissions": {
    "allow_file_read": true,
    "allow_file_write": "ask",
    "allow_shell_commands": "ask",
    "allow_network": false
  }
}
```

### Sandboxing levels

| Level | What it restricts | Tools |
|-------|------------------|-------|
| **None** | Full system access | Dangerous! |
| **Process** | Network, some syscalls | Bubblewrap, Seatbelt |
| **Container** | Filesystem, network | Docker |
| **VM** | Everything | Firecracker microVM |

### Practical sandboxing with Docker

Run your agent inside a container:

```bash
# Create a Dockerfile for your project
FROM python:3.11-slim

WORKDIR /project
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Run agent inside container
```

```bash
# Run with limited access
docker run -it \
  --network none \          # No network access
  --read-only \             # Read-only root filesystem
  --tmpfs /tmp \            # Writable tmp only
  -v $(pwd):/project \      # Mount project directory
  my-agent-container
```


## Supervision strategies

If you choose to use agentic tools, implement these safeguards:

### 1. Start with read-only mode

Many tools have modes where they can only suggest, not execute:

```bash
# Claude Code in plan-only mode
claude --plan-only "Add error handling to main.py"
```

### 2. Review all proposed changes

```
Agent proposes changes to 3 files:
  [M] src/main.py      (+15, -3)
  [M] src/utils.py     (+8, -0)
  [A] tests/test_main.py (+45)

Review changes? [y/n]
```

**Never skip this step.** Read every change.

### 3. Use version control religiously

```bash
# Before any agentic session
git status                    # Check clean state
git stash                     # Stash any changes
git checkout -b ai-experiment # Work on a branch

# After agent finishes
git diff                      # Review all changes
git add -p                    # Stage selectively
git commit                    # Or git reset --hard to undo
```

### 4. Set up monitoring

Watch what the agent is doing:

```bash
# In another terminal, watch file changes
watch -n 1 'git status'

# Or use a file system watcher
fswatch -r . | while read f; do echo "Modified: $f"; done
```

### 5. Limit scope

Give agents minimal access:

```
Good: "Fix the bug in parse_data() function in src/parser.py"
Bad:  "Fix all the bugs in the project"

Good: "Add a unit test for the validate_email function"
Bad:  "Set up the entire test infrastructure"
```


## When agentic tools make sense (and when they don't)

### Appropriate use cases

- **Boilerplate generation**: "Create a Flask app skeleton"
- **Test generation**: "Write unit tests for this module"
- **Documentation**: "Generate docstrings for these functions"
- **Refactoring**: "Rename this variable across all files"
- **Exploration**: "Explain how this codebase handles authentication"

### Inappropriate use cases

- **Production deployments**: Too much can go wrong
- **Database migrations**: Need human review
- **Security-critical code**: Don't trust without verification
- **Code with sensitive data**: Risk of exposure
- **Unfamiliar codebases**: You can't verify what you don't understand


## Be ready to take over

Even with excellent tools, there comes a point where human expertise is needed.

:::{admonition} Practitioner's perspective: Know when to step in
:class: warning

*"LLMs are no replacement for human intuition and experience. I've spent
enough time with GitHub Actions that I know what kind of things to look for,
and in this case it was faster for me to step in and finish the project
rather than keep on trying to get there with prompts."*

— Simon Willison

His colophon project worked, but something wasn't right: there were two
deploy jobs running. Rather than continue prompting, he read the documentation
and found a settings switch that needed changing.

**The skill is knowing when:**
- More prompting won't help
- You need to understand the underlying system
- Reading documentation is faster than iterating
- The AI is confidently going in the wrong direction
:::

The most effective approach combines AI speed with human oversight—not
blind trust in either direction.


## Exercises

:::{exercise} Exercise Agent-1: Explore an agent (read-only)
If you have access to an agentic tool (Claude Code, Cursor, etc.):

1. Start it in read-only or plan-only mode
2. Give it a task: "Explain the structure of this project"
3. Observe:
   - What files does it try to read?
   - What is its reasoning process?
   - How does it present findings?
4. Do NOT allow it to make any changes yet

This helps you understand how the agent "thinks" before giving it write access.
:::

:::{solution}
Key observations to make:
- The agent typically starts by reading project structure (ls, tree)
- It then reads key files (README, main entry points, config)
- It may make assumptions that are wrong about your project
- Its explanation reveals its understanding (which may have gaps)

This exploration builds trust incrementally before allowing modifications.
:::

:::{exercise} Exercise Agent-2: Sandboxed experiment
Set up a sandboxed environment to safely experiment with agentic tools:

1. Create a test project directory with some simple Python files
2. Initialize a git repository
3. If using Docker:
   ```bash
   docker run -it --network none -v $(pwd):/project python:3.11 bash
   ```
4. If not using Docker, at minimum:
   - Work in a dedicated directory
   - Keep version control active
   - Don't give the agent access to sensitive directories

5. Try giving the agent a simple task and observe its behavior
:::

:::{solution}
The exercise teaches:
- How to isolate experiments from important data
- The importance of version control as a safety net
- How agents behave when they have limited access
- The difficulty of truly sandboxing modern tools (they often need network access to function)
:::

:::{exercise} Exercise Agent-3: Permission audit
For an agentic tool you're considering:

1. Find its documentation on permissions and data handling
2. Answer these questions:
   - What data is sent to remote servers?
   - Can it be configured to work locally?
   - What system resources can it access?
   - How does it handle credentials in your project?
   - Is there an audit log of actions taken?

3. Based on your findings, what safeguards would you implement before using it?
:::

:::{solution}
This exercise builds security awareness. Key findings typically include:
- Most agentic tools require network access to the AI provider
- Few offer fully local operation
- Audit logs are often incomplete
- Credential handling varies widely
- Documentation may not cover all data flows

Safeguards should include version control, network monitoring, and credential isolation.
:::


## Comparison: When to use each scenario

| Factor | Scenario I (Chat) | Scenario II (IDE) | Scenario III (Agent) |
|--------|-------------------|-------------------|----------------------|
| **User control** | Full | Moderate | Limited |
| **Speed** | Slow | Fast | Fastest |
| **Visibility** | Complete | Partial | Low |
| **Risk level** | Low | Medium | High |
| **Best for** | Learning, design | Routine coding | Boilerplate, refactoring |
| **Avoid for** | Large volumes | Sensitive code | Production systems |


## Summary

Agentic coding tools offer the most automation but also the highest risk:

- They can read files, execute commands, and modify your codebase
- Risks include data exposure, malicious packages, and destructive actions
- Sandboxing and permission controls provide some protection
- Version control is your essential safety net
- Always review all changes before accepting them

The question isn't "Should I ever use agentic tools?" but rather "For which
tasks, with what safeguards, and how much supervision?"


## See also

- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Cursor Documentation](https://cursor.com/features)
- [Docker Security Best Practices](https://docs.docker.com/develop/security-best-practices/)
- [Bubblewrap (sandboxing tool)](https://github.com/containers/bubblewrap)
- [OpenSSF Guide for AI Code Assistants](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
- [Simon Willison: Claude Code example with transcript](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#a-detailed-example-using-claude-code) - Real-world agentic session
- [DataNorth: Claude Code Complete Guide](https://datanorth.ai/blog/claude-code-ai-coding-assistant-guide-2025) - Comprehensive comparison with other tools


:::{keypoints}
- Agentic tools can read, write, and execute without per-action approval
- Risks include file deletion, data exposure, and supply chain attacks
- Use sandboxing (Docker, Bubblewrap) to limit agent access
- Always use version control and review all changes
- Match tool autonomy to task risk, use agents for low-stakes tasks
:::

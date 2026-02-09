# Scenario II: IDE Integration (Tab Completion and Inline Suggestions)

:::{questions}
- How do IDE-integrated AI assistants like GitHub Copilot work?
- What information is sent to remote servers when I use these tools?
- How can I configure these tools to balance productivity and privacy?
:::

:::{objectives}
- Understand how IDE-integrated AI assistants process your code
- Learn to configure GitHub Copilot and similar tools
- Recognize the reduced transparency compared to chat-based approaches
- Develop habits for reviewing inline suggestions critically
:::


## The IDE integration scenario

In this scenario, AI assistance is integrated directly into your development
environment. As you type, the AI suggests completions, entire functions, or
even multi-line code blocks.

```
+----------------------------------------------------+
|  Your IDE (VS Code, JetBrains, etc.)               |
|                                                    |
|  +----------------------------------------------+  |
|  | def calculate_mean(numbers):                 |  |
|  |     """Calculate the arithmetic mean."""     |  |
|  |     return sum(numbers) / len(numbers)       |  | <-- AI suggests this
|  |            ~~~~~~~~~~~~~~~~~~~~~~~~~~~~      |  |     as you type
|  |            [Tab to accept] [Esc to dismiss]  |  |
|  +----------------------------------------------+  |
|                                                    |
+------------------------+---------------------------+
                         |
                         | Code context sent
                         | automatically
                         v
              +----------+----------+
              |   Remote AI Server  |
              |                     |
              |  - Processes your   |
              |    file contents    |
              |  - Returns          |
              |    suggestions      |
              +---------------------+
```

**ASCII diagram generated with claude, we should use screenshots here**


### How this differs from chat-based coding

| Aspect | Chat-based (Scenario I) | IDE-integrated (Scenario II) |
|--------|------------------------|------------------------------|
| **Data flow** | You explicitly paste code | Context sent automatically |
| **Visibility** | You see exactly what you share | Less clear what's transmitted |
| **Execution** | Manual copy/paste | Still manual, but faster |
| **Context** | Limited to what you provide | May include entire project |


## What information leaves your machine?

When you use IDE-integrated tools, data transmission happens automatically
and continuously. Understanding what's sent is crucial.

### Typically transmitted data

| Data Type | Purpose | Privacy Concern |
|-----------|---------|-----------------|
| Current file content | Generate relevant suggestions | May contain sensitive code |
| Open file tabs | Broader context | More exposure |
| File paths | Understand project structure | Reveals project organization |
| Cursor position | Know what to complete | Minimal concern |
| Recent edits | Understand your intent | Sent frequently |

### What may be transmitted (tool-dependent)

- Other files in your workspace
- Git history
- Comments and docstrings
- Import statements (revealing your dependencies)

:::{callout} Read your tool's documentation
Each tool has different data collection policies. For example:
- GitHub Copilot transmits code context to GitHub/Microsoft servers
- Some tools offer "local only" models (e.g., Tabnine)
- Enterprise tiers may offer data isolation guarantees
:::


## Setting up GitHub Copilot

**THIS MIGHT HAVE CHANGED IN EARLY FEBRUARY, IT NEEDS VERIFICATION PLUS LINKS**


GitHub Copilot is currently the most widely-used IDE-integrated assistant.
Here's how to set it up with privacy and control in mind.

### Installation in VS Code

1. Install the "GitHub Copilot" extension from the marketplace
2. Sign in with your GitHub account
3. If you have an educational (.edu) email, you may qualify for free access

### Key configuration settings

Open VS Code settings (Ctrl+, or Cmd+,) and search for "Copilot":

```json
{
    // Disable automatic suggestions (require manual trigger)
    "github.copilot.enable": {
        "*": true,
        "markdown": false,  // Disable for documentation
        "plaintext": false  // Disable for plain text files
    },

    // Require explicit trigger instead of automatic suggestions
    "editor.inlineSuggest.enabled": true,

    // Control which files Copilot can access
    "github.copilot.advanced": {
        "indentationMode": {
            "python": true,
            "javascript": true
        }
    }
}
```

### Disabling for sensitive files

You can create a `.copilotignore` file in your project root:

```
# Don't process these files
.env
secrets.py
config/credentials.yaml
data/*  # Ignore data directory
```

:::{discussion}
**When to disable AI suggestions**

Consider disabling inline AI for:
- Files containing credentials or API keys
- Proprietary algorithms
- Files with sensitive personal data
- Configuration files with internal infrastructure details
:::


## The hidden context problem

A key difference from chat-based AI is that you don't explicitly control
what context the model receives. This has implications:

### Example: Unintended context sharing

```
+------------------+     +------------------+     +------------------+
|   main.py        |     |   config.py      |     |   secrets.env    |
|   (you're here)  |     |   (open tab)     |     |   (open tab!)    |
+------------------+     +------------------+     +------------------+
         |                       |                       |
         +-----------------------+-----------------------+
                                 |
                                 v
                    All may be sent as context
```

**The issue**: You might have a secrets file open in another tab, and parts
of it could be included in the context sent to the AI server.

### Mitigation strategies

1. **Close sensitive tabs** when using AI completion
2. **Use `.copilotignore`** to exclude sensitive files
3. **Consider workspace separation** - keep sensitive projects in different VS Code windows
4. **Review your open tabs** before intensive AI-assisted sessions


## Features beyond simple completion

Modern IDE integrations offer more than tab completion:

### Inline chat

Many tools now allow you to chat with the AI directly in your editor:
- Select code and ask "explain this"
- Ask to refactor or improve selected code
- Generate tests for highlighted functions

### Automated multi-file edits

Some tools (like Cursor, Copilot Workspace) can:
- Edit multiple files simultaneously
- Apply changes across your codebase
- Suggest refactoring patterns

:::{callout} Increased risk with automated edits
When tools can modify multiple files:
- Changes may have unintended consequences
- Harder to review all modifications
- Version control becomes essential

**Always commit before automated multi-file operations.**
:::


## Developing critical review habits

With suggestions appearing automatically, it's easy to accept them without
sufficient review. Build these habits:

### The 3-second rule

**TODO ENrico has a reference for this**

Before pressing Tab to accept:
1. **Read** the entire suggestion
2. **Ask** yourself: "Does this match my intent?"
3. **Check** for obvious issues (wrong variable names, edge cases)

### Watch for common issues

| Issue | Example | How to Catch |
|-------|---------|--------------|
| Wrong variable | `return total / len(items)` when you named it `data` | Check names match your code |
| Missing edge case | No check for empty list | Ask yourself about inputs |
| Outdated API | Using deprecated function | Check documentation |
| Wrong assumption | Assuming sorted input | Read comments critically |

### Accept-then-review workflow

Some developers find it faster to:
1. Accept the suggestion
2. Immediately review line-by-line
3. Delete and redo if wrong

This works but requires discipline. Don't skip step 2.


## Alternative tools: Codeium and Tabnine

GitHub Copilot isn't your only option. Here's how alternatives compare:

### Codeium

- **Pricing**: Free core features
- **Privacy**: Claims no training on your code
- **Features**: 70+ languages, chat interface, Windsurf IDE

Setup in VS Code:
1. Install "Codeium" extension
2. Create account at codeium.com
3. Authenticate in VS Code

### Tabnine

- **Pricing**: Free basic tier, paid for advanced
- **Privacy**: Offers local-only models (no cloud transmission)
- **Features**: Learns from your codebase, SOC 2 compliant

Setup in VS Code:
1. Install "Tabnine" extension
2. Create account
3. Choose cloud or local mode

### Comparison for research settings

| Factor | Copilot | Codeium | Tabnine (Local) |
|--------|---------|---------|-----------------|
| Privacy | Cloud-based | Cloud-based | Can run locally |
| Quality | Excellent | Good | Good |
| Cost | $10-19/mo | Free | Free basic |
| Institutional acceptance | Varies | Varies | Often preferred |


## Exercises

:::{exercise} Exercise IDE-1: Configure your tool
Set up GitHub Copilot (or Codeium/Tabnine) and:

1. Create a `.copilotignore` or equivalent to exclude:
   - Any `.env` files
   - A `secrets/` directory
   - Data files (`*.csv`, `*.json` in `data/`)

2. Find the setting to disable suggestions for Markdown files

3. Test that suggestions work in a Python file but not in the excluded files
:::

:::{solution}
Example `.copilotignore`:
```
# Sensitive files
.env
.env.*
secrets/
credentials.*

# Data files
data/
*.csv
*.json
*.parquet

# Documentation (disable AI here)
*.md
docs/
```

In VS Code settings:
```json
{
    "github.copilot.enable": {
        "*": true,
        "markdown": false
    }
}
```
:::

:::{exercise} Exercise IDE-2: Critical suggestion review
With AI suggestions enabled, type the following function signature and
observe the suggestions:

```python
def validate_email(email_string):
    """Check if a string is a valid email address."""
```

1. Accept the suggestion
2. Review it critically:
   - Does it use regex or simple string checks?
   - Does it handle all valid email formats?
   - Is it too strict or too lenient?
3. Search online for email validation edge cases
4. Ask yourself: is the AI-generated solution appropriate for your use case?
:::

:::{solution}
Common issues with AI-generated email validation:
- May reject valid emails (e.g., with + signs, subdomains)
- May accept invalid emails (missing TLD validation)
- Regex might be overly complex or overly simple

The key lesson: "valid email" is surprisingly complex, and AI suggestions
reflect common patterns, not necessarily correct patterns.

For many applications, using a well-tested library (like Python's
`email-validator`) is better than AI-generated regex.
:::


:::{exercise} Exercise IDE-3: Context awareness experiment
Test what context your AI tool uses:

1. Open two Python files in VS Code:
   - `main.py`: Empty except for a function signature
   - `helpers.py`: Contains a utility function

2. In `helpers.py`, write:
   ```python
   def calculate_tax(amount, rate=0.21):
       """Calculate tax amount for the Netherlands."""
       return amount * rate
   ```

3. In `main.py`, start typing:
   ```python
   def process_invoice(amount):
       tax =
   ```

4. Does the AI suggest calling `calculate_tax`?
5. What does this tell you about cross-file context?
:::

:::{solution}
If the AI suggests `calculate_tax`, it demonstrates that:
- Your open files are being used as context
- The AI understands relationships between files
- This is useful but also means sensitive code in open tabs could inform suggestions

This experiment shows why closing sensitive files matters when using
IDE-integrated AI tools.
:::


## When to use IDE integration vs. chat

| Use IDE integration when | Use chat-based when |
|--------------------------|---------------------|
| Writing boilerplate code | Designing architecture |
| Completing obvious patterns | Debugging complex issues |
| Speed matters, risk is low | You need to control context carefully |
| Working on non-sensitive code | Learning a new concept |


## Summary

IDE-integrated AI assistants offer faster workflows than chat-based
approaches, but with reduced visibility into what data is shared:

- Context is transmitted automatically
- You must proactively configure exclusions
- Critical review habits are essential
- Alternative tools offer different privacy trade-offs

The key question is: **Do you know what's being sent to the AI server?**
If you're not sure, either find out or switch to a more transparent approach.


## See also

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Codeium Documentation](https://codeium.com/documentation)
- [Tabnine Documentation](https://docs.tabnine.com/)
- [VS Code AI Extensions Comparison](https://marketplace.visualstudio.com/search?term=ai%20code&target=VSCode)


:::{keypoints}
- IDE-integrated AI sends code context automatically to remote servers
- Use configuration files (`.copilotignore`) to exclude sensitive files
- Always review suggestions before accepting, even if they look correct
- Consider local-model alternatives (Tabnine) for sensitive projects
- Close sensitive tabs when working with AI-assisted completion
:::

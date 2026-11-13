# Quick Reference

A condensed reference for the lesson on responsible AI-assisted coding.


## The three scenarios

| Scenario | Control | Risk | Best for |
|----------|---------|------|----------|
| I: Chat | Full | Low | Learning, design, sensitive work |
| II: IDE integration | Medium | Medium | Routine coding, boilerplate |
| III: Agentic | Limited | High | Refactoring, test generation |


## Security checklist

### Before any AI interaction
- [ ] Remove credentials from files
- [ ] Use synthetic data, not real data
- [ ] Close sensitive tabs (IDE integration)

### When AI suggests a package
```bash
# Verify it exists before installing
pip index versions <package-name>
# Or, even better, check https://pypi.org/project/<package-name>
```

### When AI generates code
- [ ] Read and understand all code
- [ ] Check for injection vulnerabilities (SQL, command, path)
- [ ] Test edge cases (e.g.: empty input, None, negative numbers)
- [ ] Run security linter like [Bandit](https://github.com/PyCQA/bandit): `bandit -r your_code/`


## Effective prompting

### Two modes of prompting

| Mode | When | How |
|------|------|-----|
| **Exploration** | Starting, researching | "What are options for doing X?" |
| **Planning** | Designing a stable workflow | "I want to do task X using Y and Z, please plan each script and function needed for designing the workflow." |
| **Production** | Implementing | Provide exact function signatures |

### Do: Be specific with function signatures
```
Write a Python function with this signature:

def validate_email(address: str) -> bool:
    """Validate email using a well-tested library.
    Return True if valid, False otherwise.
    Raise TypeError if address is not a string."""
```

### Don't: Be vague
```
"Write code to handle emails"
```

### Structure of prompts for coding
1. Context (programming language, libraries, environment)
2. Task (what you want)
3. Constraints (what to avoid, requirements)
4. Format (documentation, tests, explanations)

### Remember: It's a conversation
- First response is a starting point, not the answer
- Iterate: "Break that out into a function", "Split this long script into independent units"
- Ask follow-ups: "What could go wrong with this?"


## IDE integration settings

### VS Code settings.json

Example to make sure some configurations yaml files are not accessed by copilot (uncertain if copilot respects this).

```json
{
	"github.copilot.enable": {
  		"*": true,          // enable Copilot for all languages by default
  		"yaml": false,      // disable for YAML files
		"plaintext": false  // disable for plain text files
	}
}
```

### .copilotignore
```gitignore
.env
.env.*
secrets/
credentials.*
*.pem
*.key
data/
```


## Sandboxing commands

### Docker (basic isolation)

TODO: Needs testing on multiple systems

Gets a basic python container and starts bash:

```bash
docker run -it \
    --network none \
    --read-only \
    --tmpfs /tmp \
    -v $(pwd):/project \
    python:3.11 bash
```

### Bubblewrap (Linux)

TODO: Needs testing and limitations on where it works

Limits the command "your_command" to certain places in your Linux computer.

```bash
bwrap \
    --ro-bind /usr /usr \
    --ro-bind /lib /lib \
    --bind $(pwd) $(pwd) \
    --unshare-net \
    your_command
```


## Common vulnerabilities in AI code

TODO: this list is not exhaistive. We should add more links / further readings.

### SQL Injection
```python
# Bad (AI often generates this)
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")

# Good
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

### Path Traversal
```python
# Bad
open(f"/data/{filename}")

# Good
from pathlib import Path
path = (Path("/data") / filename).resolve()
if not path.is_relative_to(Path("/data")):
    raise ValueError("Invalid path")
```

### Command Injection
```python
# Bad
os.system(f"ping {hostname}")

# Good
subprocess.run(["ping", "-c", "1", hostname], check=True)
```


## Package verification script

TODO: this was generated with Claude. To be tested.

```python
import requests

def verify_package(name):
    """Check if a package exists on PyPI."""
    r = requests.get(f"https://pypi.org/pypi/{name}/json")
    if r.status_code == 404:
        print(f"WARNING: {name} not found!")
        return False
    info = r.json()["info"]
    print(f"Found: {name}")
    print(f"  Author: {info.get('author')}")
    print(f"  Home: {info.get('home_page')}")
    return True
```


## Resources

- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OpenSSF AI Code Assistant Guide](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
- [Bandit security linter](https://bandit.readthedocs.io/)
- [PyPI package search](https://pypi.org/)
- [Simon Willison: How I use LLMs for code](https://simonwillison.net/2025/Mar/11/using-llms-for-code/)
- [Addy Osmani: My LLM coding workflow](https://addyosmani.com/blog/ai-coding-workflow/)

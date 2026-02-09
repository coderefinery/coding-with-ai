# Scenario I: Full Control (Chat-Based Coding)

:::{questions}
- How can I use generative AI chatbots effectively for coding tasks?
- What are the risks and benefits of manual copy/paste workflows?
- What prompting strategies work best for research code?
:::

:::{objectives}
- Learn effective prompting techniques for code generation
- Understand what information is shared when using chatbots
- Develop a modular approach to building research pipelines with AI assistance
- Practice critical evaluation of AI-generated code
:::


## The full control scenario

In this scenario, you interact with an AI assistant through a web interface
(like ChatGPT, Claude, or Gemini) and manually copy code between the chatbot
and your development environment.

```
+------------------+                    +------------------+
|                  |   Your prompts     |                  |
|    AI Chatbot    | <----------------- |       You        |
|    (Web UI)      |                    |                  |
|                  | -----------------> |                  |
+------------------+   Generated code   +--------+---------+
                                                 |
                                        manual   |
                                       copy/paste|
                                                 v
                                        +--------+---------+
                                        |    Your IDE      |
                                        | (VS Code/Jupyter)|
                                        |                  |
                                        | - Review code    |
                                        | - Run manually   |
                                        | - Debug yourself |
                                        +------------------+
```
**ASCII chart generated with Claude, it should be replaced with a block diagram**

### Why this is the lowest-risk approach

1. **You see everything**: Every piece of code goes through your eyes and clipboard
2. **Nothing runs automatically**: You decide when and how to execute code
3. **Clear boundaries**: The AI cannot access your files, run commands, or modify anything
4. **Explicit data sharing**: You control exactly what context the AI receives


## What information leaves your machine?

When you use a chat interface, the following is typically sent to the provider's servers:

| What you send | Considerations |
|---------------|----------------|
| Your prompts | May contain sensitive project details |
| Code you paste | Could include proprietary algorithms |
| Error messages | May reveal system configuration, usernames, local paths, filenames |
| Data samples | Never paste real research data!  |

:::{callout} Data handling policies vary
Different providers have different policies:
- Some use your conversations to train future models
- Some offer enterprise tiers with data isolation (e.g. Microsoft Azure)
- Read the terms of service for your chosen tool
- When in doubt, assume your input may be retained
- Most providers still retain all your activities for 30 days, even if you opted out.
:::

### What NOT to share

- Real research data (use synthetic examples instead)
- API keys, passwords, or credentials
- Proprietary algorithms or research/trade/IPR secrets
- Patient/participant identifiable information
- Unpublished results or embargoed data


## Three modes of AI-assisted coding

Effective AI-assisted coding involves switching between three distinct modes
depending on where you are in the development process.

### Exploration mode: Ask for options

When starting a project or evaluating approaches, use the AI for research:

```
"What are options for parsing XML in Python? Include pros/cons of each."

"What approaches could I use for parallelizing this computation?"

"What are some useful drag-and-drop libraries in JavaScript?
Build me an example demonstrating each one."
```

This helps you understand the landscape before committing to an approach.
The AI's training cut-off means newer libraries won't be suggested, but
for established options this is often fine—you want stability anyway.


### Planning mode: Design the workflow

Once you have explored your options, good planning makes sure you have the workflow sketched out,
dependencies are considered, and your system has the right setup for starting the actual work
(folder strcuture, version control, environment and other dependencies).

If the first stage was about brainstorming, this is about project management. A good well written plan
makes it easier later for adding or removing components in your workflow. This is the equivalent
of writing down all the comments, before writing your code.

Example of useful prompts:

**TODO ENRICO TO FINISH THIS**




### Production mode: Tell them exactly what to do

Once you've decided on an approach, switch to authoritarian prompting.
Provide precise specifications:

```
Write a Python function with this signature:

def validate_participant_id(pid: str) -> bool:
    """
    Validate a participant ID matches our format: P followed by 3 digits.

    Parameters
    ----------
    pid : str
        The participant ID to validate

    Returns
    -------
    bool
        True if valid, False otherwise

    Raises
    ------
    TypeError
        If pid is not a string
    """
```

You act as the function designer; the AI implements to your specification.

:::{admonition} Practitioner's perspective: Function signatures as prompts
:class: tip

*"I find LLMs respond extremely well to function signatures. I get to act as
the function designer, the LLM does the work of building the body to my
specification.*

*If your reaction to this is 'surely typing out the code is faster than
typing out an English instruction of it', all I can tell you is that it
really isn't for me any more. Code needs to be correct. English has enormous
room for shortcuts, and vagaries, and typos, and saying things like 'use
that popular HTTP library' if you can't remember the name off the top of
your head.*

*The good coding LLMs are excellent at filling in the gaps. They're also
much less lazy than me—they'll remember to catch likely exceptions, add
accurate docstrings, and annotate code with the relevant types."*

— Simon Willison
:::


## Managing context effectively

The key skill in AI-assisted coding is managing **context**—the accumulated
text in your conversation that influences the AI's responses.

### What goes into context

| Element | Impact |
|---------|--------|
| Your prompts | Directly shape the response |
| AI's previous responses | Become part of the "memory" |
| Code you paste | Provides examples and patterns |
| Error messages shared | Help with debugging |

**TODO insert a box about context windows in popular AI systems, also talk about "memory" features which can influence the context**


### Practical context management

**Tips for context management:**

1. **Start fresh when stuck**: If a conversation isn't productive, begin a new one
2. **Build iteratively**: Get simple versions working, then add complexity
3. **Seed with examples**: Paste working code as context for new but similar tasks
4. **Provide multiple examples**: "Here are three similar functions I've written.
   Use them as inspiration for this new one."


## Effective prompting strategies

The quality of AI-generated code depends heavily on how you ask for it.

**TODO: this is a collection of promopts in no particilar order from an initial list expanded by claude. It needs refining, citations, sorting, what works well...**



### Strategy 1: Start with architecture, not implementation

Instead of asking for 500 lines of code at once, begin with structure:

**Poor approach:**
> "Write a complete Python script to analyze my fMRI data, including
> preprocessing, statistical analysis, and visualization."

**Better approach:**
> "I need to build a pipeline for fMRI analysis. What would be a good
> modular structure? I'm thinking separate functions for:
> 1. Loading data
> 2. Preprocessing
> 3. Statistical analysis
> 4. Visualization
>
> Can you outline what each module should do and what the interfaces
> between them should look like?"

### Strategy 2: Provide context about your environment

Help the AI understand your constraints:

```
I'm working on:
- Python 3.11
- Ubuntu 22.04
- Using NumPy, SciPy, and Matplotlib (prefer not to add new dependencies)
- Data is in HDF5 format

I need a function to...
```

### Strategy 3: Ask for explanations, not just code

```
Write a function to perform bootstrap resampling. Please:
1. Explain the statistical principle first
2. Show the implementation
3. Include comments explaining each step
4. Note any assumptions the code makes
```

### Strategy 4: Iterate incrementally

```
Session flow:
1. "Write a function to load CSV data with error handling"
2. [Test it, find an issue]
3. "This fails when the file has a BOM. Can you fix that?"
4. [Works now]
5. "Now let's add support for gzipped files..."
```

:::{discussion}
**Why modular development matters even more with AI**

When you generate code in small, testable pieces:
- You can verify each piece works before moving on
- Errors are easier to localize and fix
- You maintain understanding of what each part does
- The AI has clearer context for each request
:::

### Strategy 5: Remember it's a conversation

The first response is rarely the final answer. Use follow-up prompts freely:

- "Break that repetitive code out into a function"
- "Use string manipulation methods rather than a regular expression"
- "Write that better!"
- "Now write tests for this using pytest"
- "Can you explain what line 15 does?"


## Building a research pipeline: Worked example

Let's walk through how to build a simple analysis pipeline using the
chat-based approach.

### Step 1: Define the task

> "I have a CSV file with columns: participant_id, condition, reaction_time, accuracy.
> I need to:
> 1. Load and validate the data
> 2. Calculate summary statistics per condition
> 3. Run a statistical test comparing conditions
> 4. Create a visualization
>
> Let's start with step 1. Write a Python function to load the CSV and
> validate that all expected columns are present."

### Step 2: Review and test

The AI might respond with:

```python
import pandas as pd

def load_and_validate_data(filepath):
    """
    Load experimental data and validate required columns.

    Parameters
    ----------
    filepath : str
        Path to the CSV file

    Returns
    -------
    pd.DataFrame
        Validated dataframe

    Raises
    ------
    ValueError
        If required columns are missing
    """
    required_columns = {'participant_id', 'condition', 'reaction_time', 'accuracy'}

    df = pd.read_csv(filepath)

    missing = required_columns - set(df.columns)
    if missing:
        raise ValueError(f"Missing required columns: {missing}")

    return df
```

**Your review checklist:**
- Does it handle the file not existing?
- What about encoding issues?
- Is the error message helpful?

### Step 3: Iterate based on testing

> "This works, but I'd like to add:
> 1. Check for missing values in critical columns
> 2. Validate that reaction_time is positive
> 3. Log warnings instead of failing for recoverable issues"

### Step 4: Continue to next module

Once satisfied with data loading, move to summary statistics:

> "Great, that function is working. Now write a function that takes
> the validated dataframe and returns summary statistics (mean, std, n)
> for reaction_time and accuracy, grouped by condition."


## Common pitfalls and how to avoid them

### Pitfall 1: Accepting code without understanding it

**Problem**: You paste code and it works, but you don't understand why.

**Solution**: Always ask "Can you explain how this works?" If you can't
explain the code to a colleague, you shouldn't use it.

### Pitfall 2: Not testing edge cases

**Problem**: Code works for your test case but fails on real data.

**Solution**: Explicitly ask about edge cases:
> "What edge cases should I test for this function? What inputs might break it?"

### Pitfall 3: Accumulating technical debt

**Problem**: Quick solutions that are hard to maintain long-term.

**Solution**: Ask for production-quality code:
> "Write this function following best practices: include docstrings,
> type hints, and error handling."

### Pitfall 4: Hallucinated packages or functions

**Problem**: AI suggests importing a package that doesn't exist.

**Solution**: Before running `pip install`, verify the package exists:
- Check on [PyPI](https://pypi.org/)
- Search GitHub for the repository
- Be especially suspicious of packages you've never heard of


## When to relax the rules: Learning and exploration

The careful, verify-everything approach described above is essential for
production code. But for **learning and exploration**, a looser approach
can accelerate your understanding of what AI can do.

:::{admonition} Practitioner's perspective: Vibe-coding for learning
:class: tip

Andrej Karpathy coined the term "vibe-coding" for a more relaxed approach:

*"There's a new kind of coding I call 'vibe coding', where you fully give in
to the vibes, embrace exponentials, and forget that the code even exists.
[...] I ask for the dumbest things like 'decrease the padding on the sidebar
by half' because I'm too lazy to find it. I 'Accept All' always, I don't
read the diffs anymore."*

Simon Willison's take:

*"Andrej suggests this is 'not too bad for throwaway weekend projects'. It's
also a fantastic way to explore the capabilities of these models—and really
fun. The best way to learn LLMs is to play with them. Throwing absurd ideas
at them and vibe-coding until they almost sort-of work is a genuinely useful
way to accelerate the rate at which you build intuition for what works and
what doesn't."*
:::

**When vibe-coding is appropriate:**
- Learning a new library or framework
- Prototyping ideas quickly
- Building throwaway demonstrations
- Exploring what's possible

**When to be rigorous:**
- Research code that will be published
- Code that processes real data
- Anything that will be shared or maintained
- Security-sensitive applications

:::{warning}
The distinction matters: vibe-coding is for **learning**, not for
**production**. Don't let the ease of accepting suggestions lead you to
deploy code you don't understand.
:::


## Using AI to understand existing code

One of the lowest-risk, highest-value use cases for AI: asking it to explain
code you didn't write.

### Why this is valuable for researchers

- Onboarding to inherited codebases
- Understanding library internals
- Code review preparation
- Learning from others' implementations

### Workflow for code understanding

1. **Paste the code** (or relevant portions) into a chat
2. **Ask for an overview**: "Give me an architectural overview of this code"
3. **Drill down**: "Explain what this specific function does"
4. **Identify issues**: "What could go wrong with this implementation?"

:::{admonition} Practitioner's perspective: Questions are low-stakes
:class: tip

*"If the idea of using LLMs to write code for you still feels deeply
unappealing, there's another use-case for them which you may find more
compelling. Good LLMs are great at answering questions about code.*

*This is also very low stakes: the worst that can happen is they might get
something wrong, which may take you a tiny bit longer to figure out. It's
still likely to save you time compared to digging through thousands of lines
of code entirely by yourself.*

*The trick here is to dump the code into a long context model and start
asking questions."*

— Simon Willison
:::

### Example prompts for code understanding

```
"What is the main purpose of this module?"

"Trace the data flow from input to output."

"What external dependencies does this code have?"

"Where might this code fail with unexpected input?"

"Explain the algorithm in step 3 of this function."
```

This is a great starting point for researchers who are hesitant about
AI-generated code—you remain in full control, and you're using the AI
to augment your understanding rather than replace your work.


## Exercises

:::{exercise} Exercise Chat-1: Build a data loader
Using [duck.ai](https://duck.ai) (or your preferred AI chatbot):

1. Ask it to write a function that loads JSON files containing experimental
   results with this structure:
   ```json
   {"participant": "P01", "scores": [85, 90, 78], "completed": true}
   ```

2. Ask it to add validation for:
   - Required fields
   - Score values between 0-100
   - Proper data types

3. Test the resulting function with valid and invalid inputs

4. Ask the AI: "What could still go wrong with this function?"
:::

:::{solution}
The key learning points:
- Starting with structure before asking for implementation
- Iterating to add validation
- Explicitly asking about failure modes

Your final function should handle:
- Missing files
- Invalid JSON syntax
- Missing required fields
- Out-of-range values
- Wrong data types
:::

:::{exercise} Exercise Chat-2: Debug with AI assistance
Take this buggy code and use [duck.ai](https://duck.ai) to help fix it:

```python
def calculate_statistics(data):
    mean = sum(data) / len(data)
    variance = sum((x - mean) ** 2 for x in data) / len(data)
    std = variance ** 0.5
    return {"mean": mean, "std": std, "variance": variance}

# This crashes:
result = calculate_statistics([])
```

Practice:
1. Describe the error to the chatbot
2. Ask it to fix the bug
3. Ask what other edge cases might cause problems
:::

:::{solution}
The function fails on empty lists (division by zero). A proper fix handles:
- Empty lists
- Single-element lists (variance undefined or requires Bessel's correction)
- The difference between population and sample variance

Good prompting would reveal these statistical subtleties.
:::


## Best practices checklist

When using chat-based AI coding:

- [ ] **Never paste sensitive data** - use synthetic examples
- [ ] **Start with architecture** - outline before implementation
- [ ] **Work modularly** - small functions you can test
- [ ] **Verify packages exist** - check PyPI before installing
- [ ] **Test thoroughly** - include edge cases
- [ ] **Understand the code** - if you can't explain it, don't use it
- [ ] **Document your process** - note which parts were AI-generated


## The real benefit: Enabling more ambitious projects

It's tempting to think of AI coding assistants purely in terms of speed.
But the deeper benefit is different.

:::{admonition} Practitioner's perspective: Speed enables ambition
:class: tip

*"This is why I care so much about the productivity boost I get from LLMs:
it's not about getting work done faster, it's about being able to ship
projects that I wouldn't have been able to justify spending time on at all.*

*The fact that LLMs let me execute my ideas faster means I can implement
more of them, which means I can learn even more."*

— Simon Willison
:::

For researchers, this might mean:
- Building a quick visualization tool you wouldn't have time to code manually
- Prototyping analysis approaches before committing to one
- Creating small utilities that improve your workflow
- Exploring "what if" questions through rapid implementation

:::{warning}
**Managing expectations**: LLMs amplify existing expertise. An experienced
developer will get better results than a beginner because they:
- Know what to ask for
- Can evaluate whether the output is correct
- Understand which follow-up questions to ask
- Recognize when the AI is wrong

This doesn't mean beginners can't benefit—they can. But don't expect AI to
instantly give you expert-level results if you're still learning the
fundamentals.
:::


## Summary

The chat-based approach offers maximum control over AI-assisted coding:

- You explicitly choose what information to share
- You review all code before it runs
- You maintain full understanding of your codebase

The trade-off is speed: manually copying code and context takes time. For
many research applications, this trade-off is worthwhile, as the control and
transparency support reproducibility and scientific integrity.


## See also

- [Prompting Guide](https://www.promptingguide.ai/) - General prompting techniques
- [Google: Best Practices for AI Coding Assistants](https://cloud.google.com/blog/topics/developers-practitioners/five-best-practices-for-using-ai-coding-assistants)
- [Simon Willison: How I use LLMs to help me write code](https://simonwillison.net/2025/Mar/11/using-llms-for-code/) - Detailed practical workflow
- [Zero To Mastery: How to Use ChatGPT to 10x Your Coding](https://zerotomastery.io/blog/how-to-use-chatgpt-for-coding/) - Prompt engineering techniques


:::{keypoints}
- Chat-based AI coding gives you maximum control over data and execution
- Use **exploration mode** (ask for options) when researching, **production mode** (precise specs) when implementing
- Context management is the key skill: start fresh when stuck, build iteratively, provide examples
- Work in small, testable modules rather than generating large code blocks
- Iterate freely: the first response is a starting point, not the final answer
- AI is also valuable for understanding existing code, not just generating new code
- The real benefit is enabling projects you wouldn't otherwise have time for
- Never share sensitive research data with AI chatbots
:::

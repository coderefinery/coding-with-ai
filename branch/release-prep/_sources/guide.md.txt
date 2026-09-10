# Instructor's guide

## Why we teach this lesson

AI coding assistants are already part of the daily workflow used by researchers and developers,
but many users lack awareness of the security risks and best practices. This
lesson fills a critical gap by:

- Providing a risk-aware framework rather than just tool tutorials
- Helping researchers make informed decisions about AI tool adoption
- Teaching security practices specific to AI-assisted coding
- Addressing the unique needs of researchers (data sensitivity,
  reproducibility, institutional policies)


## Intended learning outcomes

After this lesson, learners should be able to:

1. **Explain** how LLMs generate code and their fundamental limitations
2. **Compare** the three scenarios (chat, IDE, agentic) in terms of control and risk
3. **Configure** AI coding tools with appropriate privacy and security settings
4. **Verify** AI-suggested packages and review code for common vulnerabilities
5. **Implement** sandboxing strategies for agentic tools
6. **Develop** a personal policy for responsible AI-assisted coding


## Timing

TODO: this needs actual timing

Estimated total time: XXX hours

| Section | Time | Notes |
|---------|------|-------|
| Introduction to LLMs | 30 min | Can be shortened for technical audiences |
| Scenario I: Full control | 30 min | Core concepts, many exercises |
| Scenario II: IDE integration | 30 min | Hands-on setup possible |
| Scenario III: Agentic | 30 min | Discussion-heavy |
| Security | 30 min | Critical section, don't rush |
| Conclusion | 15 min | Reflection and planning |

**For a 2-hour workshop**: Skip some exercises, focus on Scenarios I and II,
and the security section.

**For a half-day workshop**: Include all exercises and add hands-on setup time.


## Preparing exercises

### Before the workshop

1. **Check institutional policies**: Some institutions restrict AI tool use.
   Know the local rules before teaching or make sure learners are aware of their university's rules..

2. **Test tool access**: Verify that learners can access at least one AI
   chatbot. We recommend [Duck.ai](https://duck.ai) as it requires no account
   and is privacy-focused. Alternatives include ChatGPT/Copilot, Claude, or Gemini. 
   Some Universities might provide some local implementation of these tools, and those can also be used.

3. **Prepare fallback examples**: If learners can't install (and don't want to install) IDE extensions or agents,
   have screenshots or recordings ready.

4. ??? DO WE NEED THIS ??? **Create a test repository**: Set up a simple Python project for sandbox
   exercises if using Docker.

### Exercise preparation

- **Chat exercises (Scenario I)**: Work with any AI chatbot, no special setup
- **IDE exercises (Scenario II)**: Requires VS Code and extension installation
  (may take 10-15 minutes for setup + accounts)
- **Security exercises**: Can be done as discussion if time is short
- **Sandboxing exercises**: Require Docker (optional, can demonstrate instead or leave it for self-learning)


## Other practical aspects

### Audience considerations

**For researchers/scientists:**
- Emphasize data sensitivity and reproducibility
- Connect to research integrity discussions
- Discuss publication and funding agency requirements

**For developers/RSEs:**
- Can move faster through basics
- Emphasize security and code review practices
- Discuss team workflow integration

**For mixed audiences:**
- Start with common ground (everyone uses or will use these tools)
- Use research examples but acknowledge varied backgrounds

### Tools needed

- Learners need laptops with:
  - Web browser (for chatbots)
  - VS Code (for IDE integration section)
  - Docker (optional, for sandboxing)

### Handling sensitive topics

- **Privacy concerns**: Acknowledge that data handling varies by tool and tier.
  Don't dismiss concerns.
- **Job security fears**: Frame AI as a tool that augments, not replaces.
  Emphasize the importance of human judgment.
- **Institutional restrictions**: Some learners may not be allowed to use
  certain tools. Provide alternatives (local models, chat-only approaches).
- **Sustainability**: Sustainability it is not only about the environment, but also
  the fact that the most used tools are controlled by few private entities and their goal
  is to maximise their profits (more tokens might mean better performance, but also more revenue). 
  Long-term use of such tool might erode critical thinking capabilities of the user.


## Setting realistic expectations

One of the most important things instructors can do is set realistic
expectations about what learners will experience.

### LLMs amplify existing expertise

Experienced developers get dramatically better results from AI coding
assistants than beginners (TODO: enrico to add the citation). This is because they:

- Know what to ask for (have a mental model of the solution)
- Can evaluate whether output is correct
- Know which follow-up questions to ask
- Recognize when the AI is confidently wrong
- Understand the ecosystem (libraries, patterns, best practices)

**Instructor note**: Be explicit that a learner with basic Python skills
won't get the same results as someone with years of experience. This isn't
a failure of the tools or the learner—it's the nature of amplification.

### Frame the benefit correctly

The real benefit isn't "coding faster"—it's **enabling projects that
wouldn't otherwise exist**. For researchers, this might mean:

- Building a quick visualization they wouldn't have time to code manually
- Prototyping approaches before committing
- Creating small utilities that improve workflow

Use this framing to avoid disappointment when AI doesn't instantly solve
complex problems.

### The testing imperative

One thing that cannot be outsourced: **verifying that code actually works**.

> *"Your responsibility as a software developer is to deliver working
> systems. If you haven't seen it run, it's not a working system. You need
> to invest in strengthening those manual QA habits."*
> — Simon Willison

Emphasize this repeatedly. The most common failure mode is accepting code
that looks right but hasn't been tested.


## Interesting questions you might get

**"Is AI-generated code plagiarism?"**
- As usually, it depends (e.g. on your institution's policies and the context)
- Disclosure is generally recommended (see ALLEA principles for researchers in EU)
- Check your journal's/funder's requirements
- The code itself is typically not copyrighted, but be aware of licensing
  issues with training data

**"What about code completion I've been using for years (IntelliSense, etc.)?"**
- Traditional autocomplete uses static analysis, not LLMs
- It doesn't send your code to external servers
- Modern AI completion is fundamentally different in capability and risk

**"Can I use AI for my thesis/dissertation code?"**
- Check with your supervisor and institution
- Disclose AI use in your methods section
- Be prepared to explain and defend all code
- Document your verification process

**"Which tool is best?"**
- Depends on your use case, risk tolerance, and institutional policies
- There's no single "best" tool AND the landscape changes very fast
- We recommend starting with chat-based approaches, take notes of what 
  worked well for you, improvise and adapt to changes.

**"What about local/offline models?"**
- There are options (Ollama, LM Studio, etc.)
- Quality gap has narrowed significantly (Qwen2.5-Coder rivals GPT-4 on benchmarks **CITATION NEEDED**)
- Good option for sensitive work and offline use
- See {doc}`appendix-local-llms` for setup instructions


## Typical pitfalls

### Teaching pitfalls

1. **Tool-specific focus**: Don't spend too much time on one tool's UI.
   Focus on principles that transfer.

2. **Overselling or underselling**: Neither "AI will replace programmers"
   nor "AI is useless" is accurate. Stay balanced.

3. **Skipping security**: Security section is often rushed. Don't skip it,
   it's the most important practical content.

4. **Assuming access**: Not everyone can sign up for or afford AI tools.
   Have free alternatives ready.

5. **Ethicality of technology**: only a handful of AI models have been trained 
   according to pricinples of responsible AI (e.g. SSAFE-D framework: Sustainability, 
   Safety, Accountability, Fairness, Explainability, and Data-Stewardship). Make
   informed decisions based on your ethical principles. 

### Learner pitfalls

1. **Uncritical acceptance**: Learners may start accepting all suggestions
   without review. Emphasize verification repeatedly.

2. **Over-reliance**: Some learners want AI to do everything. Stress that
   understanding code is still essential.

3. **Privacy complacency**: "I have nothing to hide" attitude toward data
   sharing. Use concrete examples of sensitive data.

4. **Skipping verification**: Learners may skip package verification steps.
   Make this a graded exercise if possible.


## Adapting the material

TODO: this needs rewrite.

### For shorter workshops (1 hour)

Focus on:
- Brief intro (10 min)
- Scenario I with one exercise (20 min)
- Security highlights (20 min)
- Conclusion and resources (10 min)

### For longer courses (half day or more)

Add:
- Hands-on IDE setup with troubleshooting time
- Docker sandbox setup and testing
- Group discussion on institutional policies
- Peer code review of AI-generated code

### For online delivery

- Use breakout rooms for exercises
- Have a shared document for collecting questions
- Consider asynchronous components (pre-reading, post-workshop exercises)
- Test screen sharing of IDE and terminal beforehand


## Resources for instructors

### Training and pedagogy
- [CodeRefinery instructor training](https://coderefinery.github.io/instructor-training/)
- [The Carpentries instructor training](https://carpentries.github.io/instructor-training/)

### Practitioner perspectives (valuable for examples and discussion)
- [Simon Willison: How I use LLMs to help me write code](https://simonwillison.net/2025/Mar/11/using-llms-for-code/) - Detailed practical workflow from an experienced developer
- [Simon Willison's AI-assisted programming series](https://simonwillison.net/series/ai-assisted-programming/) - Ongoing collection of posts
- [Addy Osmani: My LLM coding workflow](https://addyosmani.com/blog/ai-coding-workflow/) - Best practices from a software engineering lead

### Security
- [OpenSSF Security Guide](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions) - Good source for security examples

### Note on using practitioners' quotes

The course materials include quotes from practitioners like Simon Willison.
These serve several purposes:
- **Credibility**: Showing that experienced developers use these tools thoughtfully
- **Nuance**: Practitioners often capture subtleties better than abstract principles
- **Accessibility**: Conversational language can be more memorable than formal statements

When teaching, you can expand on these quotes with your own experience or
invite learners to discuss whether they agree with the perspectives shared.

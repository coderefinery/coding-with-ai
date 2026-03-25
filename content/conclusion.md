# Conclusions and Next Steps

:::{questions}
- How do I decide which AI coding approach is right for my work?
- What should I try first to get started safely?
- Where can I learn more and stay updated?
:::

:::{objectives}
- Provide a decision framework for choosing AI coding approaches
- Suggest concrete next steps for exploration
- Point to resources for continued learning
:::


## What we've learned

This course has explored the spectrum of AI-assisted coding, from full manual
control to autonomous agents.

```
+------------------------------------------------------------------+
|                   Control vs. Automation Trade-off               |
+------------------------------------------------------------------+
|                                                                  |
|  HIGH CONTROL                                      LOW CONTROL   |
|  LOW RISK                                          HIGH RISK     |
|                                                                  |
|  +----------------+  +----------------+  +----------------+      |
|  |  Scenario I    |  |  Scenario II   |  |  Scenario III  |      |
|  |                |  |                |  |                |      |
|  |  Chat + paste  |  |  IDE integrate |  |  Agentic AI    |      |
|  |                |  |                |  |                |      |
|  |  You control:  |  |  You control:  |  |  You control:  |      |
|  |  - All context |  |  - Which files |  |  - Task scope  |      |
|  |  - All code    |  |  - Accept/rej  |  |  - Final review|      |
|  |  - Execution   |  |  - Execution   |  |                |      |
|  +----------------+  +----------------+  +----------------+      |
|                                                                  |
|  SLOW                                                FAST        |
+------------------------------------------------------------------+
```

### Key takeaways

1. **LLMs are pattern matchers, not reasoners**
   - They excel at common patterns but can confidently produce wrong code
   - Always verify, never blindly trust

2. **Control and speed trade off**
   - More automation means faster development but less oversight
   - Choose the level appropriate for your task's risk profile

3. **Security is non-negotiable**
   - Verify packages before installing
   - Review code for vulnerabilities
   - Never share sensitive data with AI services

4. **Transparency matters**
   - Understand what data leaves your machine
   - Know your tool's privacy policies
   - Document AI-assisted portions of your work


## Decision framework

Use this framework to decide which approach fits your situation:

### Step 1: Assess the sensitivity

| Factor | Low sensitivity | High sensitivity |
|--------|-----------------|------------------|
| Data | Public/synthetic | Private/confidential |
| Code | Open source style | Proprietary/patented |
| System | Isolated dev machine | Production/research infrastructure |

**High sensitivity → Use Scenario I (chat) or local models (see {doc}`appendix-local-llms`)**

### Step 2: Assess the risk tolerance

| Factor | Low stakes | High stakes |
|--------|------------|-------------|
| Reversibility | Easy to undo | Hard to fix |
| Impact | Personal project | Shared/published work |
| Verification | Easy to test | Complex to validate |

**High stakes → More control, more review**

### Step 3: Match approach to task

| Task type | Recommended approach |
|-----------|---------------------|
| Learning a new concept | Scenario I (chat) |
| Designing architecture | Scenario I (chat) |
| Writing routine code | Scenario II (IDE) |
| Refactoring | Scenario II or III |
| Boilerplate generation | Scenario III (agentic) |
| Security-critical code | Scenario I with extra review |
| Production deployment | Manual, with AI consultation only |

:::{warning}
**Expertise amplification**: Remember that AI tools amplify existing
expertise. An experienced developer with domain knowledge will get
dramatically better results than a beginner. This is because they:

- Know what to ask for (have a mental model of the solution)
- Can evaluate whether output is correct
- Know which follow-up questions to ask
- Recognize when the AI is confidently wrong

Don't expect AI to compensate for fundamentals you haven't learned.
It's a force multiplier, not a replacement for understanding.
:::


## Recommended first steps

If you're new to AI-assisted coding, start conservatively:

### Week 1: Chat-based exploration

1. **Try a chatbot for a real task**
   - Pick something non-critical from your current work
   - Ask it to help design a function or explain existing code
   - Practice the modular prompting approach

2. **Verify everything**
   - Check any suggested packages exist
   - Test generated code thoroughly
   - Ask the AI about edge cases

### Week 2: IDE integration (carefully)

1. **Set up with restrictions**
   - Install GitHub Copilot or Codeium
   - Configure `.copilotignore` for sensitive files
   - Disable for markdown and data files

2. **Practice critical review**
   - Use the 3-second rule before accepting
   - Watch for wrong variable names and assumptions
   - Compare suggestions to your own solutions

### Week 3: Evaluate advanced tools

1. **Research before installing**
   - Read privacy policies
   - Understand permission models
   - Check institutional policies

2. **Try in a sandbox**
   - Use Docker or a test environment
   - Start with read-only modes
   - Never give access to real research data


## Tools to try

:::{admonition} Beyond these recommendations
:class: note

The tools listed below are starting points aligned with our three scenarios.
The real landscape is much broader—see {doc}`appendix-spectrum` for a
comprehensive taxonomy of AI coding tools, including local/self-hosted options,
PR-native agents, and specialized review tools.
:::

### Chatbots (Scenario I)

| Tool | Access | Notes |
|------|--------|-------|
| [Duck.ai](https://duck.ai) | Free, no account needed | Privacy-focused, anonymous |
| [ChatGPT](https://chat.openai.com) | Free tier available | Most widely used |
| [Claude](https://claude.ai) | Free tier available | Large context window |
| [Gemini](https://gemini.google.com) | Free | Google integration |

### IDE extensions (Scenario II)

| Tool | Access | Notes |
|------|--------|-------|
| [GitHub Copilot](https://github.com/features/copilot) | Free for students | Most mature |
| [Windsurf](https://windsurf.com) | Free core | Good free option |


### Agentic tools (Scenario III)

| Tool | Access | Notes |
|------|--------|-------|
| [Claude Code](https://github.com/anthropics/claude-code) | Requires Claude sub | Terminal-based |
| [OpenAI Codex](https://openai.com/codex/) | Requires OpenAI subscription | Terminal based |
| [Aider](https://aider.chat) | Open source | Multiple models |


## Staying informed

The AI coding landscape changes rapidly. Stay updated:

### News and research

- [Hacker News](https://news.ycombinator.com) - Tech community discussions
- [arXiv cs.SE](https://arxiv.org/list/cs.SE/recent) - Software engineering research
- [The Pragmatic Engineer](https://newsletter.pragmaticengineer.com) - Industry perspective

### Practitioner blogs

- [Simon Willison's Weblog](https://simonwillison.net/) - Practical AI-assisted coding insights
- [Addy Osmani's Blog](https://addyosmani.com/blog/) - Software engineering and AI workflows

### Security updates

- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OpenSSF](https://openssf.org) - Open Source Security Foundation

### Community discussions

- Your local research computing community
- CodeRefinery workshops and Zulip chat
- The Carpentries community


## Institutional considerations

Before adopting AI tools for research, check:

### Policy compliance

- Does your institution have AI usage policies?
- Are there data handling requirements for your field?
- What about publication requirements (disclosing AI use)?

### Research integrity

- How will you document AI-assisted portions?
- What verification process will you use?
- How will you ensure reproducibility?

### Collaboration

- Are your collaborators comfortable with AI tools?
- How will you handle shared codebases?
- What about code review processes?

:::{callout} Emerging standards
Many journals and funding bodies are developing policies on AI use in research.
Stay informed about requirements in your field. When in doubt, disclose AI
assistance and document your verification process.
:::


## Final exercise

:::{exercise} Exercise Final: Create your personal AI coding policy
Create a brief document (1 page) that outlines your personal policy for
using AI coding assistants. Include:

1. **Which tools you'll use** and for what purposes
2. **Security measures** you'll implement
3. **Verification steps** you'll always perform
4. **What you won't do** (boundaries)
5. **How you'll document** AI assistance in your work

Share this with your research group or collaborators for discussion.
:::

:::{solution}
Example policy outline:

**My AI Coding Policy**

**Tools I'll use:**
- ChatGPT/Claude for design discussions and learning
- GitHub Copilot for routine coding (with restrictions)
- No agentic tools on production systems

**Security measures:**
- All secrets in environment variables, never in code
- .copilotignore for data and credential files
- Verify all suggested packages on PyPI
- Run bandit on AI-generated code

**Verification steps:**
- Test all AI code with edge cases
- Review for OWASP Top 10 vulnerabilities
- Understand all code before committing

**Boundaries:**
- No real participant data shared with AI
- No AI for security-critical authentication code
- No AI commits without human review

**Documentation:**
- Note AI-assisted sections in code comments
- Include "AI-assisted" in commit messages where applicable
- Disclose in publications per journal requirements
:::


## Summary

AI coding assistants are powerful tools that require thoughtful adoption:

- Start with high-control approaches and gradually explore automation
- Security is not optional, build verification into your workflow
- Match tool autonomy to task risk
- Stay informed as the landscape evolves
- Document and be transparent about AI assistance

The goal is not to avoid AI tools, but to use them responsibly in a way that
enhances your productivity while maintaining the integrity and security of
your research.



:::{keypoints}
- Match AI tool autonomy to task risk and sensitivity
- Start with chat-based approaches before exploring more automation
- Security measures and verification steps are non-negotiable
- Document AI assistance for transparency and reproducibility
- Stay informed as the landscape evolves rapidly
:::

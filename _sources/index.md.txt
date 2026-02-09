# Responsible Use of Generative AI in Assisted Coding

Generative AI tools are transforming how researchers write code. From simple
chatbot interactions to fully autonomous coding agents, these tools offer
powerful capabilities ... and with great power comes great risks and responsibilities.

This lesson provides a practical framework for researchers who want to use
AI coding assistants in their day-to-day software work with research.  The framwork stresses on the importance of maintaining control, security, and research integrity. We will explore three  scenarios of increasing automation and decreasing
user control, helping you make informed decisions about which approach fits your needs.


```
+------------------+     +------------------+     +------------------+
|  Scenario I      |     |  Scenario II     |     |  Scenario III    |
|  Full Control    |     |  IDE Integration |     |  Agentic AI      |
|                  |     |                  |     |                  |
|  Chat + manual   |     |  Tab completion  |     |  Autonomous      |
|  copy/paste      |     |  inline suggest  |     |  code agents     |
|                  |     |                  |     |                  |
|  LOW RISK        |     |  MEDIUM RISK     |     |  HIGH RISK       |
|  HIGH CONTROL    |     |  MEDIUM CONTROL  |     |  LOW CONTROL     |
+------------------+     +------------------+     +------------------+
```
*ASCII created with Claude. It should be replaced with an image*


:::{prereq}
- Basic familiarity with programming (Python examples used, replace with your favourite language)
- Access to a code editor (e.g. VS Code) or Jupyter environment
- Optional: Accounts for AI tools you want to try (ChatGPT, Claude, GitHub Copilot, etc. Please note that some tools are not free and require a credit card).
:::

:::{warning}
This is work in progress. Known limitations
1. There might be too much content than what can be covered during the CodeRefinery workshop.
2. Some tools change so fast, this is likely already obsolete in two months from now.
3. Many bits here and there need testing and improvement.
:::


```{csv-table}
:delim: ;
:widths: auto

xx min ; {doc}`introduction`
xx min ; {doc}`scenario-full-control`
xx min ; {doc}`scenario-ide-integration`
xx min ; {doc}`scenario-agentic`
xx min ; {doc}`security`
xx min ; {doc}`conclusion`
```

```{toctree}
:caption: The lesson
:maxdepth: 1

introduction
scenario-full-control
scenario-ide-integration
scenario-agentic
security
conclusion
```

```{toctree}
:caption: Reference
:maxdepth: 1

quick-reference
guide
```

```{toctree}
:caption: Appendices
:maxdepth: 1

appendix-spectrum
appendix-local-llms
```

(learner-personas)=

## Who is the course for?

This course is designed for:

- **Researchers at all levels** who write code for data analysis, simulations, or scientific computing and wish to accelerate their coding workflows
- **Research software engineers** who support scientific projects
- **Anyone** curious about using AI assistants responsibly in a research context

No prior experience with AI coding tools is required. 


## About the course

This course takes a research-integrity-focused approach on AI-assisted coding. **There is no point in trying to optimise deep knowledge for any of these tools** since what works well today, will most likely be replaced by something else in a few months. Rather than
simply teaching you how to use these tools, we help you understand:

- **What happens under the hood**: How do these tools work? What data were they trained on?
- **What leaves your machine**: When you use these tools, what information is sent to remote servers?
- **What risks exist**: From hallucinated packages to prompt injection attacks
- **How to mitigate risks**: Sandboxing, verification strategies, and best practices




## See also

- [OpenSSF Security Guide for AI Code Assistants](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions)
- TODO Add more references here


## Credits

This lesson was developed as part of [CodeRefinery](https://coderefinery.org/) training activities. You can check the list of contributors, submit an issue, or even suggest an edit via pull request, by visiting ADD GITHUB URL HERE.

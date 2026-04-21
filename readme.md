# Responsible Use of Generative AI in Assisted Coding

Repository for the [CodeRefinery](https://coderefinery.org/) lesson **"Responsible Use of Generative AI in Assisted Coding"**.

Rendered lesson: https://coderefinery.github.io/coding-with-ai/

---

## About the lesson

Generative AI tools are transforming how researchers write code. From simple chatbot interactions to fully autonomous coding agents, these tools offer powerful capabilities — and with great power comes great responsibility.

This lesson provides a practical framework for researchers who want to use AI coding assistants in their day-to-day work. Rather than simply teaching how to use specific tools (which change rapidly), it helps learners understand what happens under the hood, what data leaves their machine, what risks exist, and how to mitigate them.

### Lesson structure

The lesson is organised around three scenarios of increasing automation and decreasing user control:

| Section | Description | Duration |
|---|---|---|
| **Introduction** | What LLMs are, how coding models are trained, landscape of tools | 25 min |
| **Scenario I — Full control** | Chat-based coding: the user copies code from a chatbot into their IDE or notebook manually. Lowest risk; user sees everything. | 20 min |
| **Scenario II — IDE integration** | AI-assisted tab completion and inline suggestions (e.g. GitHub Copilot). Less transparency; some tools can edit files automatically. | 15 min |
| **Scenario III — Agentic** | Fully autonomous coding agents (e.g. Claude Code, OpenAI Codex). Highest risk; agent can install packages, edit files, run commands. | 20 min |
| **Security considerations** | Hallucinated packages, prompt injection, data leakage, sandboxing and mitigation strategies | 10 min |
| **Conclusion** | Summary and decision framework | 5 min |

### Reference and appendices

- **Quick reference** — condensed checklists and comparison tables
- **Instructor guide** — learning outcomes, setup instructions, timing notes
- **Appendix: Spectrum of tools** — full taxonomy of AI coding assistants
- **Appendix: Local LLMs** — how to run models entirely on your own machine

---

## Workshop: Data Exploration and Visualization with Python and Generative AI

In addition to the main lesson, this repository contains a standalone **hands-on workshop** designed for researchers with little or no prior coding experience. The workshop teaches data exploration and visualisation in Python using a GenAI assistant as a coding companion.

### Workshop structure

- The geography of computing — where should your code run, and why does it matter for data privacy?
- Setting up JupyterLab (local, institutional cloud, or Google Colab)
- Two approaches to writing code: brain+search vs. GenAI assistant
- Choosing a GenAI assistant: local models, institutional services, external cloud
- Hands-on exercises using classic open datasets (Titanic, Iris, Palmer Penguins, Gapminder, and more)
- Exploring the Altair and Seaborn visualisation galleries

### Workshop pages

- [Workshop overview](https://coderefinery.github.io/coding-with-ai/workshop-data-viz.html)
- [JupyterLab quick introduction](https://coderefinery.github.io/coding-with-ai/workshop-jupyter-basics.html)
- [Reference datasets](https://coderefinery.github.io/coding-with-ai/workshop-datasets.html)

---

## Contributing

Contributions are welcome. Please open an issue or pull request on GitHub.

Note: parts of the initial draft of the lesson content were created with the assistance of generative AI tools (ChatGPT and Claude). All content has been reviewed and edited by the human authors listed below.

## License

The lesson material is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/).

## Authors and citation

See [CITATION.cff](CITATION.cff) for full citation metadata.

The lesson was initially developed by Enrico Glerean as part of CodeRefinery training activities, building on the workshop "AI and Research Work" ([Glerean & Silva, 2024](https://zenodo.org/records/14032261)). It was further expanded and reviewed by Bjørn Lindi, Ina Pöhner, Jarno Rantaharju, Simon Christ, Ashwin V. Mohanan, Michele Mesiti, Frankie Robertson, and Thomas Pfau.

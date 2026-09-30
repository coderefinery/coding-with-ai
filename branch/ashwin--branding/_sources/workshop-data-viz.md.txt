# Data Exploration and Visualization with Python and Generative AI

This workshop is designed for beginners who have little or no prior coding experience. Over the course of the session you will learn how to explore and visualise a dataset using Python, with generative AI as your coding companion. By the end you will have produced at least one chart from real data — and you will understand a bit better what is actually happening when you let an AI write code for you.

The workshop was developed at Aalto University as part of the [Aalto Scientific Computing](https://scicomp.aalto.fi/) training activities.

:::{seealso}
**Recommended reading:** [Fundamentals of Data Visualization](https://clauswilke.com/dataviz/) by Claus O. Wilke (O'Reilly, freely available online). A practical, code-free guide to what makes a chart work — covers choosing the right chart type, handling colour, dealing with overplotting, and communicating uncertainty. Worth bookmarking even if you only read one chapter at a time.
:::

:::{prereq}
- No prior coding experience required
- Access to a computer with an internet connection
- A computing environment set up (see {ref}`workshop-setup` below)
:::


## How data visualisation happens

Data visualisation is not a single action. It is a process with four steps that you will repeat many times, going back and forth between them:

1. **Find and load data** — identify a dataset relevant to your question and get it into your coding environment as a table or array.
2. **Explore the data structure** — look at what columns exist, what data types they are, how many rows, whether there are missing values.
3. **Process the data** — clean it up: handle missing values, filter rows you don't need, rename columns, create derived variables.
4. **Visualise** — make a chart that communicates something meaningful about the data.

:::{callout} These steps are not linear
You will almost always go back to step 2 or 3 after you make your first chart and realise something looks wrong or unexpected. That is normal and expected — it is how data exploration works.
:::


:::{exercise} Exercise 0: Share a data visualisation you like
Before we dive into code, let's take a moment to think about what good data visualisation looks like.

1. Think of a data visualisation you have seen and liked — it could be from a newspaper, a scientific paper, a website, a social media post, or anywhere else.
2. Find the image or a link to it.
3. Add it to the shared notes document with a one-sentence explanation of why you liked it.
4. Have a look at what others have shared. Is there a common theme in what people find appealing?
:::


## Ready-made tools vs writing code

There are two broad approaches to data visualisation:

| Approach | Examples | Strengths | Limitations |
|---|---|---|---|
| Ready-made GUI tools | Tableau, Datawrapper, Google Sheets, MS Excel | No coding needed; fast to produce standard charts | Limited customisation; hard to fully automate or reproduce; tied to one vendor |
| Custom code | Python, R, Julia | Full control over every visual detail; reproducible; automatable | Requires learning a programming language and its libraries |

:::{callout} Why we use Python in this workshop
Python has a large ecosystem of free, open-source data and visualisation libraries (pandas, matplotlib, seaborn, altair, plotly, …). The skill transfers directly to research computing, data analysis pipelines, and publication-quality figures. And with a GenAI assistant, the initial learning curve is much lower than it used to be.
:::

:::{warning} A fourth option: GenAI-generated visualisations
A newer category of tool lets you skip code entirely and ask an AI to produce a chart directly from your data — for example [paperbanana](https://paperbanana.run/) and similar services. You upload a file or paste some numbers and receive an image of a chart.

This is genuinely promising for quick exploratory views, but it comes with serious limitations that matter in a research context:

- **Hallucination** — the AI may invent values, mislabel axes, or silently exclude data points without warning you.
- **Text distortions** — axis labels, legends, and annotations in AI-generated images are often garbled, misspelled, or nonsensical, because image-generation models do not "understand" text.
- **No reproducibility** — you cannot share, version, or re-run the code that produced the chart, because there is no code.
- **Difficult to iterate** — asking for a small change (different colour, different axis range, add a trend line) often means regenerating from scratch with no guarantee of consistency.
- **Not grounded in the data** — unlike a Python script where you can inspect every transformation step, you have no way to verify that the chart accurately reflects the underlying data.

For research work, any visualisation that appears in a paper, report, or presentation must be reproducible and verifiable. Code-based visualisation satisfies both requirements; AI-generated image visualisation currently does not.
:::


## Checklist before you start

Before you write a single line of code, make three explicit decisions:

- **Which programming language?** — Python (this workshop). The most popular language for data science; enormous library ecosystem; runs everywhere.
- **Where will the computation run?** — Your laptop, your organisation's cloud, or an external cloud service. See the next section.
- **Where does your data live?** — Local disk, institutional storage, or a public URL. This matters especially if the data is sensitive or personal.

The "where" question is more important than it first appears. It affects data confidentiality, ease of setup, and whether your workflow will still work in six months when a cloud service changes its terms.


(workshop-setup)=
## The geography of computing

Every time you run code, the computation happens *somewhere*. There are three broad categories:

```
  ◄──────── more private, more setup ─────────────── less private, less setup ────────►

  ┌────────────────────────┐    ┌────────────────────────┐    ┌────────────────────────┐
  │                        │    │                        │    │                        │
  │      Your laptop       │    │    Your org cloud      │    │    External cloud      │
  │        (local)         │    │   Aalto, CSC, etc.     │    │   Colab, Kaggle, etc.  │
  │                        │    │                        │    │                        │
  │  data stays with you   │    │  data stays in-house   │    │  data leaves your org  │
  │  you manage installs   │    │  software pre-installed│    │  zero setup needed     │
  │                        │    │                        │    │                        │
  └────────────────────────┘    └────────────────────────┘    └────────────────────────┘
```

| Environment | Examples | Advantages | Considerations |
|---|---|---|---|
| **Local laptop** | Your own machine, Python installed | Data never leaves your hands; works offline; full control | Risk if device is lost/stolen; requires manual installation; limited RAM/CPU |
| **Organisation cloud** | Aalto JupyterHub (`jupyter.cs.aalto.fi`), CSC Noppe (`noppe.csc.fi`) | Institutional data protection; software pre-installed; no personal hardware risk | Depends on institution's uptime and quota; requires account |
| **External cloud** | Google Colab, Kaggle Notebooks | Zero installation; free tier available; easy to share | Unclear or unfavourable terms of service; data uploaded to third party; may not be suitable for sensitive data |

:::{warning}
If you are working with real research data — especially personal data, clinical data, or anything covered by your institution's data management plan — check with your data protection officer or IT security team before choosing an environment. External cloud services (including Google Colab and GenAI tools) are generally **not** suitable for sensitive or personal data.
:::

### Setting up your environment

**On your own laptop:**

```bash
pip install jupyterlab pandas matplotlib seaborn altair vega_datasets
jupyter lab
```

**Aalto University or CSC (Finland):**

- Aalto JupyterHub: [https://jupyter.cs.aalto.fi](https://jupyter.cs.aalto.fi) (requires Aalto account)
- CSC Noppe: [https://noppe.csc.fi](https://noppe.csc.fi) (open to Finnish researchers and students)

**External cloud (easiest to start, least private):**

- Google Colab: [https://colab.research.google.com](https://colab.research.google.com) (Google account required)

To test that your environment is working, run this in a notebook cell:

```python
import pandas as pd
pd.__version__
```

If it prints a version number without an error, you are ready to go.


## The JupyterLab interface

JupyterLab is the interactive coding environment we use in this workshop. It runs in your browser and lets you write code, run it, and see the output — all in the same document called a *notebook*.

If you have never used JupyterLab before, read the short introduction at {doc}`workshop-jupyter-basics` before continuing.


## Two approaches to writing code

Once your environment is set up, you have two broad strategies for actually writing Python:

```
  ┌──────────────────────────────────┐              ┌──────────────────────────────────┐
  │                                  │              │                                  │
  │      Your brain + search         │              │        GenAI assistant           │
  │                                  │    v s .     │                                  │
  │  - You write every line          │              │  - You describe the goal         │
  │  - You look up the docs          │              │  - AI writes the code            │
  │  - You understand it all         │              │  - You read and verify           │
  │  - Nothing leaves your machine   │              │  - Code may contain errors       │
  │                                  │              │                                  │
  └──────────────────────────────────┘              └──────────────────────────────────┘
        slow to start, resilient                          fast, but verify always!
```

| Approach | Description | Strengths | Risks |
|---|---|---|---|
| **Brain + search** | Write code yourself; look up what you don't know in documentation or via Google | Builds real understanding; no data shared externally; works offline; you learn | Slower, especially at the beginning |
| **GenAI assistant** | Describe what you want in plain language; receive code from the AI | Fast; low barrier for beginners; good for boilerplate | Loss of understanding and autonomy; hallucination risk; cybersecurity concerns; ethical/legal open questions about training data |

:::{callout} Using GenAI with eyes open
This workshop uses a GenAI assistant as a tool — not as a shortcut to avoid understanding. The goal is always to **read the code you receive**, understand what it does (or ask the AI to explain it), and only run code you are reasonably confident about.

A GenAI assistant can:
- Save time on boilerplate (loading data, formatting a chart)
- Help you discover libraries and functions you didn't know existed
- Explain error messages in plain language

A GenAI assistant will sometimes:
- Invent function names or library versions that do not exist ([hallucination](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence)))
- Produce code that runs but does the wrong thing
- Use a library you don't have installed
- Give subtly different results each time you ask the same question

Always verify: does the code run? Does the output look correct? Do you understand what it is doing?
:::


## Choosing a GenAI assistant

There are three categories of GenAI coding assistant, differing mainly in where the computation runs:

```
  ◄──────── more private, more setup ─────────────── less private, less setup ────────►

  ┌────────────────────────┐    ┌────────────────────────┐    ┌────────────────────────┐
  │                        │    │                        │    │                        │
  │      Your laptop       │    │   Your org's GenAI     │    │    External cloud      │
  │      llama.cpp         │    │    ai.aalto.fi, ...    │    │  ChatGPT, Gemini,      │
  │      Ollama, ...       │    │                        │    │  Claude, ...           │
  │                        │    │  follows institutional │    │                        │
  │  fully private,        │    │  data policies         │    │  easy, but data goes   │
  │  runs on your CPU/GPU  │    │                        │    │  to third party        │
  │                        │    │                        │    │                        │
  └────────────────────────┘    └────────────────────────┘    └────────────────────────┘
```

| Option | Example | Notes |
|---|---|---|
| **Local (on your machine)** | [llama.cpp](https://github.com/ggerganov/llama.cpp) | Fully private; no data leaves your computer; requires a capable GPU or CPU; setup is non-trivial. See {doc}`appendix-local-llms`. |
| **Organisation's GenAI** | [ai.aalto.fi](https://ai.aalto.fi) (Aalto only) | Complies with institutional data policy; recommended for sensitive work |
| **External cloud** | ChatGPT, Gemini, Claude | Easy to access; generally free tier available; review the provider's privacy terms before use |

:::{warning}
Do **not** paste real personal data, clinical data, or any data covered by a confidentiality agreement into an external GenAI tool. The text you submit is typically used to improve the model or stored on the provider's servers.
:::

For a local laptop option without any cloud dependency, you can run a small coding model with:

```bash
llama-server --hf-repo ggml-org/Qwen2.5-Coder-1.5B-Q8_0-GGUF \
             --hf-file qwen2.5-coder-1.5b-q8_0.gguf -c 2048
```

This downloads and runs a compact coding model entirely on your machine. The quality is lower than large cloud models but it is completely private.


## Your first GenAI coding prompt

Let's start with a concrete example. Copy the following prompt and paste it into your GenAI assistant:

```
I need a Python script that loads the Titanic dataset and shows the first
rows of the data. Can you load it from an internet URL?
```

:::{callout} What to look for in the response
- Does it use the `pandas` library? (`import pandas as pd`)
- Does it provide a URL to download the CSV from?
- Is the URL real? (You can paste it in your browser to check.)
- Does it use `pd.read_csv()`?
- Does it call `.head()` or `.head(10)` to show the first rows?

A working response should look roughly like this:

```python
import pandas as pd
url = "https://raw.githubusercontent.com/pandas-dev/pandas/master/doc/data/titanic.csv"
titanic = pd.read_csv(url)
titanic.head()
```

If the AI gives you a URL that returns a 404 error, ask it to provide an alternative source or give it the correct URL yourself.
:::


## Hands-on exercises

Work through these exercises at your own pace. For each one, use your GenAI assistant to generate the code, then copy it into a notebook cell and run it. Read the code before running it.

:::{exercise} Exercise 1: Load the Titanic dataset
1. Use the prompt from the previous section with your chosen GenAI assistant.
2. Copy the generated code into a new Jupyter notebook cell.
3. Run the cell (Shift+Enter). Do you see a table with the first rows of the data?
4. If it fails, read the error message and ask the GenAI to fix it — paste the error back into the chat.
5. Ask the GenAI: "What do the column names in this dataset mean?"
:::

:::{exercise} Exercise 2: Make your first chart
1. In the same chat session, ask:
   ```
   Can you add a bar chart showing how many passengers survived versus
   did not survive? Use the 'Survived' column.
   ```
2. Copy the code and run it. Is the chart readable?
3. Ask the GenAI to improve it: add a title, change the x-axis labels from 0/1 to "Did not survive"/"Survived", and add the count as a number above each bar.
4. How many prompts did it take to get a chart you are happy with?
:::

:::{exercise} Exercise 3: A different dataset
1. Go to {doc}`workshop-datasets` and choose any dataset that interests you (the Iris, Palmer Penguins, or Gapminder datasets are good starting points).
2. Load the dataset using the code provided on that page.
3. Ask the GenAI to make one visualisation of your choice. Let it suggest something if you're not sure.
4. Iterate: ask for at least two improvements (different colours, better axis labels, adding a trend line, etc.).
5. Can you explain to a neighbour what the chart shows?
:::

:::{exercise} Exercise 4: Independent exploration
1. Pick a question you find genuinely interesting in one of the datasets.
2. Design your own analysis — you decide what to look at and how to show it.
3. Use GenAI to help you code each step.
4. Be ready to share your chart with the group and explain: what question were you asking, and what does the chart tell you?

Some starter questions if you need inspiration:
- Titanic: Is the survival rate different for men vs women in the same passenger class?
- Penguins: Which island has the heaviest penguins on average?
- Gapminder: Which ten countries had the biggest increase in life expectancy between 1952 and 2007?
:::

:::{exercise} Exercise 5: Explore the Altair and Seaborn galleries
The best way to discover what a visualisation library can do is to browse its example gallery, find a chart you like, and run it yourself.

1. Open the [Altair example gallery](https://altair-viz.github.io/gallery/index.html) in your browser. Browse through the categories (bar charts, scatter plots, maps, interactive charts, …). Pick one example that looks interesting or useful to you.
2. Click on the example to see its full code. Copy the code into a new notebook cell and run it. Does it work as shown?
3. Now do the same with the [Seaborn example gallery](https://seaborn.pydata.org/examples/index.html). Pick one plot you like, copy its code into a new cell, and run it.
4. For either chart, ask your GenAI assistant to adapt it to one of the datasets from {doc}`workshop-datasets`. For example: *"Can you adapt this Altair heatmap example to work with the Titanic dataset?"*
5. What did you notice about the difference in style and syntax between Altair and Seaborn?
:::


:::{keypoints}
- Data visualisation follows four iterative steps: find data, explore structure, process, visualise.
- Before you start, decide explicitly: which language, where will computation run, where does your data live.
- The computing environment you choose affects data privacy — external cloud services are not appropriate for sensitive data.
- GenAI is a fast coding companion but is not a reliable expert: always read the code it gives you before running it.
- Iteration is the method: the first chart is rarely the final chart. Use the GenAI conversation to refine step by step.
- For real research data, prefer local or institutional environments over external cloud GenAI tools.
:::

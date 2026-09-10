# Appendix II: Running Local LLMs for Coding

:::{warning}
To-Do: This section needs reviewing and testing on multiple systems. Help us via GitHub issues and pull requests.
:::

:::{admonition} Why run models locally?
:class: note

Running AI models on your own machine offers significant advantages for
research computing:

- **Privacy**: Your code never leaves your machine
- **Offline capability**: Works without internet connection
- **No API costs**: After initial setup, inference is free
- **Full control**: Choose models, customize behavior, no vendor lock-in
- **Compliance**: May help meet institutional data handling requirements

The trade-off is reduced capability compared to cloud models and hardware
requirements. This appendix helps you set up a practical local coding
assistant.
:::


## Overview: The local AI coding stack

A local AI coding setup has three components:

```
+------------------+     +------------------+     +------------------+
|   Your Editor    |     |   Model Runner   |     |   Local Model    |
|   (VS Code)      | --> |   (Ollama)       | --> |   (Qwen2.5-Coder)|
|                  |     |                  |     |                  |
|   + Extension    |     |   Serves model   |     |   Runs on your   |
|   (Continue)     |     |   via HTTP API   |     |   CPU/GPU        |
+------------------+     +------------------+     +------------------+
```

**Recommended stack for most users:**
- **Editor**: VS Code
- **Extension**: Continue (open source, feature-rich)
- **Model runner**: Ollama (easy installation, good performance)
- **Model**: Qwen2.5-Coder (best benchmark performance for size)


## Hardware requirements

Local LLMs require significant resources. Here's what to expect:

### Minimum requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| RAM | 8 GB | 16+ GB |
| Storage | 10 GB free | 50+ GB free |
| CPU | Modern multi-core | Apple Silicon / Recent Intel/AMD |

### GPU acceleration (optional but recommended)

| GPU Type | VRAM | What you can run |
|----------|------|------------------|
| None (CPU only) | - | 1-3B models, slow |
| 6-8 GB | NVIDIA/AMD | 7B models comfortably |
| 12-16 GB | NVIDIA/AMD | 14B models, some 32B quantized |
| 24+ GB | NVIDIA/AMD | 32B+ models |
| Apple Silicon | Unified | 7-14B models well, larger with patience |

:::{admonition} Apple Silicon note
:class: tip

Apple Silicon Macs (M1/M2/M3/M4) work well for local LLMs because they use
unified memory—the same RAM serves both CPU and GPU. A Mac with 16GB unified
memory can comfortably run 7B models and handle 14B models reasonably well.
:::


## Step 1: Install Ollama

[Ollama](https://ollama.ai) is the easiest way to run local models. It handles
downloading, quantization, and serving models via a simple API.

### macOS

```bash
# Option 1: Download from website
# Visit https://ollama.ai and download the app

# Option 2: Homebrew
brew install ollama
```

### Linux

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### Windows

Download the installer from [ollama.ai](https://ollama.ai).

### Verify installation

```bash
# Start Ollama (may start automatically)
ollama serve

# In another terminal, test it
ollama --version
```


## Step 2: Download a coding model

### Recommended models for coding

Based on current benchmarks (2025), here are the best local coding models:

| Model | Size | Best for | Command |
|-------|------|----------|---------|
| **qwen2.5-coder:7b** | ~4.5 GB | Best balance of quality/speed | `ollama pull qwen2.5-coder:7b` |
| **qwen2.5-coder:1.5b** | ~1 GB | Fast autocomplete | `ollama pull qwen2.5-coder:1.5b` |
| **qwen2.5-coder:14b** | ~9 GB | Higher quality, slower | `ollama pull qwen2.5-coder:14b` |
| **deepseek-coder:6.7b** | ~4 GB | Good alternative | `ollama pull deepseek-coder:6.7b` |
| **codellama:7b** | ~4 GB | Meta's coding model | `ollama pull codellama:7b` |
| **starcoder2:3b** | ~2 GB | Lightweight, good for autocomplete | `ollama pull starcoder2:3b` |

:::{admonition} Model recommendation: Start with Qwen2.5-Coder
:class: tip

[Qwen2.5-Coder](https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct) currently
leads open-source coding benchmarks, scoring 88.4% on HumanEval with the 7B
model—higher than GPT-4's 87.1%. It significantly outperforms alternatives
like StarCoder2 and DeepSeek-Coder at similar sizes.

For most users, start with `qwen2.5-coder:7b` for chat/assistance and
`qwen2.5-coder:1.5b` for fast autocomplete.
:::

### Download your chosen model

```bash
# Download the recommended model (takes a few minutes)
ollama pull qwen2.5-coder:7b

# Optionally, also get a small model for autocomplete
ollama pull qwen2.5-coder:1.5b
```

### Test the model

```bash
# Interactive test
ollama run qwen2.5-coder:7b "Write a Python function to calculate fibonacci numbers"
```

If this works, your model is ready.


## Step 3: Install Continue extension in VS Code

[Continue](https://continue.dev) is an open-source AI coding assistant that
integrates with VS Code and JetBrains IDEs. It supports local models via Ollama.

### Install the extension

1. Open VS Code
2. Go to Extensions (Ctrl/Cmd + Shift + X)
3. Search for "Continue"
4. Click Install on "Continue - Codestral, Claude, and more"

Alternatively, from the command line:

```bash
code --install-extension Continue.continue
```

### Initial setup

After installation:

1. Continue will appear in the sidebar (look for the Continue icon)
2. Click on it to open the Continue panel
3. It will prompt you to configure a model


## Step 4: Configure Continue for Ollama

Continue uses a `config.json` file for configuration. Open it via:

1. Open Command Palette (Ctrl/Cmd + Shift + P)
2. Type "Continue: Open Config"
3. Select it to open the configuration file

### Basic configuration

Replace the contents with this configuration:

```json
{
  "models": [
    {
      "title": "Qwen2.5 Coder 7B (Local)",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen2.5 Coder 1.5B (Autocomplete)",
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b"
  },
  "tabAutocompleteOptions": {
    "multilineCompletions": "auto"
  }
}
```

### Configuration explained

| Setting | Purpose |
|---------|---------|
| `models` | Models available for chat (Ctrl/Cmd + L) |
| `tabAutocompleteModel` | Model used for inline code completion |
| `tabAutocompleteOptions` | Settings for autocomplete behavior |

### Multiple models configuration

You can configure multiple models and switch between them:

```json
{
  "models": [
    {
      "title": "Qwen2.5 Coder 7B",
      "provider": "ollama",
      "model": "qwen2.5-coder:7b"
    },
    {
      "title": "Qwen2.5 Coder 14B (Slower)",
      "provider": "ollama",
      "model": "qwen2.5-coder:14b"
    },
    {
      "title": "DeepSeek Coder",
      "provider": "ollama",
      "model": "deepseek-coder:6.7b"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Fast Autocomplete",
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b"
  }
}
```


## Step 5: Using Continue

### Chat with your code (Ctrl/Cmd + L)

1. Select some code in your editor
2. Press Ctrl/Cmd + L
3. Type your question: "Explain this code" or "Add error handling"
4. The response appears in the Continue panel

### Inline editing (Ctrl/Cmd + I)

1. Select code you want to modify
2. Press Ctrl/Cmd + I
3. Describe the change: "Add type hints" or "Convert to async"
4. Review and accept/reject the changes

### Tab autocomplete

Once configured, you'll see ghost text suggestions as you type. Press Tab
to accept, or keep typing to ignore.

:::{admonition} First-run slowness
:class: note

Local models are slow on the first prompt while they load into memory. After
the initial "warm-up," responses are much faster. Don't judge performance
until after the first response.
:::


## Troubleshooting

### "Connection refused" or model not responding

1. Make sure Ollama is running:
   ```bash
   ollama serve
   ```
2. Check that the model is downloaded:
   ```bash
   ollama list
   ```
3. Test the model directly:
   ```bash
   ollama run qwen2.5-coder:7b "Hello"
   ```

### Autocomplete not appearing

1. Check VS Code settings: `editor.inlineSuggest.enabled` must be `true`
2. Disable other completion extensions (GitHub Copilot, etc.) that might conflict
3. Ensure `tabAutocompleteModel` is configured in Continue config

### Slow responses

- Use a smaller model (1.5b or 3b) for autocomplete
- Ensure you have enough RAM (close other applications)
- On NVIDIA GPUs, verify CUDA is being used (check Ollama logs)
- Consider using quantized models (Ollama does this automatically)

### Out of memory errors

- Use a smaller model
- Close memory-intensive applications
- On systems with dedicated GPU, ensure model fits in VRAM


## Alternative setups

### Cline (agentic alternative to Continue)

[Cline](https://github.com/cline/cline) is a VS Code extension that offers
more autonomous capabilities—it can edit files and run commands, not just
suggest code.

Install from VS Code marketplace, then configure for Ollama:

1. Open Cline settings
2. Select Ollama as provider
3. Choose your model

Cline is more powerful but also higher risk (it can modify files). Use with
appropriate caution.

### LM Studio (GUI alternative to Ollama)

[LM Studio](https://lmstudio.ai) provides a graphical interface for running
local models. It's easier for beginners but less scriptable.

1. Download from lmstudio.ai
2. Search for and download coding models
3. Start the local server
4. Configure Continue to use the LM Studio endpoint:

```json
{
  "models": [
    {
      "title": "LM Studio Model",
      "provider": "openai",
      "model": "local-model",
      "apiBase": "http://localhost:1234/v1"
    }
  ]
}
```

### llama.cpp (advanced)

For maximum control and efficiency, you can run [llama.cpp](https://github.com/ggml-org/llama.cpp)
directly. This is more complex but offers the best performance tuning.

```bash
# Clone and build
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
make

# Download a model (GGUF format) from HuggingFace
# Then run the server
./llama-server -m path/to/model.gguf --port 8080
```

Configure Continue to use it:

```json
{
  "models": [
    {
      "title": "llama.cpp Model",
      "provider": "openai",
      "model": "local",
      "apiBase": "http://localhost:8080/v1"
    }
  ]
}
```


## Comparison: Local vs. cloud models

| Aspect | Local (Ollama + Qwen2.5) | Cloud (GPT-4, Claude) |
|--------|--------------------------|----------------------|
| **Privacy** | Complete | Code sent to servers |
| **Cost** | Free after setup | Per-token charges |
| **Quality** | Good (88% HumanEval) | Better (90%+ HumanEval) |
| **Speed** | Depends on hardware | Generally faster |
| **Offline** | Yes | No |
| **Setup** | Required | None |
| **Context window** | Model-dependent (4K-32K typical) | Often larger (128K+) |

:::{admonition} When to use local vs. cloud
:class: tip

**Use local models when:**
- Working with sensitive/proprietary code
- Need offline capability
- Want to avoid API costs
- Institutional policy restricts cloud AI

**Use cloud models when:**
- Maximum quality is critical
- Working with very large codebases (need big context)
- Hardware is insufficient for local models
- Quick tasks where setup time isn't justified
:::


## Model recommendations by use case

| Use case | Recommended model | Why |
|----------|-------------------|-----|
| **General coding chat** | qwen2.5-coder:7b | Best quality/speed balance |
| **Fast autocomplete** | qwen2.5-coder:1.5b | Snappy tab completion |
| **Complex problems** | qwen2.5-coder:14b or 32b | Higher quality, needs more RAM |
| **Limited hardware** | starcoder2:3b | Runs on minimal resources |
| **Research/multilingual** | deepseek-coder:6.7b | Good multilingual support |


## Security considerations

:::{warning}
Running local models is safer from a data privacy perspective, but introduces
other considerations:

1. **Model provenance**: Only download models from trusted sources (Ollama
   library, HuggingFace official repos)
2. **System resources**: Models can consume significant CPU/GPU/RAM
3. **Network exposure**: By default, Ollama only listens on localhost. Don't
   expose it to the network without authentication
4. **Model output**: Local models can still generate insecure code—verification
   practices from the main course still apply
:::


## Further resources

### Official documentation
- [Ollama documentation](https://ollama.ai)
- [Continue documentation](https://docs.continue.dev)
- [Qwen2.5-Coder model card](https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct)

### Tutorials and guides
- [Ollama Blog: Continue code assistant](https://ollama.com/blog/continue-code-assistant)
- [Continue: Tab Autocomplete setup](https://docs.continue.dev/customize/deep-dives/autocomplete)
- [Local AI coding assistant guide](https://keyholesoftware.com/ollama-vs-code-your-guide-to-local-llm-development/)

### Model benchmarks and comparisons
- [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)
- [Best local coding LLMs (2025)](https://localllm.in/blog/best-local-llm-for-coding-2025)


:::{keypoints}
- Local AI coding requires: model runner (Ollama) + IDE extension (Continue) + model (Qwen2.5-Coder)
- Qwen2.5-Coder currently leads open-source coding benchmarks
- Use smaller models (1.5b-3b) for autocomplete, larger (7b+) for chat
- First response is slow while model loads; subsequent responses are faster
- Local models trade some quality for privacy, offline capability, and zero API costs
:::

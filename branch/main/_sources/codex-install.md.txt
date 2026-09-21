# Codex installation 

## Codex is a CLI -based AI agent by OpenAI, licensed under Apache 2.0 license.  

### First, install codex: 

- Open https://github.com/openai/codex/ 
- Install codex according to the instructions on the page above 
- WSL/Linux: npm install -g @openai/codex 
- Mac: brew install --cask codex 

### (optional) Set up non basic Model use (requires API endpoints to be openai compatible)


- Add the following to $HOME/.codex/config.toml – change the API key part 
  ```
  model = "<model_name>" 
  model_provider = "nonopenai" 
  model_reasoning_effort = "medium"  #(can be low medium or high)
 
  [model_providers.nonopenai] 
  name = "Non Open AI model" 
  base_url = "https://your.base.api/endpoint"  # the model needs to be served using /responses or /chat/completions under this endpoint.
  # the API to use (chat or responses)
  wire_api = "responses" 
  # Additional headers, e.g. for different auth scheme (otherwise the api key needs to be in the OPENAI_API_KEY environment variable)
  http_headers = { "Your-Auth-Header" = "YOUR_API_KEY_HERE" } 
  [analytics] 
  enabled = false # disables use of request for analytics
  [feedback] 
  enabled = false # Disables use for feedback
  ```

Finally, launch the codex installation, 

    Launch codex by $ codex in a folder that you want to edit. 

    Answer NO to the automatic edit mode (or whatever is in your folder WILL be modified)
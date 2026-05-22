+++
title="Codestral Configuration File" 
date=2026-05-22
[taxonomies]
tags=["AI", "Codestral", "Mistral Vibe", "Code Generation", "LLM", "Developer Tools", "Configuration File"]
+++


I have been experimenting with Mistral AI `codestral`. The coding model that is accessible through API at the following url: [codestral](https://console.mistral.ai/codestral).

The model comes with two endpoints: `chat` and `fim`(fill-in-the-Middle)). I have been able to successfully use these with the following addition to the `~./vibe/config.toml` file.

Here are working additions to `config.toml`:

```toml
[[providers]]
name = "codestral"
api_base = "https://codestral.mistral.ai/v1"
api_key_env_var = "CODESTRAL_API_KEY"
api_style = "openai"
backend = "mistral"

[providers.extra_headers]

[[models]]
name = "codestral-latest"
provider = "codestral"
api_base = "https://codestral.mistral.ai/v1/fim/completions"
alias = "codestral-fim"
completion_type = "fim"
temperature = 0.2
thinking = "off"
auto_compact_threshold = 200000
 
[[models]]
name = "codestral-latest"
provider = "codestral"
api_base = "https://codestral.mistral.ai/v1/chat/completions"
alias = "codestral-chat"
completion_type = "chat"
temperature = 0.7
thinking = "off"
auto_compact_threshold = 200000
```

or append to `config.toml`:

```bash
cat >> config.toml << 'EOF'

[[providers]]
name = "codestral"
api_base = "https://codestral.mistral.ai/v1"
api_key_env_var = "CODESTRAL_API_KEY"
api_style = "openai"
backend = "mistral"

[providers.extra_headers]

[[models]]
name = "codestral-latest"
provider = "codestral"
api_base = "https://codestral.mistral.ai/v1/fim/completions"
alias = "codestral-fim"
completion_type = "fim"
temperature = 0.2
thinking = "off"
auto_compact_threshold = 200000

[[models]]
name = "codestral-latest"
provider = "codestral"
api_base = "https://codestral.mistral.ai/v1/chat/completions"
alias = "codestral-chat"
completion_type = "chat"
temperature = 0.7
thinking = "off"
auto_compact_threshold = 200000
EOF
```

Alongside the `MISTRAL_API_KEY`, you'll need to add the `CODESTRAL_API_KEY` to the `~/.vibe/.env`.

A successful addition should result in:

![Codestral Config Endpoints (chat & fim)](/images/codestral-config.png)

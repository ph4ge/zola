+++
title="Codestral and Mistral Vibe: A Practical Guide to Code Generation and Chat" 
date=2026-05-16  
[taxonomies]
tags=["AI", "Codestral", "Mistral Vibe", "Code Generation", "LLM", "Developer Tools"]  
+++



## **Introduction**

Mistral AI’s **Codestral** is a powerful code-specialized model designed for **Fill-in-the-Middle (FIM) code completion** and **conversational chat** about code. When paired with **Mistral Vibe**, a command-line coding assistant, Codestral becomes a versatile tool for developers looking to **boost productivity** with AI-assisted coding.

In this post, we’ll explore:

1. **What Codestral is and why it stands out**.
2. **How to configure Codestral in Mistral Vibe**.
3. **Practical use cases for FIM and chat**.
4. **Troubleshooting common issues**.

Let’s dive in!

---

## **What is Codestral?**

Codestral is Mistral AI’s **first code-focused generative model**, optimized for:

- **Fill-in-the-Middle (FIM)**: Predicting missing code snippets in the middle of a function or block.
- **Code Explanation**: Providing conversational responses about code logic, best practices, and debugging.
- **Multi-Language Support**: Works with **80+ programming languages**.

### **Key Features**


| Feature           | Description                                                                   |
| ----------------- | ----------------------------------------------------------------------------- |
| **FIM Support**   | Infers missing code between `<fim_prefix>` and `<fim_suffix>` tokens.         |
| **Chat Support**  | Explains code, suggests improvements, and answers questions conversationally. |
| **Low Latency**   | Optimized for speed, making it ideal for real-time coding assistance.         |
| **Self-Hostable** | Available for local deployment via Hugging Face or Mistral’s API.             |


---

## **Setting Up Codestral with Mistral Vibe**

Mistral Vibe is a **terminal-based AI coding agent** that integrates seamlessly with Codestral. Here’s how to set it up.

---

### **Prerequisites**

1. A **Mistral AI account** with access to Codestral.
  - Sign up at [Mistral Console](https://console.mistral.ai).
  - Generate a **Codestral API key** (separate from the general Mistral API key).
2. **Mistral Vibe CLI** installed.
  - Clone the repo: `git clone https://github.com/mistralai/mistral-vibe.git`.
  - Install dependencies: `pip install -e .`.

---

### **Step 1: Configure the API Key**

Codestral uses a **separate API key** from the general Mistral API. Store it securely:

#### **Option A: Environment Variable (Recommended)**

Add to your `~/.bashrc` or `~/.zshrc`:

```bash
export CODESTRAL_API_KEY="your_codestral_api_key_here"
```

Reload your shell:

```bash
source ~/.bashrc
```

#### **Option B: `.env` File**

Create a `.env` file in `~/.vibe/`:

```ini
CODESTRAL_API_KEY=your_codestral_api_key_here
```

Load it in your session:

```bash
set -a && source ~/.vibe/.env && set +a
```

---

### **Step 2: Update `config.toml`**

Edit `~/.vibe/config.toml` to add the **Codestral provider and model**:

```toml
# Provider for Codestral
[[providers]]
name = "codestral"
api_base = "https://codestral.mistral.ai/v1"  # Base URL for Codestral
api_key_env_var = "CODESTRAL_API_KEY"         # Uses the env var we set
api_style = "openai"
backend = "mistral"

[providers.extra_headers]

# Single model definition for Codestral
[[models]]
name = "codestral-latest"  # Must match the API's expected model name
provider = "codestral"
api_base = "https://codestral.mistral.ai/v1/fim/completions"
alias = "codestral-fim"        # Default alias for easy reference
completion_type = "fim"     # Default to FIM (optional)
temperature = 0.2
thinking = "off"

[[models]]
name = "codestral-latest"
provider = "codestral"
api_base = "https://codestral.mistral.ai/v1/chat/completions"
alias = "codestral-chat"
completion_type = "chat"
temperature = 0.7
thinking = "off"
```

#### **Key Notes**

- **Model Name**: Must be `codestral-latest` (the API rejects custom names like `codestral-fim`).
- **Provider**: The `api_base` points to `codestral.mistral.ai/v1`, not `api.mistral.ai`.
- **Completion Type**: Vibe uses this to route to `/fim/completions` or `/chat/completions`.

---

### **Step 3: Verify the Setup**

Test Codestral with **FIM** and **chat**:

#### **FIM (Code Completion)**

```bash
VIBE_ACTIVE_MODEL=codestral-fim vibe --prompt "<fim_prefix>def calculate_sum(a, b):\n    <fim_suffix>"
```

**Expected Output**:

```python
def calculate_sum(a, b):
    return a + b
```

#### **Chat (Conversational)**

```bash
VIBE_ACTIVE_MODEL=codestral-chat vibe --prompt "Explain this function: def calculate_sum(a, b): return a + b"
```

**Expected Output**:

```
This function takes two arguments, `a` and `b`, and returns their sum. It performs a basic arithmetic operation and assumes `a` and `b` are numeric values (e.g., integers or floats).
```

---

## **How Codestral Routes Requests**

Codestral’s **endpoint routing** is automatic based on the task:


| **Task Type** | **Endpoint**           | **Trigger**                       | **Example Prompt**                          |
| ------------- | ---------------------- | --------------------------------- | ------------------------------------------- |
| **FIM**       | `/v1/fim/completions`  | `<fim_prefix>` and `<fim_suffix>` | `<fim_prefix>def add(x, y):\n <fim_suffix>` |
| **Chat**      | `/v1/chat/completions` | Natural language questions        | "Explain this Python function: ..."         |


### **Why This Matters**

- You **don’t need separate models** for FIM and chat. The same model (`codestral-latest`) handles both.

---

## **Common Pitfalls and Fixes**


| **Issue**                      | **Cause**                         | **Solution**                                                                        |
| ------------------------------ | --------------------------------- | ----------------------------------------------------------------------------------- |
| `Invalid model: codestral-fim` | Custom model names not recognized | Use `name = "codestral-latest"` in `config.toml`.                                   |
| `401 Unauthorized`             | Wrong or missing API key          | Ensure `CODESTRAL_API_KEY` is set and matches the key in the Mistral Console.       |
| `400 Bad Request`              | Incorrect endpoint or model name  | Use `api_base = "https://codestral.mistral.ai/v1"` and `name = "codestral-latest"`. |
| FIM not working                | Missing FIM tokens                | Include `<fim_prefix>` and `<fim_suffix>` in your prompt.                           |
| Chat not working               | Using FIM model for chat          | Use natural language prompts (no FIM tokens) for chat.                              |


---

## **Advanced: Automating Workflows**

### **Shell Aliases**

Add these to your `~/.bashrc` or `~/.zshrc` for quick access:

```bash
# Codestral FIM
alias vibe-codestral-fim='VIBE_ACTIVE_MODEL=codestral-fim vibe --prompt'

# Codestral Chat
alias vibe-codestral-chat='VIBE_ACTIVE_MODEL=codestral-chat vibe --prompt'
```

Usage:

```bash
vibe-codestral-fim "<fim_prefix>def add(x, y):\n    <fim_suffix>"
vibe-codestral-chat "Explain this function: def add(x, y): return x + y"
```

### **Python Script for Direct API Calls**

If you need **programmatic control**, use Mistral’s API directly:

```python
import requests
import os

api_key = os.getenv("CODESTRAL_API_KEY")
headers = {"Authorization": f"Bearer {api_key}", "Content-Type": "application/json"}

# FIM Request
fim_response = requests.post(
    "https://codestral.mistral.ai/v1/fim/completions",
    headers=headers,
    json={
        "model": "codestral-latest",
        "prompt": "<fim_prefix>def add(x, y):\n    <fim_suffix>",
        "max_tokens": 20
    }
)

# Chat Request
chat_response = requests.post(
    "https://codestral.mistral.ai/v1/chat/completions",
    headers=headers,
    json={
        "model": "codestral-latest",
        "messages": [{"role": "user", "content": "Explain this function: def add(x, y): return x + y"}]
    }
)

print("FIM Response:", fim_response.json()["choices"][0]["message"]["content"])
print("Chat Response:", chat_response.json()["choices"][0]["message"]["content"])
```

---

## **Why Codestral Stands Out**

1. **Speed and Efficiency**:
  - Optimized for **low-latency responses**, making it ideal for real-time coding assistance.
2. **Multi-Language Support**:
  - Works with **80+ programming languages**, from Python to Rust.
3. **Flexibility**:
  - Supports **both FIM and chat**, so you can use it for code completion *and* explanations.
4. **Integration-Friendly**:
  - Works seamlessly with **Mistral Vibe**, **Continue.dev**, and **JetBrains IDEs**.

---

## **Conclusion**

Codestral is a **game-changer** for developers who want **AI-assisted coding** without sacrificing control. By configuring it correctly in Mistral Vibe, you can:

- **Complete code snippets** with the `fim` endpoint.
- **Get conversational explanations** of complex logic with the `chat` endpoint.
- **Automate repetitive tasks** like writing tests or documentation.

---

### **Resources**

- [Mistral AI Console](https://console.mistral.ai)
- [Mistral Vibe GitHub](https://github.com/mistralai/mistral-vibe)
- [Codestral Documentation](https://mistral.ai/news/codestral)

---

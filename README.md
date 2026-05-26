# Ask I Want (Flow Launcher Plugin)

> Forked By [BowieHe/ask-ai-plugin](https://github.com/BowieHe/ask-ai-plugin)

Ask I Want is a [Flow Launcher](https://www.flowlauncher.com/) plugin that brings AI assistants to your fingertips. It supports both local models via Ollama and cloud providers via OpenAI-compatible APIs, with optional web search and thinking/reasoning mode.

## Features

- **Multi-provider support**: Local models via Ollama, cloud models via any OpenAI-compatible API (OpenAI, DeepSeek, etc.)
- **Thinking mode**: Display the model's reasoning process with `[thinking]` and `[answer]` markers
- **Web search**: Optional Tavily integration for up-to-date information — auto-triggered by the model or forced with prefixes (`net`, `web`, `search`, `联网`)
- **Copy to clipboard**: One-click copy of the answer
- **Open in text editor**: View the full response (including thinking) in a temporary text file
- **Customizable system prompt**: Define the AI's behavior and role
- **Lightweight**: Dependencies are bundled in `lib/`, no extra install needed

## Requirements

- Windows/Linux with Flow Launcher
- Python 3.10+
- **For Ollama**: Ollama running locally (or on a reachable host)
- **For OpenAI-compatible APIs**: Valid API key and endpoint URL
- Tavily API key (optional, only needed for web search)

## Configuration

In Flow Launcher settings for this plugin:

### Provider Settings

| Setting | Description | Default |
| --- | --- | --- |
| **API Type** | `Ollama` uses native `/api/chat`, `OpenAI Compatible` uses `/v1/chat/completions` | `ollama` |
| **Model Name** | Model to use (e.g. `qwen3:4b`, `gpt-4o`, `deepseek-chat`) | `qwen3:4b` |
| **API Base URL** | API endpoint URL | `http://localhost:11434` |
| **API Key** | Required when API Type is `openai_compatible`; leave empty for Ollama | _(empty)_ |

Use the **API Type** dropdown to explicitly switch between Ollama and OpenAI-compatible APIs. When upgrading from an older version, if `api_type` is not set, the plugin falls back to auto-detection based on the URL (`localhost`/`127.0.0.1` → Ollama).

### Feature Settings

| Setting                          | Description                                         | Default                        |
| -------------------------------- | --------------------------------------------------- | ------------------------------ |
| **Enable thinking/reasoning**    | Show model reasoning process (for supported models) | On                             |
| **Enable web search by default** | Let the model auto-trigger web searches             | Off                            |
| **Tavily API Key**               | Required for web search                             | _(empty)_                      |
| **Max Search Results**           | Number of search results (1-3)                      | `3`                            |
| **Response preview length**      | Characters shown in the result title (40-500)       | `160`                          |
| **System Prompt**                | Custom system prompt to define AI behavior          | `You are a helpful assistant.` |
| **Use system proxy**             | Enable if behind a corporate proxy                  | Off                            |

## Usage

### Basic Questions

```
ask what is function calling
ask 什么是函数调用
```

Press Enter on the result to send your question.

### Web Search

Force web search with these prefixes:

```
ask net latest AI news
ask web today's weather in Beijing
ask search Python 3.13 release notes
ask 联网 今天北京的天气
```

Or enable "web search by default" in settings to let the model decide when to search.

### After Getting an Answer

- **Primary result**: Shows a preview of the answer — press Enter to close
- **Copy it**: Copies the answer to clipboard
- **Open Full Response in Text Editor**: Opens the complete response (with thinking process if enabled) in a temporary `.txt` file

### Response Format

When thinking mode is enabled:

```
[thinking]
Let me analyze this step by step...

[answer]
Based on my reasoning, the answer is...
```

When web search was used:

```
[answer]
Here's what I found...

[Web Search Results]
- Article Title
  https://example.com
  Summary of the article...
```

## How It Works

1. Your query is sent to the configured provider (Ollama or OpenAI-compatible API)
2. If thinking is enabled, the model's reasoning is captured and displayed
3. If web search is enabled (or forced via prefix), the model can call the `search_web` tool
4. When search is triggered, the plugin queries Tavily and feeds results back to the model
5. The model returns a final answer, with search results appended if applicable

## Troubleshooting

### Provider Issues

- **Ollama error**: Make sure Ollama is running and reachable at the configured URL
- **API key error**: Verify your API key is correct and has sufficient credits
- **Model not found**: For Ollama, run `ollama pull <model_name>`. For cloud APIs, check the model name spelling.

### Web Search Issues

- **Search not working**: Ensure Tavily API key is set in settings
- **Search timeout**: Check your network connection
- **Results not showing**: Make sure "Enable web search" is enabled, or use a search prefix

### Other Issues

- **Settings not showing**: Clear Flow Launcher cache and restart
- **Proxy issues**: Enable "Use system proxy" if behind a corporate proxy
- **Thinking not showing**: Enable "Enable thinking" in settings and use a model that supports it

## Files

| File                    | Purpose                                                 |
| ----------------------- | ------------------------------------------------------- |
| `main.py`               | Core plugin logic                                       |
| `plugin.json`           | Plugin metadata and action keyword                      |
| `SettingsTemplate.yaml` | Settings UI definition                                  |
| `requirements.txt`      | Python dependencies (`flowlauncher`, `requests`)        |
| `lib/`                  | Bundled dependencies (requests, urllib3, certifi, etc.) |

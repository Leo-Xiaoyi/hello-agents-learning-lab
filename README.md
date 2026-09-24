# Hello Agents Learning Lab

Notes and Python exercises from my completed study of Datawhale's Hello-Agents tutorial, focused on agent fundamentals, tool use, and practical LLM application patterns.

The repository includes a weather and travel assistant exercise, a shared LLM client, and learning notes. API keys and local environments are excluded from Git.

## Learning Source

- Upstream project: [datawhalechina/hello-agents](https://github.com/datawhalechina/hello-agents)

## Published Exercises

| Folder | Focus |
| --- | --- |
| `Try_code/C1T1_WeatherTravelAssistant` | ReAct-style weather and travel assistant using tools and an OpenAI-compatible LLM client. |
| `Try_code/C4T1_AgentParadigms` | Shared OpenAI-compatible LLM client for agent paradigm exercises. |

## Agent and LLM Knowledge

Familiar with agentic workflows and LLM application concepts, including RAG, tool/function calling, LangGraph and LangChain, MCP, embeddings and vector search, prompt and context engineering, short- and long-term memory, agent evaluation and guardrails, multi-agent orchestration, ReAct, and Plan-and-Solve.

## Hands-on Practice

- Calling OpenAI-compatible chat completion APIs from Python.
- Managing model, base URL, and API keys through environment variables.
- Building tool-using agent loops with Thought/Action/Observation style prompting.
- Connecting simple external tools such as weather lookup and Tavily search.
- Keeping learning code reproducible without committing secrets or local environments.

## Setup

Create a virtual environment and install dependencies for the exercise you want to run.

```sh
cd Try_code/C1T1_WeatherTravelAssistant
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Then fill in `.env` with your own API keys and endpoint settings.

## Run

```sh
python main.py
```

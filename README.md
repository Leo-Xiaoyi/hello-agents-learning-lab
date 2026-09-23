# Hello Agents Learning Lab

Personal learning lab based on the open-source Datawhale Hello-Agents tutorial, which I have completed. This repository contains selected early exercises, not an implementation of every chapter.

This repository keeps a small selection of practice code and notes. The upstream tutorial package, PDF, virtual environment, local cache files, and private API configuration are kept out of Git.

## Learning Source

- Upstream project: [datawhalechina/hello-agents](https://github.com/datawhalechina/hello-agents)
- This repository is a personal study workspace, not a fork or redistribution of the full tutorial.

## Published Exercises

| Folder | Focus | Status |
| --- | --- | --- |
| `Try_code/C1T1_WeatherTravelAssistant` | A simple ReAct-style weather and travel assistant using tools and an OpenAI-compatible LLM client. | Initial practice version |
| `Try_code/C4T1_AgentParadigms` | Shared OpenAI-compatible LLM client for agent paradigm exercises. | Initial setup |

## Agent and LLM Knowledge

Familiar with agentic workflows and LLM application concepts, including RAG, tool/function calling, LangChain and LangGraph, MCP, embeddings and vector search, prompt and context engineering, short- and long-term memory, agent evaluation and guardrails, multi-agent orchestration, ReAct, and Plan-and-Solve. These describe my knowledge of the field; the public code here is a small selection of tutorial exercises.

## What This Shows

- Calling OpenAI-compatible chat completion APIs from Python.
- Managing model, base URL, and API keys through environment variables.
- Building small tool-using agent loops with Thought/Action/Observation style prompting.
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

## Repository Policy

The root folder may contain local-only study material, including the original tutorial download and local virtual environment. Git intentionally tracks only my public learning exercises and notes.

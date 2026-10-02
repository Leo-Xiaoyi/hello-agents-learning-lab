# Hello Agents Learning Lab

My notes from studying [Datawhale's Hello-Agents](https://github.com/datawhalechina/hello-agents), covering Chapters 1–10. I spent more time on Chapters 4–10, especially agent workflows, architecture, information flow, and design trade-offs. Closely related chapters share a note.

These are my working notes, not a walkthrough of the tutorial code. I focus on what each part does and why it is there; diagrams and pseudocode are enough for most implementation details.

The original Chinese notes are preserved in the [Chinese edition / 中文版](README.zh.md).

## Chapter notes

| Chapters | Note | Question I worked through |
| --- | --- | --- |
| 1–3 | [How an agent works](notes/en/01-03-agent-basics.md) | The agent loop, earlier approaches, and what an LLM can actually see |
| 4 | [ReAct, Plan-and-Solve, and Reflection](notes/en/04-agent-patterns.md) | When to act while reasoning, plan first, or review an answer |
| 5–6 | [Low-code platforms and agent frameworks](notes/en/05-06-platforms-and-frameworks.md) | How to organise workflows, state, and collaboration |
| 7 | [The layers of an agent framework](notes/en/07-agent-framework.md) | What the model, messages, agent, configuration, and tools each do |
| 8 | [Memory and RAG](notes/en/08-memory-and-rag.md) | How past interactions and external documents are stored and retrieved |
| 9 | [Context engineering](notes/en/09-context-engineering.md) | What to put in front of the model for a particular call |
| 10 | [MCP, A2A, and ANP](notes/en/10-agent-protocols.md) | How applications, tools, and independent agents connect |

[Learning log (English)](learning-log.en.md) · [学习记录（中文版）](learning-log.md)

## Python exercises

| Directory | What I kept |
| --- | --- |
| [`Try_code/C1T1_WeatherTravelAssistant`](Try_code/C1T1_WeatherTravelAssistant) | Weather and travel assistant exercise using tools and an OpenAI-compatible model interface. |
| [`Try_code/C4T1_AgentParadigms`](Try_code/C4T1_AgentParadigms) | Shared model interface for the Chapter 4 agent-pattern exercises. |

Local commands for the weather and travel exercise (API settings go in `.env`):

```sh
cd Try_code/C1T1_WeatherTravelAssistant
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python main.py
```

The `.env` file and local virtual environments are excluded from Git.

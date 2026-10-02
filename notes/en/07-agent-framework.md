# Chapter 7 | The Layers of an Agent Framework

Source: [Hello-Agents Chapter 7](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter7). I focused on which logic can be shared and how one task moves through the layers.

[中文版](../07-agent-framework.md)

## From one loop to a reusable system

The Chapter 4 examples can put the whole “question → choose a tool → execute → answer” loop in one program. As the project grows, changing models, agent patterns, or tools repeatedly touches the same message handling and error handling. Separating those shared responsibilities lets different agents reuse the same foundation.

```text
User / application
    ↓
Agent (task entry point, execution loop, stopping conditions)
    ├── Message / History (what has happened in this turn)
    ├── LLM interface (send input to the model and receive its response)
    ├── Tool Registry / Executor (find and run allowed tools)
    └── Config (model and runtime settings)
```

The files do not have to be arranged identically in every framework, but something must take responsibility for each part. A common interface lets multiple agent patterns share model calls, messages, and tool access.

## How one request moves through the layers

1. The application passes a task to an agent. The agent prepares model input from the task, system requirements, and relevant history.
2. The LLM interface handles the model request and returns either an answer or an intention to call a tool.
3. If a tool is needed, the agent checks the registry and passes the arguments to the executor. The result is added to the message history.
4. The agent calls the model again with that result, allowing it to continue or answer.
5. Once a stopping condition is met, the agent returns the result and retains only the state needed for a later turn.

The boundary I keep in mind: the model proposes an action; the program executes it. Tool Registry lists what is available, while the execution layer makes the call and deals with failures or timeouts.

## Responsibilities of each layer

- **Unified LLM interface:** keeps provider-specific connection details out of agent logic. Switching models should disturb the upper layers as little as possible, although differences in tool calling and output still matter.
- **Message:** gives system, user, assistant, and tool content a common representation. History can then be stored, filtered, and converted into the format required by a model API.
- **Config:** keeps adjustable values such as model choice, temperature, and history length in one place instead of hard-coding them in every agent.
- **Base agent and concrete patterns:** present a similar entry point while implementing ordinary chat, ReAct, planning, or reflection differently inside.
- **Tool system:** standardises names, descriptions, inputs, and outputs. The registry answers “what tools exist?”; the executor answers “what happened when one ran?” Memory, retrieval, and protocol connections can also be exposed through this common entry point.

The tutorial's “everything is a tool” idea is most useful to me as a common way for an agent to discover and call capabilities. Their internals remain different: memory manages stored state, RAG manages documents and retrieval, and protocol-based tools handle remote connections.

## Trade-offs and debugging

Layers add files and interfaces, so a small one-off script may not need a full framework. The abstraction earns its place when several agents share model, message, or tool logic. It should not hide the actual flow: when something breaks, I still need to tell whether the problem came from model output, message conversion, tool selection, or execution.

If an agent picks the wrong tool, I would follow the chain: Was the description shown to the model clear? Could the returned tool name and arguments be parsed? Was the tool registered? Did its result make it back into the messages correctly? That is more useful than changing the prompt without knowing where the failure occurred.

## The execution path I retained

A task enters through the agent. The model returns either an answer or a tool-call intention. The program checks and executes the action, then sends the result back as a message in the next turn. Separating these responsibilities lets agents share lower-level capabilities and makes model changes, new tools, and debugging more contained.

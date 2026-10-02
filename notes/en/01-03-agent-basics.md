# Chapters 1–3 | How an Agent Works

My basic understanding of agents, language models, and fixed workflows. Source: [Hello-Agents](https://github.com/datawhalechina/hello-agents), Chapters 1–3.

[中文版](../01-03-agent-basics.md)

## Chapter 1: What an agent does

After receiving a task, an agent can call a model and its tools more than once. The model uses the information available now to propose the next step. The program keeps track of state, executes tools, and decides when to stop. The continuing loop is what makes this different from a single question-and-answer call.

```text
User task
  → Assemble the current state and available tools
  → Model proposes the next step: answer / call a tool / continue
  → Program executes the step and gets a result
  → Put the result back into the current state
  → Continue or stop
```

One model call might only complete one step. Once a tool result is fed into the next call, the agent can revise its earlier judgement.

### Model, tools, state, and control flow

- Model: interprets the goal, chooses a next step, or produces an answer. A proposed tool call still needs the program to execute it.
- Tools: carry out actions such as search, calculation, and file reading. Their results need checking rather than automatic acceptance.
- State: keeps the task's progress, previous actions, tool results, and the history needed for the next call.
- Control flow: checks whether an action is valid, handles errors, and sets step limits and stopping conditions.

I now check an agent design with four questions: What can the model see? What can it do? How do action results come back? When does the process stop or hand over to a person?

### Fixed workflow versus agent

In a fixed workflow, the developer specifies most steps and branches in advance. An agent leaves some decisions about the next step to the model. They can coexist: a fixed outer workflow can set the boundaries while an agent chooses tools within a step that requires exploration.

For a stable path, I would make the steps explicit. If the next step depends on new information, the model needs room to choose. More autonomy also means more need for recorded state, tool permissions, and stopping rules.

## Chapter 2: From rule-based agents to LLM-based agents

The chapter traces how agents developed. Earlier systems relied on predefined rules. They could handle cases their designers anticipated but struggled outside those rules. Language models make it possible to choose a next step more flexibly from a text description and the information at hand.

The model's flexibility does not remove the rest of the program. Tools still carry out external actions, and the program still manages state, permissions, and errors. LLMs change how some decisions are made; they do not take over every responsibility.

## Chapter 3: What the model sees on each call

The point that helped me most is that a model responds to the content supplied in this particular request. A previous conversation does not automatically appear in a new request, and the model does not read local files on its own. The application must assemble relevant history, file excerpts, and tool results before the call.

A model can propose a plan and answer questions, but it can also produce unsupported or incorrect claims. The application has to decide what information goes in and whether the output has adequate support. I keep “can generate an answer” separate from “has reliable evidence.”

## My outline of one question-answer cycle

For a task that needs research before an answer, I wrote the cycle this way:

1. Receive the task and identify the goal, requirements, and available tools.
2. Retrieve useful information from history, memory, documents, and notes; assemble the model's input.
3. The model decides whether to answer, call a tool, or continue a step.
4. The program checks and executes the action, then adds its result to the current state.
5. The model continues or stops based on the new result; the program handles errors and step limits.
6. After the turn, keep the necessary history and any information worth retaining longer term.

Not every task needs memory, retrieval, or every tool. This outline helps me keep information retrieval, action selection, and action execution distinct.

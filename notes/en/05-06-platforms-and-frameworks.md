# Chapters 5–6 | Low-Code Platforms and Agent Frameworks

Sources: [Hello-Agents Chapter 5](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter5) and [Chapter 6](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter6). I kept platforms and code frameworks together to compare how they control a workflow.

[中文版](../05-06-platforms-and-frameworks.md)

## Both organise the workflow

The hand-written agent loop in Chapter 4 makes the sequence visible: the model proposes an action, the program executes it, and the result comes back. A larger task also needs tool connections, state, branches, collaboration, and debugging. Platforms and frameworks both help organise these concerns, but they expose different levels of control to the developer.

```text
Task goal
  ├─ Platform: configure nodes, a knowledge base, and external connections
  │            to assemble a runnable workflow more quickly
  └─ Code framework: define state, roles, nodes, edges, and execution rules
                         ↓
              Underneath: model, tools, state, and control loop
```

My choice would depend on whether existing features cover the task, how much of the flow needs to be customised, and how I would debug and maintain it.

## Chapter 5: configuring a workflow on a platform

The four platforms left different impressions on me. These are the aspects emphasised by the tutorial's examples:

- Coze: assemble a conversational assistant fairly quickly with existing models, prompts, and plugins.
- Dify: bring question answering, a knowledge base, and tools into one AI application.
- FastGPT: focus more closely on document ingestion, chunking, retrieval, and question answering.
- n8n: connect event triggers and data movement between systems into a fixed workflow.

These are observations about the examples, not complete feature lists. Instead of memorising product features, I want to know which parts the model decides, which parts follow fixed rules, and whether a failure can be traced to a particular step. When the whole path is stable, an explicit workflow is usually easier to inspect.

## Chapter 6: expressing collaboration and state in code

Frameworks organise agents through code. I noted the different ways the four examples express collaboration:

| Perspective | Frameworks in the tutorial | Design focus |
| --- | --- | --- |
| Multi-role conversation | AutoGen, CAMEL | Divide roles and let messages drive collaboration; watch for drift and termination |
| Message and multi-agent management | AgentScope | Standardise messages, roles, and runtime management |
| Explicit state and flow | LangGraph | Represent steps as nodes and transitions as edges while passing state through the flow |

In LangGraph, a node is a processing step rather than another model. An edge determines where execution goes next, and state carries information shared between steps. Conditional edges make a return to search, regeneration, or termination visible in the code. Checkpoints can also preserve where a long-running process got to.

Multi-role conversation puts more weight on the messages exchanged between roles. It can leave the flow more open, but then message content and termination rules matter more. I would not treat open collaboration and explicit state control as a simple ranking.

## My approach to choosing one

1. Sketch the input, output, required steps, and likely failure paths before picking a product.
2. For a stable path that mostly connects existing systems, consider an explicit workflow and platform configuration.
3. For intricate state, loops, or precise control, use a code framework to express the important branches.
4. If the next step depends on a new observation, allow agent-style decisions at the relevant point.

In the end, I want to be able to describe the actual flow: what is fixed, what the model decides, and where I would look when something fails.

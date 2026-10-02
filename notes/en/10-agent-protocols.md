# Chapter 10 | MCP, A2A, and ANP

Source: [Hello-Agents Chapter 10](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter10). I distinguish the three protocols by what they connect.

[中文版](../10-agent-protocols.md)

## Connecting different systems

An agent can register a local function as a tool. But if every external service requires a separate convention for tool descriptions, inputs, connections, and errors, integrations become costly. Working with another independent agent also requires a way to describe its capabilities, submit a task, and receive a result. Protocols provide reusable agreements for these interactions.

Three different connections:

```text
Agent-hosting application ── MCP ── Tools / resources / prompts
Agent A                   ── A2A ── Agent B's capabilities and tasks
Agents on an open network ── ANP ── Identity, description, discovery, connection
```

The application still manages permissions, state, and business logic.

## MCP: connect external capabilities to the agent's application

MCP uses a Host–Client–Server structure. The Host is the application running the agent; it manages client connections to servers. A Server exposes capabilities. Besides executable **Tools**, a server can expose readable **Resources** and reusable **Prompts**.

```text
Application connects to Server → Discover capabilities → Call or read
                               → Receive result → Application decides how to use it
```

I separate the protocol from task control. MCP specifies how capabilities and data are exchanged; the agent and application still decide what to call, how to use it, and what permissions apply. A server's response does not automatically become model input. The application must choose which parts enter the current context.

## A2A: give a task to another agent

Another agent may have its own tools, state, and long-running process. A2A addresses capability descriptions, task submission, message exchange, and result delivery. I picture the interaction like this:

```text
Discover the other agent's capabilities → Submit a task and necessary context
                                       → Track status / exchange messages
                                       → Receive a result or artifact
```

This fits collaboration with an agent that has an independent responsibility. I would still ask how much context to send, how task completion is reported, who retries a failure, and who checks the result. A complex task does not automatically need multiple agents; a clear workflow with a few tools may be simpler to run.

## ANP: discovery and interconnection on an open network

The tutorial places ANP in a more open network setting: how an agent states its identity and capabilities, how another system finds it, and how they establish a trusted connection. Its concerns include identity, capability descriptions, service discovery, and cross-domain interaction. A2A and ANP both involve agents connecting to agents, but A2A is more directly about task collaboration, while ANP focuses on the identity and discovery needed in an open network.

I would not map the protocols to “small, medium, and large systems.” MCP connects an application to external capabilities; A2A supports task interactions between agents; ANP concerns the conditions for interconnection across an open network. The relevant question is which system boundary needs to be crossed.

## Where protocols fit in the rest of the system

Protocols handle the connection between systems. After they connect, the agent loop still chooses the next step; the tool system manages capabilities; memory and RAG supply information; context engineering selects what the model sees. The application still has to judge whether a result is trustworthy.

## References I checked

- [MCP architecture](https://modelcontextprotocol.io/docs/learn/architecture)
- [A2A key concepts](https://a2a-protocol.org/latest/topics/key-concepts/)
- [ANP getting started guide](https://agent-network-protocol.com/docs/anp-getting-started-guide)

I now distinguish them by connection target: MCP for tools and resources, A2A for task collaboration between independent agents, and ANP for identity and discovery on an open network. Task control and context selection remain with the application.

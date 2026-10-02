# Chapter 9 | Context Engineering

Source: [Hello-Agents Chapter 9](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter9). My understanding of context engineering is deciding what the model sees before each call.

[中文版](../09-context-engineering.md)

## What goes into one model call

A prompt is only part of the input. A call may also include system requirements, the current question, conversation history, relevant memory, retrieved documents, notes, and recent tool results. Context engineering chooses which of these to include now and how to arrange them.

More information is not always better. It uses tokens, can bury what matters, and may bring in stale or contradictory material. The system needs both good retrieval and a willingness to leave irrelevant information out.

```text
User question
   ↓
Gather candidates from history, memory, RAG, notes, and tool results
   ↓
Select by relevance and budget → Organise into sections → Check final length
   ↓
Constructed context + current question → Model → Answer
   ↓
If needed, save important new information to memory or notes
```

The constructed context is input prepared for the model, not the model's answer. Whether to write something into long-term records is a separate decision made afterwards.

## Can every agent share one ContextBuilder?

Yes. If each agent assembles its own context, adding a memory source or changing a budget requires edits in multiple places. `ContextBuilder` gives them a shared entry point and keeps the selection and organisation policy in one place.

I also asked why each candidate must be wrapped in a `ContextPacket`, with its token count recorded. Candidates need to be compared, ranked, and selected within a budget. A common structure and length estimate make that possible. `ContextConfig` holds budgets, thresholds, and weights centrally, so changing the policy does not mean hunting for parameters in every agent.

## Gather → Select → Structure → Compress

**Gather:** Find candidate information from recent conversation, relevant memories, RAG documents, existing notes, and other tools. Candidates are not sent to the model yet. Their sources need to remain distinct: something the user once said should not be blended into a passage retrieved from an external document. If one retrieval source fails, the others can still return useful information; the failure should not be mistaken for “nothing relevant exists.”

**Select:** Keep the system requirements, then judge other candidates by relevance, recency, importance, and token cost. The tutorial example mainly scores relevance and recency before selecting within the budget. Recent does not always mean important: an older constraint central to the task may outrank a new but incidental message. If the Builder repeatedly selects irrelevant content, where should I look first? At candidate retrieval and relevance scoring, not at simply raising the token budget.

**Structure:** Put information in distinct sections for rules and role, task, state, evidence, context, and output requirements. Retrieved external documents belong in Evidence; conversation history and memories belong in Context. This makes it easier to see what is an instruction, what is a documentary source, and what happened earlier, including when one section is causing a bad answer.

**Compress:** Check the length after assembling the final input. Individual candidates can each fit while the combined text, headings, task, and output requirements exceed the budget. The final pass compresses or truncates when needed, while trying to retain the goal, important constraints, and supporting evidence.

## Retrieve details when needed

Progressive disclosure presents a trade-off I recognise: keep a lightweight index, path, or directory first, and open detailed material with tools only when the task requires it. This saves context but can make lookup slower or send the agent down the wrong path. My working compromise is to include the essential rules and project outline up front, then allow deeper lookup on demand.

`NoteTool` can hold stage conclusions, next steps, and blockers for a long-running task. Its index helps locate a note, while the actual content remains in Markdown. The tutorial's simple search should not be confused with full semantic understanding. `TerminalTool` can inspect files and logs as needed, but its commands and access scope need limits. `NoteTool`, `TerminalTool`, `MemoryTool`, and `RAGTool` differ in what they access and when their information is useful.

For a task spanning many turns, my question was how to keep stage results once the context becomes full. One option is to compress earlier conversation while retaining decisions, progress, and blockers. Another is to write structured notes periodically and retrieve them as needed. If the task can be divided, sub-agents can handle separate parts and report back. These address continuity across turns. The Compress stage of GSSC handles a different problem: a single assembled input that is too long.

## The order I retained

Before a model call, gather candidates for the current question, select within the relevance and token budget, keep rules, task, external evidence, and history in separate sections, and check the assembled length. After the answer, decide separately whether any new information belongs in longer-term records.

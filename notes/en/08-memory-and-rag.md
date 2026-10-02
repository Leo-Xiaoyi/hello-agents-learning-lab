# Chapter 8 | Memory and RAG

Source: [Hello-Agents Chapter 8](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter8). I keep memory and RAG separate in my notes: one handles information left by interactions; the other handles external documents.

[中文版](../08-memory-and-rag.md)

## Where the two kinds of information come from

A model call only processes what it receives in that call. Recent messages may sit in the program's conversation history, but a new session cannot automatically recover them after a restart or a change of conversation. The model also does not automatically have the user's documents or up-to-date external material.

Two information paths:

```text
Past interactions, preferences, events → Memory: save, maintain, retrieve as needed
External documents and knowledge base  → RAG: preprocess, index, retrieve by question
                                                   ↓
                                      Relevant results enter this turn's context
```

Both can retrieve information, but they manage different sources. A fact established in a conversation belongs to memory; a passage from a manual belongs to the external knowledge base. Keeping them separate also makes it easier to tell whether an answer rests on what the user previously said or on a traceable document.

## The basic memory flow

```text
New interaction → Decide whether to remember it → Store by type
New question    → Retrieve relevant memories       → Add to the current context
Over time       → Expire, consolidate, or correct old memories
```

The four memory types differ in what they retain and how they are retrieved:

| Type | What it mainly holds | Design concern |
| --- | --- | --- |
| Working memory | Temporary information for the current task or conversation | Quick access, size limits, expiry |
| Episodic memory | Specific events or interactions | Recovering the event, time, and context |
| Semantic memory | More stable concepts, rules, and preferences | Consolidation, validity, conflicting information |
| Perceptual memory | Images, audio, and other multimodal information | Representation and cross-modal retrieval |

### A question I asked: Don't Conversation History and MemoryTool overlap?

They can hold similar recent information, but their responsibilities differ. `conversation_history` is the message record the agent can use to continue the current conversation. `MemoryTool` provides a way to write and retrieve memories when needed, including the lifetimes of different memory types. Working memory may overlap with conversation history. Episodic memory preserves selected events from interactions rather than a permanent copy of every message.

A useful memory system also has to maintain what it stores. Old information may no longer be true, preferences may change, and repeated events may need consolidation. Accumulating everything indefinitely would cause retrieval to surface stale material.

## RAG has two pipelines

While preparing the knowledge base:

```text
Documents → Extract and normalise text → Chunk at suitable boundaries
          → Add searchable representations and source metadata → Store
```

While answering a question:

```text
Question → Retrieve candidate chunks → Select relevant evidence
         → Send it with the question to the model → Generate an evidence-based answer
```

In the tutorial, documents are first converted to Markdown and then chunked with help from headings and other structure. I keep chunking separate from vectorisation: the first organises text; the second gives it a representation for one kind of retrieval. Large chunks may bring irrelevant content along; very small chunks may sever an idea. Titles and sources should stay attached to the chunks.

Retrieval need not use just one method. Keyword matching helps with exact terms. Vector retrieval can use semantic similarity, but may miss precise names or return loosely related material. The tutorial also covers Multi-Query Expansion (MQE) and Hypothetical Document Embeddings (HyDE). MQE searches with several phrasings to widen the candidate set. HyDE generates a hypothetical answer-like passage to help retrieve documents. That generated passage is a search aid, not evidence or the final answer; the retrieved real documents still need screening.

## Using both in one turn

For a new question, the system can search memory and the knowledge base separately. Each returns candidates with their sources; context construction then decides what the model sees this turn. After the answer, the system decides whether there is a new fact worth remembering.

When an answer is wrong, I would locate the failure first. Was the material never ingested? Was it chunked badly? Did retrieval miss it? Was it found but left out of the context? Or did the model misuse the evidence? Adding more documents blindly would not address all of these cases.

# Chapter 4 | ReAct, Plan-and-Solve, and Reflection

Source: [Hello-Agents Chapter 4](https://github.com/datawhalechina/hello-agents/tree/main/docs/chapter4). I think of these three patterns as placing the loop at different points in a task.

[中文版](../04-agent-patterns.md)

## How the three patterns differ

A complex task often takes more than one model call. ReAct adjusts while acting; Plan-and-Solve plans the steps before executing them; Reflection checks and revises an initial result. A task can use more than one pattern.

## ReAct: decide again after each new result

```text
Receive question → Decide what information or action is needed → Call a tool
                 → Read tool result → Update the current judgement
                 → Act again or answer
```

The model proposes an action, the program parses and executes it, and the result becomes an observation in the next turn. If a search produces weak evidence, for example, the next turn can change the search terms. This fits tasks whose path is not known in advance.

I noted two boundaries in particular. A tool result is an observation, not automatically the final answer; the model still has to decide whether it addresses the original question. Also, proposing a tool call and actually making it are separate steps. The program identifies the tool, checks its arguments, and handles missing tools or execution failures.

ReAct costs multiple model calls. It can be slow or get stuck in a loop. A defined tool set, a record of each step, a maximum number of steps, and a stopping condition help keep it controlled.

## Plan-and-Solve: plan the whole task, then execute steps

```text
Original task → Create a step-by-step plan → Execute each step
                         ↑                      ↓
                         └── Check or revise the plan when needed
```

This separates the overall plan from the current step. During execution, the agent needs the original task, the plan, and earlier results so later steps do not lose the goal or repeat work. It suits tasks that can be broken into dependent steps.

The plan gives execution direction, but the first plan may be wrong or new information may change it. I would add checkpoints: if a step cannot be completed, a premise fails, or the results drift from the goal, the agent should revise the plan rather than mechanically finish it.

## Reflection: review an initial result with specific feedback

```text
Produce draft → Check it against the task → Give specific feedback → Revise
                                            ↘ Stop when requirements are met
                                              or the iteration limit is reached
```

Reflection asks whether the result is good enough. The check can look for missed constraints, weak evidence, gaps in reasoning, or format problems. A further generation pass is useful only when the feedback identifies something concrete to fix.

An instruction to “improve it again” without criteria can lead to endless rewording without better correctness. The review needs a target, an iteration limit, and verifiable evidence when it is available.

## Comparing and combining them

| Main difficulty | Pattern I would consider | What to watch |
| --- | --- | --- |
| New information keeps changing the next step | ReAct | Tool failures, loops, and cost |
| The task has dependent steps and an overall goal | Plan-and-Solve | A flawed plan or changed assumptions |
| An initial result needs checking and revision | Reflection | Whether the feedback is specific and improves the result |

The patterns can be combined: plan the stages, use ReAct to research within one stage, then review the answer. I would first locate the uncertainty. Is the next action unclear, or does an existing result need checking? Wherever the loop sits, it also needs a way to stop.

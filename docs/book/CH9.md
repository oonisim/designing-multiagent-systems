# Chapter 9: current code guide

Chapter 9 compares frameworks. It does not map to one implementation: use the
same task shape across the framework examples, then inspect PicoAgents as the
reference implementation in this repository.

## Actual comparison material

| Need | Where to compare it |
| --- | --- |
| Agent examples | [`examples/frameworks/`](../../examples/frameworks/) |
| Workflows | [`examples/frameworks/`](../../examples/frameworks/) and [`workflow/`](../../picoagents/src/picoagents/workflow/) |
| Orchestration | [`examples/frameworks/`](../../examples/frameworks/) and [`orchestration/`](../../picoagents/src/picoagents/orchestration/) |
| PicoAgents public API | [`picoagents/__init__.py`](../../picoagents/src/picoagents/__init__.py) |

## Section guide

### 9.1–9.2 Framework criteria and capabilities

Choose one small task: a tool-using agent, a conditional workflow, or a
two-agent coordinator. Compare lifecycle, state, streaming, tools, testing,
and termination using the matching directories under
[`examples/frameworks/`](../../examples/frameworks/).

### 9.3 Applying the comparison

Read the PicoAgents implementation by dependency: [`agents/`](../../picoagents/src/picoagents/agents/),
[`workflow/`](../../picoagents/src/picoagents/workflow/), and
[`orchestration/`](../../picoagents/src/picoagents/orchestration/). The book's
decision matrix is prose; there is no repository file that implements it.

## Practical comparison order

```text
one tool-using agent → one conditional workflow → one bounded orchestrator
```

Use identical inputs and an evaluation set from Chapter 10. A framework choice
based only on a demo cannot show operational behavior or failure handling.

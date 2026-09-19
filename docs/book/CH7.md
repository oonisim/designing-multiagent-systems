# Chapter 7: current code guide

Chapter 7 selects the next agent at runtime. Learn termination first: an
orchestrator without a clear stop condition is an uncontrolled loop.

## Actual framework source

| Topic | Actual code file |
| --- | --- |
| Orchestrator contract | [`_base.py`](../../picoagents/src/picoagents/orchestration/_base.py) |
| Round-robin selection | [`_round_robin.py`](../../picoagents/src/picoagents/orchestration/_round_robin.py) |
| Model-selected speaker | [`_ai.py`](../../picoagents/src/picoagents/orchestration/_ai.py) |
| Plan-driven coordination | [`_plan.py`](../../picoagents/src/picoagents/orchestration/_plan.py) |
| Handoffs and termination | [`_handoff.py`](../../picoagents/src/picoagents/orchestration/_handoff.py) and [`termination/`](../../picoagents/src/picoagents/termination/) |

## Section guide

### 7.1 The orchestration loop

Start with [`test_orchestrator.py`](../../picoagents/tests/test_orchestrator.py),
then read [`_base.py`](../../picoagents/src/picoagents/orchestration/_base.py).
It owns shared state and the run loop; concrete strategies decide who acts next.

### 7.2 Termination

Read [`test_termination.py`](../../picoagents/tests/test_termination.py) before
the strategies in [`termination/`](../../picoagents/src/picoagents/termination/).
Set a bounded termination rule before experimenting with delegation.

### 7.3 Round robin

Read [`_round_robin.py`](../../picoagents/src/picoagents/orchestration/_round_robin.py)
and run [`round-robin.py`](../../examples/orchestration/round-robin.py). This
is the simplest baseline because selection is deterministic.

### 7.4 AI-driven orchestration

Read [`_ai.py`](../../picoagents/src/picoagents/orchestration/_ai.py), then
[`ai-driven.py`](../../examples/orchestration/ai-driven.py). The model selects
the next participant, so evaluate selection and termination together.

### 7.5 Plan-based orchestration

Read [`_plan.py`](../../picoagents/src/picoagents/orchestration/_plan.py), then
[`plan-based.py`](../../examples/orchestration/plan-based.py). It makes the
intermediate plan explicit rather than relying only on speaker selection.

## End-to-end trace

```text
test_termination.py → termination/
test_orchestrator.py → orchestration/_base.py
    → _round_robin.py → _ai.py → _plan.py
```

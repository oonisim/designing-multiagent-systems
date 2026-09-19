# Chapter 6: current code guide

Chapter 6 is the deterministic counterpart to agent orchestration: define the
graph before execution. Read tests first because they state graph, checkpoint,
and progress behavior without live-model variability.

## Actual framework source

| Topic | Actual code file |
| --- | --- |
| Workflow definition | [`core/_workflow.py`](../../picoagents/src/picoagents/workflow/core/_workflow.py) |
| Runner and execution state | [`core/_runner.py`](../../picoagents/src/picoagents/workflow/core/_runner.py) |
| Models and serialization | [`core/_models.py`](../../picoagents/src/picoagents/workflow/core/_models.py) |
| Checkpoints | [`core/_checkpoint.py`](../../picoagents/src/picoagents/workflow/core/_checkpoint.py) |
| Step contract and function steps | [`steps/_step.py`](../../picoagents/src/picoagents/workflow/steps/_step.py) and [`steps/_function.py`](../../picoagents/src/picoagents/workflow/steps/_function.py) |

## Section guide

### 6.1–6.2 Graphs, steps, and typed data

Read [`test_workflow.py`](../../picoagents/tests/workflow/test_workflow.py),
then [`steps/_step.py`](../../picoagents/src/picoagents/workflow/steps/_step.py)
and [`steps/_function.py`](../../picoagents/src/picoagents/workflow/steps/_function.py).
They define a step's inputs, outputs, and execution boundary.

### 6.3 Edges, conditions, and parallel work

Read [`conditional.py`](../../examples/workflows/conditional.py), then trace
edge construction and validation in [`core/_workflow.py`](../../picoagents/src/picoagents/workflow/core/_workflow.py).
Use the workflow tests to understand ordering before adding parallel branches.

### 6.4–6.5 Workflow and runner

Read `Workflow` in [`core/_workflow.py`](../../picoagents/src/picoagents/workflow/core/_workflow.py),
then `WorkflowRunner` in [`core/_runner.py`](../../picoagents/src/picoagents/workflow/core/_runner.py).
Run [`sequential.py`](../../examples/workflows/sequential.py) only after this trace.

### 6.6–6.7 Persistence and checkpointing

Read [`core/_models.py`](../../picoagents/src/picoagents/workflow/core/_models.py),
[`core/_checkpoint.py`](../../picoagents/src/picoagents/workflow/core/_checkpoint.py),
and [`test_checkpoint.py`](../../picoagents/tests/workflow/test_checkpoint.py).
Then inspect [`checkpoint_example.py`](../../examples/workflows/checkpoint_example.py).

## End-to-end trace

```text
test_workflow.py → steps/_step.py → core/_workflow.py
    → core/_runner.py → core/_models.py → core/_checkpoint.py
```

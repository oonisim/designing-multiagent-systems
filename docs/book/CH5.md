# Chapter 5: current code guide

Chapter 5 describes computer-use agents. Read the test first, then the small
example, then trace the framework. Browser actions require a configured browser
and provider credentials.

## Actual framework source

| Topic | Actual code file |
| --- | --- |
| Computer-use agent loop | [`_computer_use.py`](../../picoagents/src/picoagents/agents/_computer_use/_computer_use.py) |
| Interface clients and observations | [`_interface_clients.py`](../../picoagents/src/picoagents/agents/_computer_use/_interface_clients.py) |
| Action planning | [`_planning_models.py`](../../picoagents/src/picoagents/agents/_computer_use/_planning_models.py) |
| Playwright action tools | [`_playwright_tools.py`](../../picoagents/src/picoagents/agents/_computer_use/_playwright_tools.py) |

## Section guide

### 5.1–5.2 Why computer use and the agent anatomy

Start with [`test_computer_use_agent.py`](../../picoagents/tests/test_computer_use_agent.py),
then read [`_computer_use.py`](../../picoagents/src/picoagents/agents/_computer_use/_computer_use.py).
It composes the agent, planner, interface client, and action tools.

### 5.2.1–5.2.2 Action generation and action space

Read [`_planning_models.py`](../../picoagents/src/picoagents/agents/_computer_use/_planning_models.py)
for the planning contract and [`_playwright_tools.py`](../../picoagents/src/picoagents/agents/_computer_use/_playwright_tools.py)
for the concrete browser actions available to the model.

### 5.3 Interface representation

Read [`_interface_clients.py`](../../picoagents/src/picoagents/agents/_computer_use/_interface_clients.py).
It defines how browser state becomes an observation the model can use. The
book's representations are concepts; this file is the current boundary.

### 5.4–5.5 Execution and implementation

Read [`computer_use.py`](../../examples/agents/computer_use.py) after the test.
Trace its construction into `_computer_use.py`, then follow the selected tool
into `_playwright_tools.py`.

### 5.6–5.7 Challenges and production concerns

The repository implements retryable agent and tool mechanics, but it is not a
complete browser-automation product. Add approval, cancellation, observability,
and task-specific limits from Chapter 4 before granting browser control.

## End-to-end trace

```text
test_computer_use_agent.py
    → agents/_computer_use/_computer_use.py
    → _planning_models.py
    → _interface_clients.py
    → _playwright_tools.py
```

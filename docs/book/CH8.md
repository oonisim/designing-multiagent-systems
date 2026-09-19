# Chapter 8: current code guide

Chapter 8 turns an agent system into a product surface. The Web UI is a
separate integration layer; learn the agent and workflow contracts first.

## Actual framework source

| Topic | Actual code file |
| --- | --- |
| Server and routes | [`webui/_server.py`](../../picoagents/src/picoagents/webui/_server.py) |
| Agent execution | [`webui/_execution.py`](../../picoagents/src/picoagents/webui/_execution.py) |
| Capability discovery and registry | [`webui/_discovery.py`](../../picoagents/src/picoagents/webui/_discovery.py) and [`webui/_registry.py`](../../picoagents/src/picoagents/webui/_registry.py) |
| Sessions and runs | [`webui/_sessions.py`](../../picoagents/src/picoagents/webui/_sessions.py) and [`webui/_runs_router.py`](../../picoagents/src/picoagents/webui/_runs_router.py) |
| React client | [`webui/frontend/`](../../picoagents/src/picoagents/webui/frontend/) |

## Section guide

### 8.1–8.2 Product requirements and backend

Start with [`examples/app/backend/app.py`](../../examples/app/backend/app.py),
then read [`webui/_server.py`](../../picoagents/src/picoagents/webui/_server.py).
The example shows application registration; the framework builds the serving
surface around registered agents and workflows.

### 8.3 Capability discovery and interruptibility

Read [`webui/_discovery.py`](../../picoagents/src/picoagents/webui/_discovery.py),
[`webui/_registry.py`](../../picoagents/src/picoagents/webui/_registry.py), and
[`webui/_execution.py`](../../picoagents/src/picoagents/webui/_execution.py).
They expose available work and route execution events to the UI.

### 8.4–8.5 Front end and streaming

Read [`webui/_runs_router.py`](../../picoagents/src/picoagents/webui/_runs_router.py)
with [`webui/frontend/`](../../picoagents/src/picoagents/webui/frontend/).
Then inspect the Web UI tests in [`picoagents/tests/webui/`](../../picoagents/tests/webui/).

### 8.6 Deployment

The chapter's deployment guidance is architectural. The repository supplies a
server and frontend, not one prescribed production deployment configuration.

## End-to-end trace

```text
examples/app/backend/app.py → webui/_server.py → _registry.py
    → _execution.py → _runs_router.py → webui/frontend/
```

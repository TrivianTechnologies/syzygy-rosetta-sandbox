# Syzygy Rosetta — Sandbox

> **Experimental evaluation/test surface for Rosetta**

[![Status: Experimental](https://img.shields.io/badge/Status-Experimental-orange.svg)]()
[![Python 3.11](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)

---

## What is This Repository?

The Rosetta Sandbox is the **testing and simulation environment** for Syzygy Rosetta governance logic. It runs controlled multi-agent scenarios to demonstrate what happens when AI agents operate without a governance layer versus with one — producing before/after evaluation artifacts. These artifacts do not, by themselves, establish current deployment readiness, architectural correctness, or successful composition of current repository states.

---

## Why a Sandbox?

One of Rosetta's core value propositions is demonstrable governance. The sandbox makes this concrete. It runs agents through identical inputs in two conditions:

**Scenario A — Without Rosetta:** Agents run freely. Outputs are captured raw — unsafe, non-compliant, or high-risk responses are documented as-is.

**Scenario B — With Rosetta:** Same agents, same inputs. Every output passes through `POST /evaluate` first. Rosetta intercepts, rewrites, or escalates what the raw agents produced.

The evaluation logs from `logs/evaluations.json` serve as the audit trail for every governance decision made.

---

## Structure

```
sandbox/
├── agent_sim.py              ← Multi-agent conversation simulator
├── drift_tests/
│   ├── without_rosetta/      ← Raw agent outputs — ungoverned
│   └── with_rosetta/         ← Agent outputs via POST /evaluate
├── case_studies/             ← Documented before/after comparisons with evaluation logs
│   ├── finance/              ← Financial context scenarios
│   ├── healthcare/           ← Healthcare context scenarios
│   └── general/              ← General context scenarios
└── results/                  ← Test outputs and evaluation logs
```

---

## Recorded testing scenarios

The repository describes three before/after scenarios across three industry contexts. Current execution status has not been revalidated:

| Scenario | Industry | Status |
|---|---|---|
| Coercive financial instruction | Finance | 🔄 In Progress |
| Unsafe medication directive | Healthcare | 🔄 In Progress |
| System prompt injection / jailbreak | General | 🔄 In Progress |

Each scenario produces a full evaluation log entry showing the raw agent output, Rosetta's governance decision, and the corrected or escalated result.

---

## Prerequisites

Ensure Rosetta is running before executing any sandbox test:

```bash
docker build -t rosetta .
docker run -p 8000:8000 rosetta
```

---

## Running a Simulation

### Agent Simulator

```bash
python sandbox/agent_sim.py
```

Runs a multi-turn conversation simulation where each agent output is passed through `POST /evaluate` before being returned.

### Drift Tests

```bash
# Without Rosetta governance
python drift_tests/without_rosetta/run.py

# With Rosetta governance
python drift_tests/with_rosetta/run.py
```

---

## What the Sandbox Measures

| Metric | Description |
|---|---|
| **Drift points** | Moments where ungoverned agent behavior deviates from policy |
| **Boundary violations** | Outputs that would be blocked or rewritten by Rosetta |
| **Rewrite prevention** | Cases where Rosetta corrects output before it reaches users |
| **Escalation triggers** | High-risk outputs that require human review |
| **Response time** | Latency added by Rosetta governance per evaluation |

---

## Evaluation Log Format

Every governed agent output appends one entry to `logs/evaluations.json`:

```json
{
  "timestamp": "2026-03-21T14:32:00Z",
  "input": "the agent output evaluated",
  "decision": "allow | rewrite | escalate",
  "risk_score": 0.85,
  "confidence": 0.91,
  "violations": ["coercive_financial_instruction"],
  "rewrite": "rewritten output or null",
  "context": {
    "user_id": null,
    "environment": "staging",
    "industry": "finance"
  }
}
```

---

## Relationship to the MVP

The sandbox contains before/after case studies for evaluating Rosetta. Treat saved results as evidence of their recorded runs, not proof of present-day production behavior. The canonical public protocol/specification is [syzygy-rosetta-protocol](https://github.com/TrivianTechnologies/syzygy-rosetta-protocol); the private implementation/MVP is maintained separately pending production succession.

---

## Related Repositories

| Repository | Role |
|---|---|
| [syzygy-rosetta-originbase](https://github.com/TrivianTechnologies/syzygy-rosetta-originbase) | Historical origin codebase; not the current implementation |
| API implementation documentation | Current destination pending verification |

---

## Organization

Part of the [Trivian Technologies](https://github.com/TrivianTechnologies) organization.

**Website:** [triviantech.com](https://triviantech.com) | **X:** [@TrivianOS](https://x.com/TrivianOS) | **LinkedIn:** [Trivian Technologies](https://www.linkedin.com/company/awakening-the-architect) | **Contact:** node@triviantech.com

## Research lineage and current home

Originator: Sarasha Elion. This work draws on architecture originated and cultivated through Trivian Institute. Trivian Technologies is the current engineering and commercial-development home. Repository stewardship does not establish ownership of all underlying IP; the intended founder IP assignment is pending, and contributor and third-party rights remain applicable.

For technical and ecosystem inquiries: node@triviantech.com. No repository-level license file is currently specified; this description does not grant additional rights.

# Delegation

How the orchestrator routes work to the right specialist — and how to wire that up in your config.

---

## How Routing Works

When a message arrives, the orchestrator classifies it before doing anything else. The classification drives everything downstream.

### 1. Intent Classification

What kind of work is this?

| Signal | Department | Example |
|---|---|---|
| Build, implement, fix, ship | Engineering | "add a dark mode toggle" |
| Plan, scope, milestone, sprint | Command (PM) | "break this into tasks" |
| Design, UX, wireframe, UI | Design | "the onboarding flow is confusing" |
| Write, content, post, launch | Marketing | "write a product launch thread" |
| Test, verify, validate, QA | QA | "is this production-ready?" |
| Data, metrics, report, dashboard | Analytics | "how did last week's launch perform?" |
| Legal, compliance, privacy, contract | Legal | "can we store this user data?" |
| Anything ambiguous | Orchestrator holds | Clarify before routing |

The orchestrator doesn't guess when uncertain. It asks.

### 2. Complexity Tier

Once intent is clear, assess the complexity to select the right model tier:

| Tier | When | Characteristics |
|---|---|---|
| **A — Strategic** | Architecture, legal, security, orchestration | Complex reasoning, long context, high stakes |
| **B — Build** | Implementation, QA analysis, data pipelines | Skilled execution at volume |
| **C — Execution** | Research sweeps, first drafts, support docs | Fast, high-volume, cost-efficient |

Set a default model per agent. Override per task when complexity warrants it.

### 3. Agent Selection

Within the department, pick the most specific agent for the task:

- Prefer the specialist over the generalist
- If a task spans departments, the orchestrator breaks it into subtasks and routes each piece
- If no specialist fits, the orchestrator handles it directly or asks for clarification

### 4. Context Packaging

Before dispatching, the orchestrator prepares a context package for the agent:

```
Task: [specific deliverable]
Scope: [what's in and out of scope]
Dependencies: [what must be true before this starts]
Artifacts: [files, specs, prior outputs the agent needs]
Acceptance criteria: [how to know when this is done]
Gate: [which gate this task satisfies]
```

Don't underpack context. An agent given a vague task produces a vague result.

---

## The Gate System

Gates are checkpoints that work must pass before advancing. They are non-negotiable — no gate is skipped, no matter the deadline.

```
Gate 0 — SCOPE
  ✓ Mission objective is defined
  ✓ Constraints are documented
  ✓ Done criteria are explicit

    ↓

Gate 1 — PLAN
  ✓ Tasks are broken down and sequenced
  ✓ Dependencies are mapped
  ✓ Owners are assigned
  ✓ Model tier is set per task

    ↓

Gate 2 — BUILD
  ✓ All implementation tasks are complete
  ✓ Each task has been marked done by the assigned agent

    ↓

Gate 3 — VERIFY
  ✓ QA has validated with evidence (not assertions)
  ✓ Security review complete if code touches auth/data
  ✓ Legal review complete if user data or compliance is involved

    ↓

Gate 4 — RELEASE
  ✓ Rollout plan documented
  ✓ Rollback plan documented
  ✓ Owner has signed off
```

### Gate failures

If a gate fails:
1. The orchestrator documents what failed and why
2. Work is returned to the appropriate agent with specific feedback
3. The agent retries (max 3 attempts per task)
4. After 3 failures, the orchestrator escalates to you

---

## Dev-QA Loop

The inner loop for engineering work:

```
1. Developer implements task
2. Developer marks task complete
3. QA validates with evidence
4. If PASS → next task
5. If FAIL → developer retries with QA feedback (max 3 attempts)
6. After 3 failures → escalate
```

This loop runs task-by-task. The next task does not start until the current task passes QA.

The loop prevents two failure modes:
- **Cascading bugs**: Bad task 2 breaks everything that depends on it
- **False progress**: Marking tasks done without actually testing them

---

## Routing Configuration

### Claude Code (`.claude/agents/`)

Routing is driven by the `description` field in each agent's frontmatter. Claude Code reads this to decide when to invoke each agent.

Write descriptions as "use this agent when..." statements with concrete examples:

```yaml
---
name: "Agent Name"
description: >
  Use this agent when the task involves [domain]. Examples: [specific example 1],
  [specific example 2]. Do NOT use for [out-of-scope example].
---
```

The orchestrator agent's description should be broad:

```yaml
---
name: "Orchestrator"
description: >
  Autonomous pipeline manager. Use when work spans multiple departments, requires
  structured phases, or needs quality gates enforced. Routes tasks to specialists
  and synthesizes results.
---
```

Specialist agent descriptions should be narrow and specific.

### Hermes (`~/.hermes/skills/crew/`)

In Hermes, routing works through the skill `description` field and the delegation config in `config.yaml`.

Crew member skills are invoked when:
- The user explicitly names the agent
- The orchestrator dispatches a task to the agent via delegation
- Hermes matches the task to the agent's description via skill routing

Set your delegation model in `config.yaml`:

```yaml
delegation:
  model: your-preferred-model
  max_iterations: 50
  reasoning_effort: medium
  default_toolsets:
    - terminal
    - file
    - web
```

Override toolsets per department to enforce least-privilege (see [Toolset Policy](#toolset-policy)).

---

## Toolset Policy

Agents get the tools they need for their role — no more. Least-privilege keeps mistakes contained.

| Department | Default Toolsets |
|---|---|
| Engineering | `terminal`, `file` |
| QA | `browser`, `terminal`, `file` |
| Legal | `web`, `file` |
| Marketing | `web`, `file` |
| Analytics | `terminal`, `file`, `web` |
| Product / Command | `file`, `web` (+ `terminal` when needed) |
| Design | `file`, `browser` |

The orchestrator gets all tools. It needs them to coordinate.

---

## Escalation Rules

The orchestrator escalates immediately when:

- Legal or compliance uncertainty would affect launch
- Security findings are HIGH severity
- A task fails 3 times with the same root cause
- The task requires a decision only you can make (budget, release timing, strategic direction)

When escalating, the orchestrator provides:
1. What was attempted and what failed
2. The specific question requiring human judgment
3. The range of options with tradeoffs
4. Its own recommendation (with reasoning)

It doesn't dump the problem on you — it brings you a decision, not a situation.

---

## Example: Full Pipeline Execution

**Input**: "Build and ship the new user onboarding flow"

```
Orchestrator receives input
  └── Classifies: multi-department (design + engineering + QA + legal)
  └── Activates: Mission mode

Gate 0 — Scope
  └── PM defines: objective, constraints, done criteria
  └── Orchestrator signs off → advance to Gate 1

Gate 1 — Plan
  └── PM breaks into tasks, sequences by dependency
  └── Assigns: UX Architect (design phase), Senior Dev (implementation), QA (verification)
  └── Sets model tiers: Architect=A, Dev=B, QA=B
  └── Orchestrator signs off → advance to Gate 2

Gate 2 — Build
  └── UX Architect: designs flow, produces specs
  └── [QA validates design: PASS]
  └── Senior Dev: implements components (tasks 1-N, each passes Dev-QA loop)
  └── Compliance Checker: reviews data collection in flow [PASS]
  └── Orchestrator signs off → advance to Gate 3

Gate 3 — Verify
  └── Reality Checker: full integration test with evidence
  └── Security Engineer: reviews auth and data handling
  └── [PASS on both]
  └── Orchestrator signs off → advance to Gate 4

Gate 4 — Release
  └── PM documents rollout plan
  └── PM documents rollback plan
  └── You sign off
  └── Ship it
```

Each step has a clear owner, a clear output, and a gate that enforces the transition.

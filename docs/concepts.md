# Core Concepts

This document explains the ideas behind Starfleet. If you just want to get started, skip to [Getting Started](../README.md#quick-start). Come back here when something feels arbitrary or you want to know why.

---

## The Problem With One-Agent Setups

A single general-purpose AI assistant is fast to set up and works fine for simple tasks. It breaks down under load:

- **Context bleed**: "Write a legal compliance summary" and "ship this feature" require different priorities, different voices, different risk tolerances. One agent oscillates between them poorly.
- **No accountability**: When everything is one agent, there's no clear owner for quality in any domain.
- **Prompt bloat**: Trying to make one agent good at everything produces a massive, contradictory system prompt that makes it mediocre at all of it.
- **No specialization**: Legal analysis, backend architecture, and marketing copy require genuinely different mental models. One agent can do all three — just not well at the same time.

Multi-agent systems fix this by giving each concern its own agent. The challenge is coordination. That's what Starfleet is for.

---

## The Crew Model

A Starfleet crew is a set of specialist agents organized into departments, coordinated by an orchestrator, and operating under a defined command hierarchy.

```
You (Starfleet Command)
    │
    └── Orchestrator (Fleet Captain)
            │
            ├── Engineering Lead → Senior Dev, Backend, DevOps, Mobile...
            ├── Design Lead     → UX Architect, UI Designer, Researcher...
            ├── Marketing Lead  → Content Creator, Growth Hacker...
            ├── Product Lead    → Project Manager, Sprint Planner...
            ├── QA Lead         → Reality Checker, Tool Evaluator...
            ├── Support Lead    → Analytics Reporter, Finance Tracker...
            └── Legal Lead      → Compliance Checker...
```

In practice, for small crews, the lead and the specialist are the same agent. You add specialization as workload grows.

---

## Personas

Every crew member has a defined persona. This is more than flavor — it's functional:

**Consistency**: A persona is a contract. "This agent writes with precision and pushes back on hand-waving" is a behavioral spec. The model follows it more reliably than a generic instruction.

**Predictability**: When you know who you're talking to, you know what kind of output to expect. The analytics reporter gives you numbers and context. The reality checker defaults to "needs work" unless proven otherwise.

**Routing clarity**: The orchestrator needs to know who to call. Distinct personas with distinct descriptions make routing unambiguous. "Route frontend UI bugs to the UX Architect" only works if the UX Architect has a clear identity.

**Personas are optional in principle but load-bearing in practice.** Strip them out and you get an unnamed specialist agent. That still works. The persona layer makes the specialist reliably act like a specialist.

### What a Persona Includes

Each agent definition includes:

- **Name**: The handle the orchestrator uses to invoke them
- **Role/Title**: Their domain and seniority
- **Voice**: How they communicate — terse, detailed, challenging, warm, etc.
- **Priorities**: What they optimize for, what they push back on
- **Specialty**: The specific skills and tools they bring
- **Model assignment**: Which model tier serves this role (see [Model Tiering](delegation.md#model-tiering))

---

## Departments

Seven departments cover the full software-business stack. See [`departments.md`](departments.md) for the complete breakdown.

The structure follows how real software organizations divide labor:

| Department | Core Function |
|---|---|
| Command | Orchestration, project management, coordination |
| Engineering | Implementation across all stacks |
| Design | UX, UI, research |
| Marketing | Content, growth, community |
| QA & Testing | Quality gates, tool evaluation, workflow audit |
| Analytics & Finance | Data, dashboards, financial health |
| Legal & Compliance | Regulatory, licensing, risk |

You don't have to staff all departments. Define what you need.

---

## The Orchestrator

The orchestrator is the only agent that sees the full picture. It:

1. Receives your intent
2. Interprets what kind of work it is (engineering? legal? marketing?)
3. Identifies which agents need to be involved and in what order
4. Dispatches work to specialists with full context
5. Enforces quality gates before work advances
6. Synthesizes outputs and reports back

The orchestrator never does the domain work itself. It coordinates the agents that do.

This separation is critical. An orchestrator that also does the coding is an orchestrator that rationalizes bad code. Keep the roles separate.

---

## Gates

Every significant piece of work passes through five gates before it ships:

| Gate | Name | What It Enforces |
|---|---|---|
| 0 | Scope | Mission objective, constraints, and done criteria are defined |
| 1 | Plan | Tasks, dependencies, owners, and model tier are assigned |
| 2 | Build | Implementation outputs are complete |
| 3 | Verify | QA, security, and legal checks have passed with evidence |
| 4 | Release | Rollout plan, rollback plan, and owner signoff exist |

**No gate is optional.** Skipping Gate 3 because the deadline is tomorrow is how you get 3 AM incidents.

The gate system is lightweight for small tasks — a one-line check. For complex initiatives, it's a real checkpoint with artifacts.

---

## Delegation

When a task comes in, the orchestrator routes it. The routing logic is:

1. **Intent classification**: What kind of work is this? Implementation, analysis, content, compliance?
2. **Complexity tier**: Can a fast model handle this, or does it need full reasoning?
3. **Department assignment**: Which division owns this type of work?
4. **Specialist selection**: Within that division, which agent has the right skill set?
5. **Context packaging**: What does the specialist need to know to do this well?

See [`delegation.md`](delegation.md) for the full model.

---

## Model Tiering

Not every task needs your most capable (and expensive) model. Starfleet uses three tiers:

- **Tier A (Strategic)**: Orchestration, architecture decisions, legal analysis, security review — complex reasoning where quality matters more than speed or cost
- **Tier B (Build)**: Engineering implementation, QA analysis, data pipelines — capable models doing skilled work at volume
- **Tier C (Execution)**: Research sweeps, first-draft content, support docs — high-volume, fast, cost-efficient

Assign tiers at the agent level in your config. Override per task when complexity warrants it.

---

## A Note on Star Trek

The characters aren't arbitrary. Each one was chosen because their canonical traits map to the role:

- **The Admiral (Nechayev)** commands without doing the implementation work herself
- **The Engineer (La Forge)** solves impossible problems with what's available
- **The Counselor (Troi)** reads intent and understands what people actually need
- **The Security Chief (Worf)** prioritizes integrity over convenience, every time
- **The Doctor (Crusher)** runs diagnostics and tells you what's actually wrong
- **The First Officer (Riker)** executes the captain's vision and pushes back when needed

The theme helps you remember who does what. If Trek isn't your thing, swap the references for any cast of characters with distinct personalities. The pattern is what matters.

# Starfleet

> *"Things are only impossible until they are not."*

A framework for building a structured, persona-driven AI agent crew — organized into departments, coordinated by an orchestrator, and deployed across tools like Claude Code, Hermes, or any multi-agent runtime.

## What Is Starfleet?

Most AI setups are a single assistant doing everything. Starfleet is different: a command hierarchy of specialist agents, each with a defined role, a clear domain, and a consistent voice. You give an agent a persona, a department, and a purpose. Then you wire them together under an orchestrator that routes work to the right specialist based on the nature of the task.

Think of it as building an AI agency with org structure instead of a chatbot that guesses what hat to wear.

The Star Trek framing is optional. The underlying pattern — **orchestrator → department leads → specialists** — is not. That's the architecture that makes this work.

## What You Get

- A **department structure** with 7 divisions covering the full software-business stack
- An **orchestrator pattern** that routes tasks by intent, complexity, and domain
- A **gate system** that enforces quality checkpoints before work advances
- **Agent templates** for Claude Code (`.claude/agents/`) and Hermes (`~/.hermes/skills/crew/`)
- **Example crew definitions** you can clone and adapt
- **Model tiering guidance** — strategic vs. build vs. high-volume tasks

## What You Don't Get

This repo is a foundation, not a complete solution. It gives you the architecture and the scaffolding. Tuning your agents' voices, calibrating their model assignments, and wiring in your specific tools and workflows — that's the work you do on top of this.

## Quick Start

### 1. Define your command persona

Create a primary persona for your orchestrator — this is the "captain" of your crew. See [`hermes/SOUL.md.example`](hermes/SOUL.md.example) for the format.

### 2. Define your crew

Use the templates in [`agents/_template.md`](agents/_template.md) (Claude Code) or [`hermes/skills/crew/_template/SKILL.md`](hermes/skills/crew/_template/SKILL.md) (Hermes) to define each crew member. Start with:

- One orchestrator
- One agent per department you actually need
- Add specialists within departments as your workload grows

### 3. Wire up the orchestrator

The orchestrator's `description` field drives routing. Write it so your AI runtime understands when to invoke it vs. a specialist. See [`agents/orchestrator.md`](agents/orchestrator.md) for a working example.

### 4. Start small

You don't need all 7 departments on day one. A functional starter crew is:

- Orchestrator
- Senior Developer
- Project Manager
- Reality Checker (QA)

Scale from there.

## Repo Structure

```
starfleet/
├── docs/
│   ├── concepts.md         # Core framework concepts
│   ├── departments.md      # Department breakdown with roles
│   └── delegation.md       # How the orchestrator routes work
├── agents/                 # Claude Code agent definitions (.claude/agents/)
│   ├── _template.md        # Generic agent template
│   ├── orchestrator.md     # Command — Orchestrator
│   ├── engineering/        # Engineering department
│   ├── design/             # Design department
│   ├── marketing/          # Marketing department
│   ├── product/            # Product & Project Management
│   ├── qa/                 # QA & Testing
│   ├── support/            # Analytics & Finance
│   └── legal/              # Legal & Compliance
└── hermes/                 # Hermes-specific configs
    ├── SOUL.md.example     # Primary persona template
    ├── config.yaml.example # Hermes config template
    └── skills/crew/        # Hermes crew skill definitions
```

## Philosophy

**Personas create consistency.** A named agent with a defined voice behaves more predictably than a generic assistant. The persona is a contract: this agent does this kind of work, in this style, with these priorities.

**Departments create accountability.** When every task has a clear owner-by-domain, you stop getting answers that blend engineering judgment with marketing instinct with legal caution into mush. Each division has its lane.

**Orchestration creates leverage.** The orchestrator doesn't do the work — it routes, coordinates, and enforces quality gates. That separation is what lets you scale the crew without scaling the chaos.

**Gates prevent regret.** No phase advances without meeting exit criteria. Scope before plan. Plan before build. Build before verify. Verify before release. Skipping gates is how you ship broken things confidently.

## Adapting the Theme

Starfleet uses Star Trek characters as a mnemonic layer — each persona maps to a character whose canonical traits match the role. You don't have to use Trek. Swap in whatever reference frame makes your crew memorable:

- A heist crew (the planner, the hacker, the grifter...)
- A kitchen brigade (exec chef, sous chef, pastry...)
- A film crew (director, DP, editor, sound...)
- No theme at all — just names and titles

The pattern works regardless of the flavor.

## Compatibility

| Runtime | Config Location | Format |
|---|---|---|
| Claude Code | `.claude/agents/` | Markdown with YAML frontmatter |
| Hermes | `~/.hermes/skills/crew/` | `SKILL.md` per agent |
| Custom | Your runtime's agent config | Adapt the templates |

## Contributing

This is a starter kit, not a finished product. PRs welcome for:
- Additional department templates
- New example crew configurations
- Adapter guides for other runtimes
- Improved routing patterns

## License

MIT — see [LICENSE](LICENSE).

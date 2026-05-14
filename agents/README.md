# Claude Code Agent Definitions

These agent definitions are designed for the Claude Code agent system. Place them in your `.claude/agents/` directory.

## How Claude Code Agents Work

Claude Code reads agent definitions from `.claude/agents/*.md`. Each file defines a specialist agent with:

- **Frontmatter** (YAML between `---` delimiters) — metadata Claude uses to identify and route to the agent
- **System prompt** (everything after the frontmatter) — instructions the agent follows when invoked

### Frontmatter Fields

```yaml
---
name: "Display name shown in the UI"
description: "When Claude auto-selects this agent — write as 'use this when...'"
model: inherit          # or: sonnet, opus, haiku (specific model override)
color: blue             # UI color: red, blue, yellow, green, purple, magenta
---
```

The `description` field is the routing mechanism. Claude Code reads it to decide which agent to invoke for a given task. Write it with concrete trigger conditions and examples.

## Setting Up Your Crew

### Step 1: Copy the agents you need

```bash
# Copy to your project's agent directory
cp agents/orchestrator.md your-project/.claude/agents/orchestrator.md
cp agents/engineering/*.md your-project/.claude/agents/
# etc.
```

Or to your global Claude Code config (applies across all projects):

```bash
cp agents/orchestrator.md ~/.claude/agents/orchestrator.md
```

### Step 2: Customize the agents

Open each agent file and adapt:
- The persona name and identity
- The specialty description
- The operating principles for your team's standards
- The output format for your workflows
- The model assignment (if you want specific models per agent)

### Step 3: Add the orchestrator last

The orchestrator references the other agents by name. Make sure the specialist agents exist before the orchestrator tries to route to them.

### Step 4: Test routing

Start a session and give it tasks from different domains. Watch which agent gets invoked. Tune the `description` fields if routing is off.

## Included Agents

| File | Role | Department |
|---|---|---|
| `orchestrator.md` | Autonomous pipeline manager | Command |
| `product/project-manager.md` | Spec-to-tasks, delivery tracking | Command |
| `engineering/senior-developer.md` | Production frontend/full-stack | Engineering |
| `engineering/backend-architect.md` | Systems, APIs, databases | Engineering |
| `engineering/devops-automator.md` | CI/CD, infrastructure | Engineering |
| `engineering/rapid-prototyper.md` | Fast spikes and MVPs | Engineering |
| `design/ux-architect.md` | UX architecture, CSS systems | Design |
| `marketing/content-creator.md` | Content strategy and copy | Marketing |
| `qa/reality-checker.md` | Production readiness, evidence-based QA | QA |
| `support/analytics-reporter.md` | Data analysis, dashboards | Analytics |
| `legal/compliance-checker.md` | Regulatory, privacy, licensing | Legal |

## Using the Template

`_template.md` is a blank agent template. Use it to create agents for:
- Roles not covered by the examples
- Specialized variants of included roles
- Domain-specific agents for your product area

## Model Assignment Guidance

```yaml
model: inherit    # Use whatever model is active in the session
model: opus       # Claude Opus — for strategic, complex reasoning
model: sonnet     # Claude Sonnet — for skilled execution at volume
model: haiku      # Claude Haiku — for fast, high-volume, lower-complexity work
```

Recommended defaults:
- **Orchestrator** → `opus` (strategic coordination)
- **Architects, Senior Dev, Legal, Security** → `opus` or `sonnet`
- **Prototyper, Content, Analytics** → `sonnet`
- **Execution tasks, drafts, support** → `haiku`

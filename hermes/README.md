# Hermes Integration

This directory contains Starfleet configuration for [Hermes](https://github.com/NousResearch/hermes-agent).

## What is Hermes?

Hermes is an open-source AI agent framework that runs locally. It handles model routing, memory, skills, tool use, and multi-agent delegation. Starfleet runs on top of Hermes as a structured crew of specialist agents.

## Setup

### 1. Install Hermes

Follow the [Hermes installation guide](https://github.com/NousResearch/hermes-agent).

### 2. Configure your primary persona

Copy and customize the soul file:

```bash
cp hermes/SOUL.md.example ~/.hermes/SOUL.md
```

Edit it to define your crew's captain — the primary persona Hermes uses by default.

### 3. Configure Hermes

```bash
cp hermes/config.yaml.example ~/.hermes/config.yaml
```

Key settings to customize:
- `model.default` — your primary model
- `delegation.model` — model used for delegated crew agents
- `approvals.mode` — `manual` for oversight, `auto` for autonomous operation

### 4. Install crew skills

Each crew member is a subdirectory in `~/.hermes/skills/crew/`. Copy the ones you want:

```bash
# Copy the orchestrator
cp -r hermes/skills/crew/starfleet-orchestrator ~/.hermes/skills/crew/

# Copy the template for building your own
cp -r hermes/skills/crew/_template ~/.hermes/skills/crew/my-agent-name
```

### 5. Build your crew

Use `_template/SKILL.md` to define each crew member. The key fields:

```markdown
---
name: crew-member-slug
description: "When to invoke this agent..."
---

# Full Name — Title / Character

## Specialty
[Comma-separated list of skills]

## Approach
1. [Step 1]
...

## Model
[model identifier]
```

The `description` field drives routing — write it with concrete trigger conditions.

## Directory Structure

```
hermes/
├── SOUL.md.example          # Primary persona template
├── config.yaml.example      # Hermes config template
├── README.md                # This file
└── skills/
    └── crew/
        ├── _template/
        │   └── SKILL.md     # Blank crew member template
        └── starfleet-orchestrator/
            └── SKILL.md     # Example orchestrator skill
```

## Crew Skill Format

Each crew member lives at: `~/.hermes/skills/crew/<slug>/SKILL.md`

Required frontmatter:

```yaml
---
name: slug-name
description: "Routing trigger — when to invoke this crew member"
metadata:
  source: starfleet-crew
  category: starfleet-crew
---
```

The body of the SKILL.md is the system prompt for that crew member when delegated.

## Invoking Crew Members

In Hermes, crew members can be invoked:

1. **By name**: "Ask the Backend Architect to design the API"
2. **By description match**: Hermes routes based on the skill `description`
3. **Via orchestrator**: The starfleet-orchestrator dispatches to specialists automatically

## Model Assignment

Set the model at the bottom of each SKILL.md:

```markdown
## Model
anthropic/claude-opus-4-6

## Model
anthropic/claude-sonnet-4-5

## Model  
anthropic/claude-haiku-4-5
```

Recommended tiers:
- **Opus**: Orchestrator, architects, legal, security
- **Sonnet**: Senior developers, QA, analytics, content
- **Haiku**: High-volume execution, drafts, research sweeps

## Optional: Hermes Dashboard

[Hermes Dashboard](https://github.com/Kori-x/hermes-dashboard) provides a real-time monitoring UI for your crew's sessions, tool calls, and activity. Recommended if you're running a full crew.

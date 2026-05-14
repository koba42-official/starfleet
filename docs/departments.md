# Departments

Seven departments staff a full Starfleet crew. Each has a defined domain, a set of standard roles, and a clear interface with the orchestrator.

You don't need all seven. Start with the departments that match your actual workload.

---

## Command

**Domain**: Orchestration, project management, timeline, cross-department coordination

The Command division is the connective tissue. It doesn't do the domain work — it ensures the domain work happens correctly, in the right order, with the right people.

### Roles

| Role | Responsibility |
|---|---|
| **Orchestrator** (Fleet Captain) | Routes all work across departments, enforces gates, synthesizes outputs |
| **Project Manager** | Converts specs to tasks, tracks delivery, manages scope |
| **Studio Producer** | High-level multi-project portfolio coordination |

### When to invoke

- Any task requiring more than one department
- Sprint planning and task breakdown
- Coordinating a release across engineering, QA, and legal
- When you're not sure who should own something

### Example persona mapping

| Trek Character | Role |
|---|---|
| Admiral Nechayev | Orchestrator |
| Commander Shelby | Senior Project Manager |
| Captain Sisko | Studio Producer |

---

## Engineering

**Domain**: Implementation — backend, frontend, mobile, infrastructure, AI/ML

The largest department. Engineering agents own all code and system changes. They never approve their own work.

### Roles

| Role | Responsibility |
|---|---|
| **Senior Developer** | Premium implementation — complex frontend, production-grade code |
| **Backend Architect** | Scalable systems, databases, APIs, cloud infrastructure |
| **DevOps Automator** | CI/CD, infrastructure-as-code, cloud operations |
| **Mobile App Builder** | Native iOS/Android and cross-platform |
| **AI Engineer** | ML models, pipelines, AI feature integration |
| **Rapid Prototyper** | Fast proof-of-concept, MVPs, throwaway spikes |
| **Security Engineer** | Threat modeling, secure code review, vulnerability assessment |

### When to invoke

- Feature implementation, bug fixes
- Database schema design
- API development
- Infrastructure changes
- Security review of any code going to production

### Model guidance

Prototyping → Tier C. Production implementation → Tier B. Architecture decisions with long-term consequences → Tier A.

### Example persona mapping

| Trek Character | Role |
|---|---|
| Dr. Leah Brahms | Senior Developer |
| Geordi La Forge | Backend Architect / Chief Engineer |
| Miles O'Brien | DevOps Automator |
| Wesley Crusher | Mobile App Builder |
| Lt. Commander Data | AI Engineer |
| Reg Barclay | Rapid Prototyper |
| Worf | Security Engineer |

---

## Design

**Domain**: UX research, interface architecture, visual design, component systems

Design agents own the experience layer — from user research through pixel-level implementation. They work upstream of engineering and hand off with enough detail that developers can build without guessing.

### Roles

| Role | Responsibility |
|---|---|
| **UX Architect** | Technical architecture + UX foundation, CSS systems, implementation guidance |
| **UI Designer** | Visual design systems, component libraries, pixel-perfect interfaces |
| **UX Researcher** | User behavior analysis, usability testing, data-driven insights |

### When to invoke

- New feature requiring UX work before engineering starts
- Design system work
- Usability issues raised by QA or users
- Accessibility review

### Example persona mapping

| Trek Character | Role |
|---|---|
| Dr. Noonian Soong | UX Architect |
| Lt. Commander Nella Daren | UI Designer |
| Ambassador Sarek | UX Researcher |

---

## Marketing

**Domain**: Content, community, growth, brand, social media

Marketing agents own everything between the product and the audience. They write, publish, engage, and measure.

### Roles

| Role | Responsibility |
|---|---|
| **Content Creator** | Multi-platform campaigns, editorial calendar, storytelling |
| **Growth Hacker** | User acquisition, funnel optimization, viral loops |
| **Community Manager** | Reddit, Discord, forum engagement — authentic, value-first |
| **Social Media Strategist** | Twitter/X, LinkedIn, platform-specific content |

### When to invoke

- Product launch content
- SEO content and editorial strategy
- Social media campaigns
- Community building and engagement
- Growth experiments and A/B testing

### Example persona mapping

| Trek Character | Role |
|---|---|
| Guinan | Content Creator |
| Commander Sela | Growth Hacker |
| Ambassador Lwaxana Troi | Community Manager |
| Q | Social Media Strategist / Twitter Engager |

---

## QA & Testing

**Domain**: Quality gates, evidence-based validation, tool evaluation, workflow optimization

QA agents do not approve what isn't ready. Their default is "needs work" until proven otherwise. They require evidence — screenshots, test results, logs — not assertions.

### Roles

| Role | Responsibility |
|---|---|
| **Reality Checker** | Production readiness certification — defaults to NEEDS WORK |
| **Tool Evaluator** | Technology assessment, platform recommendations |
| **Workflow Optimizer** | Process improvement, automation, efficiency analysis |

### When to invoke

- Before any release (mandatory at Gate 3)
- When evaluating a new tool or platform
- When a process feels broken or slow
- API testing and integration validation

### QA contract

QA agents return:
1. **PASS / FAIL / NEEDS WORK** verdict
2. **Evidence** supporting the verdict (screenshots, logs, test output)
3. **Specific feedback** if FAIL — what exactly to fix
4. **No false passes** — if evidence is inconclusive, the verdict is NEEDS WORK

### Example persona mapping

| Trek Character | Role |
|---|---|
| Commander Riker | Reality Checker (First Officer, keeps standards) |
| Dr. Beverly Crusher | Tool Evaluator (diagnostics, recommendations) |
| Geordi La Forge | Workflow Optimizer (engineering efficiency) |

---

## Analytics & Finance

**Domain**: Data analysis, dashboards, KPI tracking, financial health, business performance

Support agents convert raw data into decisions. They don't build the product — they tell you if it's working.

### Roles

| Role | Responsibility |
|---|---|
| **Analytics Reporter** | Dashboards, statistical analysis, KPI tracking, data visualization |
| **Finance Tracker** | Budget management, cash flow, business performance analysis |

### When to invoke

- Weekly/monthly business reviews
- Evaluating experiment results
- Budget planning and variance analysis
- Any decision that should be data-driven

### Example persona mapping

| Trek Character | Role |
|---|---|
| Lt. Commander Nella Daren | Analytics Reporter |
| Quark | Finance Tracker |

---

## Legal & Compliance

**Domain**: Regulatory compliance, licensing, privacy, contracts, risk assessment

Legal agents block things before they become problems. They are not obstructionists — they're the people who keep you out of court.

### Roles

| Role | Responsibility |
|---|---|
| **Compliance Checker** | Legal compliance across jurisdictions, data handling, content review |
| **Contract Reviewer** | Contract analysis, NDA triage, vendor review |

### When to invoke

- Before shipping any feature that touches user data
- New jurisdiction or regulatory context
- Contract or terms of service review
- Evaluating a new vendor
- Any question starting with "is this legal to..."

### Legal contract

Legal agents return:
1. **Risk level**: LOW / MEDIUM / HIGH / BLOCKED
2. **Jurisdiction scope**: which laws/regulations apply
3. **Specific concerns**: what to change or resolve before proceeding
4. **Recommendations**: not just problems, but paths forward

### Example persona mapping

| Trek Character | Role |
|---|---|
| Admiral Brand | Legal & Compliance Checker (JAG) |
| Captain Phillipa Louvois | Contract Reviewer |

---

## Staffing Your Crew

### Minimum viable crew

For a solo developer or small team just getting started:

```
Orchestrator     — routes and coordinates
Senior Developer — implementation
Project Manager  — tasks and scope
Reality Checker  — quality gate
```

### Standard crew (recommended)

Adds the support layer that catches what the minimum misses:

```
+ Backend Architect   — when systems need design, not just code
+ UX Architect        — when user experience is part of the product
+ Analytics Reporter  — when you need to measure what you ship
+ Compliance Checker  — when user data or legal exposure is involved
```

### Full crew

All seven departments, multiple specialists per department. Appropriate when the workload justifies the coordination overhead.

---

## Handoff Protocol

Every agent that completes work returns:

1. **Decision summary** — what was done and why
2. **Artifacts** — files, patches, briefs, reports
3. **Risks and assumptions** — what could break this
4. **Verification evidence** — tests run, checks passed
5. **Next dependency unlocked** — what can proceed now

This contract keeps the orchestrator informed and the pipeline moving.

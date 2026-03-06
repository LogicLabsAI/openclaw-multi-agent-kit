# CLAUDE.md — Project Rules & Context

> **RULE: Update this file whenever code or project structure changes.** Every added, removed, or renamed file, template, config key, or architectural decision must be reflected here before the task is considered done.

---

## Project Overview

**OpenClaw Multi-Agent Kit** — Production-tested templates for building AI agent teams on [OpenClaw](https://openclaw.sh) with Telegram supergroup integration. This is a template/docs-only repo (no runtime code). It provides SOUL.md personality templates, IDENTITY.md metadata templates, workspace scaffolding, and openclaw.json config snippets for deploying up to 10 autonomous agents coordinated through Telegram topic channels.

**Not a library or app.** Nothing here executes — it's all markdown templates, JSON config examples, and documentation meant to be copied into an OpenClaw workspace.

## Tech Stack / Platform

- **Platform:** OpenClaw (agent orchestration platform)
- **Channel:** Telegram (supergroups with topics)
- **LLM providers:** Anthropic Claude models (sonnet-4-6, opus-4-6, haiku-4-5)
- **Config format:** JSON / JSONC
- **Docs/templates:** Markdown

## Repository Structure

```
.
├── .gitignore                         # OS artifacts and editor files
├── CLAUDE.md                          # This file — project rules & context
├── INSTRUCTIONS.md                    # AI-readable setup guide (8 phases)
├── README.md                          # Project overview & quick start
├── LICENSE                            # MIT
├── docs/
│   ├── agent-design-patterns.md       # How to write effective SOUL.md files
│   ├── scaling.md                     # Scaling guidance: when to add agents, cost, circular triggers
│   └── supergroup-setup.md            # Step-by-step Telegram supergroup setup
├── examples/
│   ├── full-team.json                 # Complete 10-agent openclaw.json config
│   └── minimal-team.json              # Minimal 3-agent config (orchestrator + coder + QA)
└── templates/
    ├── openclaw-config.jsonc          # Base config snippet with defaults
    ├── identity/
    │   └── agent-identity.md          # Standard identity template for any agent
    ├── soul/                          # Agent personality templates (SOUL.md)
    │   ├── orchestrator.md            # Lead agent — coordinates all others
    │   ├── coding-agent.md            # Software engineering specialist
    │   ├── qa-agent.md                # Testing and quality assurance
    │   ├── devops-agent.md            # Infrastructure and deployment
    │   ├── research-agent.md          # Market research and intelligence
    │   ├── growth-agent.md            # Analytics and growth experiments
    │   ├── content-agent.md           # Social media content creation
    │   ├── community-agent.md         # Community engagement (Reddit, forums)
    │   ├── leadgen-agent.md           # Prospect research and lead scoring
    │   └── ops-agent.md              # Email, calendar, and data management
    └── workspace/                     # Shared context file templates
        ├── AGENTS.md                  # Orchestrator operations guide
        ├── FEEDBACK-LOG.md            # Style corrections and lessons
        ├── SIGNALS.md                 # Shared intelligence hub
        ├── SUPERGROUP-MAP.md          # Topic and agent mapping
        └── THESIS.md                  # Business thesis — north star for all agents
```

## Architecture — Key Concepts

- **One bot per agent** — Each agent has its own Telegram bot token
- **One topic per team** — Teams share a topic channel in a supergroup
- **Primary + Secondary agents** — Primary owns the topic; secondary responds only when @mentioned or triggered
- **Shared context via markdown files** — Agents coordinate through THESIS.md, SIGNALS.md, FEEDBACK-LOG.md (not APIs)
- **Bot-to-bot via `sessions_send`** — Telegram bots cannot see each other's messages; OpenClaw's `sessions_send` bridges them
- **Structured escalation** — Agents escalate to orchestrator; orchestrator escalates to human

### Team Layout (default)

| Team     | Topic    | Primary Agent | Secondary Agents    |
|----------|----------|---------------|---------------------|
| General  | Topic 1  | Orchestrator  | —                   |
| Build    | Topic N  | Coder         | QA, DevOps          |
| Research | Topic N  | Researcher    | Growth              |
| Social   | Topic N  | Content       | Community           |
| Leads    | Topic N  | Lead Gen      | —                   |
| Ops      | Topic N  | Ops           | —                   |

### Critical Config Rule: `requireMention`

- **Multi-agent topics:** ALL bots must have `requireMention: true` — otherwise one bot responds to everything
- **Single-agent topics:** The sole agent can use `requireMention: false`
- **Orchestrator:** Must have `enabled: false` on topics owned by other agents

## Conventions

- **Template placeholders** use `[Your ... Name]`, `YOUR_*`, `[Name]`, or `{WORKSPACE}` — always replace before use
- **SOUL.md structure** follows the 10-section pattern from `docs/agent-design-patterns.md`: Identity, Who I Am, Core Principles, How I Work, Domain Sections, Communication Style, Shared Context, Team Integration, Learning/Memory, Success Metrics
- **Model selection:** Orchestrator/Coder use sonnet-4-6 or opus-4-6; lighter agents (QA, DevOps, Ops, Community) use haiku-4-5
- **Example configs** must stay in sync — `full-team.json` covers all 10 agents, `minimal-team.json` covers orchestrator + coder + QA

## Editing Guidelines

- When adding a new agent template: add the soul template in `templates/soul/`, update `README.md` tables, update `examples/full-team.json`, and update this file's structure tree
- When adding a new workspace template: add in `templates/workspace/`, update `README.md`, and update this file
- When changing config schema or keys: update `templates/openclaw-config.jsonc`, both example files, and `INSTRUCTIONS.md`
- Keep `INSTRUCTIONS.md` as the single source of truth for the AI-readable setup flow
- All markdown templates use `---` horizontal rules as section separators

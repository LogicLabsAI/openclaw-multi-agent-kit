# Supergroup Setup Guide

Step-by-step guide to setting up a Telegram supergroup for your agent team.

## Prerequisites

- A Telegram account
- OpenClaw installed and running
- Anthropic API key (or other LLM provider configured)

## Step 1: Create the Supergroup

1. Open Telegram
2. Create a new group
3. Convert it to a supergroup (Settings > Group Type > Supergroup)
4. Enable Topics (Settings > Topics > Enable)
5. Note the group ID (use a bot like @raw_data_bot or check OpenClaw logs)

## Step 2: Create Topics

Create a topic for each team:
- **General** (topic 1, auto-created) — Orchestrator
- **Build** — Coding + QA + DevOps
- **Research** — Market Research + Growth Analytics
- **Social** — Content + Community
- **Leads** — Lead Generation
- **Ops** — Email, Calendar, Data

Note each topic ID.

## Step 3: Create Bots

For each agent, go to [@BotFather](https://t.me/BotFather):

1. `/newbot`
2. Name: `[AgentName] Bot`
3. Username: `[agentname]_bot`
4. Save the token
5. `/setjoingroups` > Enable (so the bot can be added to groups)
6. `/setprivacy` > Disable (so the bot can read messages in groups)

## Step 4: Add Bots to Supergroup

Add each bot to your supergroup as admin with these permissions:
- Send messages
- Read messages
- Pin messages (optional)

## Step 5: Configure OpenClaw

See the example configs in `examples/`.

Key concepts:
- **Primary agent** on a topic: `requireMention: false` (responds to all messages) — ONLY if it's the ONLY agent in that topic
- **Secondary agent** on a topic: `requireMention: true` (only responds when @mentioned)
- **Multi-agent topics**: ALL agents must have `requireMention: true` — otherwise the bot with `false` will respond to everything, including when you @mention other agents
- **Disabled topics**: topics where the orchestrator should NOT respond (handled by other bots)

## Step 6: Test

1. Restart OpenClaw: `openclaw restart`
2. Send a message in each topic
3. Verify the correct bot responds
4. Test `sessions_send` between agents using the structured format in `docs/inter-agent-handoff-standard.md`

## Troubleshooting

- **Bot not responding:** Check bot token, verify bot is admin in group
- **Wrong bot responds:** Check topic config, ensure requireMention settings are correct
- **Bot-to-bot messages not working:** Bots cant see each other — use `sessions_send`

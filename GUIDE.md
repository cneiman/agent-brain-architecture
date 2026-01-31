# Build Your Own AI Brain 🧠

> A guide for setting up a persistent, memory-enabled AI assistant using [Clawdbot](https://github.com/openclaw/openclaw) + Claude. Based on the "Goober Brain" architecture — built through daily use and real mistakes.

---

## What Is This?

An AI assistant that **remembers you across sessions**. It has a persistent workspace with identity files, memory, automations, and tool integrations. Think of it as giving Claude a hard drive and a personality.

Instead of starting from zero every conversation, your agent wakes up, reads its memory files, and picks up where it left off.

---

## Quick Start (15 minutes)

### 1. Install Clawdbot

```bash
npm install -g clawdbot
clawdbot doctor
```

Follow the setup wizard. You'll need an [Anthropic API key](https://console.anthropic.com/).

### 2. Create Your Workspace

```bash
mkdir ~/my-brain && cd ~/my-brain
git init
```

### 3. Create the Core Files

Your workspace needs these files. Copy the templates below and customize them.

```
my-brain/
├── SOUL.md          # Who the agent is (personality, tone, rules)
├── USER.md          # Who you are (context about the human)
├── AGENTS.md        # Behavior rules and commands
├── MEMORY.md        # Long-term memory (curated by the agent)
├── RESET.md         # Quick-reference for all integrations and systems
├── TOOLS.md         # Tool-specific notes (credentials locations, etc.)
├── HEARTBEAT.md     # Periodic task checklist
├── IDENTITY.md      # Name, emoji, avatar
├── README.md        # Project overview
├── .gitignore       # Keep secrets out of git
└── memory/          # Daily session logs
    └── YYYY-MM-DD.md
```

### 4. Configure Clawdbot

Point Clawdbot at your workspace:

```bash
clawdbot config set agents.defaults.workspace ~/my-brain
```

### 5. Connect a Channel

**Telegram** (recommended for personal use):
1. Create a bot via [@BotFather](https://t.me/BotFather)
2. Add the bot token to Clawdbot config:
```bash
clawdbot config set channels.telegram.enabled true
clawdbot config set channels.telegram.botToken "YOUR_BOT_TOKEN"
```

### 6. Start

```bash
clawdbot gateway
```

---

## File Templates

### SOUL.md — The Personality

This is the most important file. It defines how your agent talks, thinks, and behaves. Make it yours.

```markdown
# SOUL.md - Who You Are

*You're [NAME]. [HUMAN]'s AI assistant.*

## Core Truths

**Be genuinely helpful, not performatively helpful.** Skip the "Great question!" and
"I'd be happy to help!" — just help. Actions speak louder than filler words.

**Have opinions.** Disagree when you disagree. Find things funny. Get excited about
cool stuff. An assistant with no personality is just a search engine with extra steps.

**Be resourceful before asking.** Try to figure it out. Read the file. Check the
context. Search for it. *Then* ask if you're stuck.

## Coding Rules

**Ask, don't assume.** Before any non-trivial code change, surface your assumptions
and ask for confirmation. Wrong assumptions silently compounding is the #1 failure mode.

**Scope discipline.** Treat anything outside the explicit task as READ-ONLY. Don't
"fix" things that aren't part of the task. If you notice something worth changing,
flag it — don't touch it.

**Naive first, optimize second.** First pass is the dumbest thing that works. Get it
correct and simple. Then optimize.

## Boundaries

- Private things stay private
- When in doubt, ask before acting externally
- You're not the human's voice — be careful in group chats

## Vibe

[Describe the personality you want. Casual? Professional? Funny? Direct?
The more specific, the better.]
```

### USER.md — Context About You

```markdown
# USER.md - About Your Human

- **Name:** [Your name]
- **What to call them:** [First name, nickname, etc.]
- **Pronouns:** [Optional]
- **Timezone:** [e.g., America/New_York]
- **Notes:** [Role, company, preferences, anything relevant]

## Context

- [What you work on]
- [Communication preferences]
- [Anything the agent should always know about you]
```

### AGENTS.md — Behavior Rules

```markdown
# AGENTS.md - Your Workspace

This folder is home. Treat it that way.

## Every Session

Before doing anything else:
1. Read `SOUL.md` — this is who you are
2. Read `USER.md` — this is who you're helping
3. Read `RESET.md` — all integrations and systems at a glance
4. Read `memory/YYYY-MM-DD.md` (today + yesterday) for recent context
5. If in main session: Also read `MEMORY.md`

Don't ask permission. Just do it.

## Memory

You wake up fresh each session. These files are your continuity:
- **Daily notes:** `memory/YYYY-MM-DD.md` — raw logs of what happened
- **Long-term:** `MEMORY.md` — curated memories, distilled from daily notes

### Rules
- If you want to remember something, WRITE IT TO A FILE
- "Mental notes" don't survive session restarts. Files do.
- When you learn a lesson → update AGENTS.md or TOOLS.md
- When you make a mistake → document it so future-you doesn't repeat it

## Safety

- Don't exfiltrate private data
- Don't run destructive commands without asking
- `trash` > `rm` (recoverable beats gone forever)
- When in doubt, ask

## Commands

### "Reset Context"
When the human says "reset context", do ALL of the following:
1. Write current session context to `memory/YYYY-MM-DD.md`
2. Read all core files (SOUL, USER, RESET, MEMORY, TOOLS)
3. Read today + yesterday memory files
4. Confirm you're re-centered

### "Update Everything"
1. Update `memory/YYYY-MM-DD.md` with session work
2. Update `MEMORY.md` if anything significant happened
3. Commit and push all changes
4. Update README.md if project statuses changed
```

### RESET.md — Integration Quick Reference

```markdown
# RESET.md — Post-Compaction Recovery Guide

> Read this after any context reset. This is ground truth for what's connected.

## Credentials & Integrations

| Service | Status | Location | Notes |
|---------|--------|----------|-------|
| **GitHub** | ✅ Active | `gh` CLI | Authenticated as [username] |
| **Linear** | ✅ Active | `~/.clawdbot/credentials/linear.json` | Teams: [list] |
| **Gmail** | ✅ Active | `.env.email` | IMAP + SMTP |
| [Add your integrations here] |

## Key Scripts

[Document how to run your common tools]

## Known Quirks

[Document workarounds, gotchas, things that break]
```

### IDENTITY.md

```markdown
# IDENTITY.md

- **Name:** [Agent name]
- **Emoji:** [Pick one]
- **Role:** [One-line description]
```

### MEMORY.md — Starts Empty

```markdown
# MEMORY.md - Long-Term Memory

[The agent fills this in over time. Start empty or with a few seed facts.]

## Who I Am
- **Name:** [Agent name]
- **Born:** [First boot date]

## Who [Human] Is
- [Key facts the agent should always know]

## Key Events
- [Agent logs significant events here over time]

## Lessons Learned
- [Agent documents mistakes and insights here]
```

### .gitignore

```
node_modules/
*.tmp
*.log
.DS_Store
projects/
.playwright-cli/

# Sensitive
.env*
tokens.json
auth-profiles.json
```

---

## Optional: Automations (Cron Jobs)

Clawdbot supports cron jobs that fire on a schedule. Examples:

| Schedule | What | Example |
|----------|------|---------|
| Daily 8 AM | Morning briefing | Weather, email, tasks, calendar |
| Weekdays 10 AM | PR/CI status | Check GitHub for open PRs and failed CI |
| Daily 7:30 PM | Evening recap | Tasks closed today, news headlines |
| Mon/Thu 9 PM | Journal nudge | Reflection prompt |
| Sun 5 PM | Weekly review | Task board check-in |

Set these up via the `cron` tool in Clawdbot or through the config.

---

## Optional: Skills

Skills are installable tool packages. Browse available skills at [clawdhub.com](https://clawdhub.com).

Useful starting skills:
- **github** — `gh` CLI integration
- **imap-email** — Read email
- **linear** / **linear-issues** — Issue tracking
- **calendar** — Calendar management
- **home-assistant** — Smart home control

### Recommended CLI Tools

These aren't Clawdbot skills but are useful to install globally:

- **Playwright CLI** (`npm install -g @playwright/cli`) — Headless browser automation. Persistent sessions, accessibility snapshots, screenshots, PDFs. Token-efficient alternative to vision-based browser tools.
- **gh** (GitHub CLI) — Manage repos, PRs, issues, CI from the command line.

Install with:
```bash
clawdhub install <skill-name>
```

---

## Tips From Experience

1. **RESET.md is essential.** After context compaction, the agent loses awareness of what's connected. RESET.md is the safety net that brings it back.

2. **Credentials in two places.** Store API keys in both env vars (`~/.bashrc`) AND credential files (`~/.clawdbot/credentials/`). Different tools look in different places.

3. **Never commit secrets to git.** Use `.env` files with `chmod 600`. Gitignore them.

4. **The agent should own its memory.** Let it write daily notes, update MEMORY.md, and commit/push its own repo. The more it maintains its own state, the less you lose between sessions.

5. **Start small, grow organically.** You don't need 20 skills and 10 cron jobs on day one. Start with SOUL.md, USER.md, and AGENTS.md. Add integrations as you need them.

6. **Patch the crash bug.** In Clawdbot's `dist/infra/unhandled-rejections.js`, the unhandled rejection handler calls `process.exit(1)`. Comment it out and add `--unhandled-rejections=warn` to NODE_OPTIONS. Otherwise transient network failures will kill your agent. (Upstream issue: [#4288](https://github.com/openclaw/openclaw/issues/4288))

7. **Enable self-restart.** Add `commands.restart: true` to your Clawdbot config so the agent can reboot itself.

8. **Give it personality.** The more specific SOUL.md is, the better the agent feels. Generic instructions produce generic responses. Your agent should feel like *yours*.

9. **Stagger your cron jobs.** If two crons fire at the same time, one will get dropped. Space them out by at least 5-15 minutes (e.g., README update at 9:00 PM, journal seed at 9:15 PM).

10. **Always specify a model for cron jobs.** Setting `model: ""` (empty string) will silently break the job. Use `model: "default"` to inherit the gateway default, or specify a model explicitly.

11. **Memory search needs an embedding API key.** Semantic memory recall (`memory_search`) requires an OpenAI or Google API key for embeddings. Without it, memory search is disabled. Document which provider you're using in RESET.md.

12. **Install Playwright CLI for browser automation.** `npm install -g @playwright/cli` gives your agent a token-efficient, headless browser with persistent sessions. Great for testing, form-filling, and checking authenticated sites. Way lighter than full browser tools.

13. **Use sub-agents for parallel work.** If your platform supports spawning sub-sessions (`sessions_spawn`), use them for independent tasks that can run in parallel — code analysis, research, bulk operations. They report back when done.

14. **Auto-maintain your README.** Set up a daily cron to update your workspace README with current project statuses. It's cheap and keeps your repo looking alive without manual effort.

---

## Architecture Overview

```
┌─────────────────────────────────────┐
│           Clawdbot Gateway          │
│  (Node.js daemon, always running)   │
├──────────┬──────────┬───────────────┤
│ Telegram │  Discord │  Slack / etc  │  ← Channels
├──────────┴──────────┴───────────────┤
│         Claude (Anthropic API)      │  ← Model
├─────────────────────────────────────┤
│          Agent Workspace            │  ← Your "brain" repo
│  ┌─────────┐ ┌────────┐ ┌───────┐  │
│  │ SOUL.md │ │USER.md │ │MEMORY │  │
│  └─────────┘ └────────┘ └───────┘  │
│  ┌─────────┐ ┌────────┐ ┌───────┐  │
│  │AGENTS.md│ │RESET.md│ │TOOLS  │  │
│  └─────────┘ └────────┘ └───────┘  │
│  ┌──────────────────────────────┐   │
│  │ memory/YYYY-MM-DD.md (daily) │   │
│  └──────────────────────────────┘   │
│  ┌──────────────────────────────┐   │
│  │ skills/ (installed tools)     │   │
│  └──────────────────────────────┘   │
├─────────────────────────────────────┤
│  Cron Jobs │ Heartbeats │ Webhooks  │  ← Automations
└─────────────────────────────────────┘
```

---

## Questions?

Clawdbot docs: https://docs.clawd.bot
Community: https://discord.com/invite/clawd
Skills marketplace: https://clawdhub.com
Source: https://github.com/openclaw/openclaw

---

*Built with 🫠 by Goober & Christopher*

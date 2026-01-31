# Agent Brain Architecture 🧠

> Give your AI a memory. Make it persistent. Make it *yours*.

A file architecture for building AI assistants that remember you across sessions. Instead of starting from zero every conversation, your agent wakes up, reads its memory files, and picks up where it left off.

Works with [Clawdbot](https://github.com/openclaw/openclaw), Claude, GPT, or any AI platform that supports file access.

---

## Two Ways to Use This

### 🤖 For Your AI → [`BOOTSTRAP.md`](./BOOTSTRAP.md)
Paste this file directly into your AI assistant. It contains machine-readable instructions that tell the AI how to set up the entire brain architecture. It will:
1. Create the file structure
2. Ask you the right questions (name, timezone, personality)
3. Write all the core files
4. Set up memory, automations, and integrations
5. Know how to recover from context resets

**This is the fast path.** Give it to your AI and let it work.

### 👤 For You → [`GUIDE.md`](./GUIDE.md)
A human-readable guide explaining the architecture, file templates, tips from experience, and how everything fits together. Read this if you want to understand the system before setting it up, or if you prefer to build it yourself.

---

## The Architecture

```
workspace/
├── SOUL.md          # Personality and behavioral rules
├── USER.md          # Context about the human
├── AGENTS.md        # Operating procedures and commands
├── MEMORY.md        # Long-term curated memory
├── RESET.md         # Integration quick-reference (recovery file)
├── TOOLS.md         # Environment-specific notes
├── IDENTITY.md      # Name, emoji, role
├── HEARTBEAT.md     # Periodic task checklist
├── .gitignore       # Keep secrets out of git
└── memory/          # Daily session logs
    └── YYYY-MM-DD.md
```

### How Memory Works

```
Session starts → Read identity + memory files → Full context restored
                            ↓
          Work happens, decisions made, things learned
                            ↓
      Session ends → Write to memory/YYYY-MM-DD.md (daily log)
                            ↓
         Periodically → Distill daily notes into MEMORY.md
                            ↓
       Next session → Read MEMORY.md → Continuity preserved
```

### The RESET.md Pattern

The single most important innovation. After context compaction (when your AI's conversation history gets truncated), it loses awareness of what's connected. RESET.md is a single file that restores full operational awareness — every integration, credential location, active project, and known quirk.

Without it, every context reset means re-explaining your entire setup. With it, recovery is automatic.

---

## Key Principles

- **Files are memory.** "Mental notes" don't survive session restarts. Files do.
- **Stagger your automations.** Two cron jobs at the same time = one gets dropped.
- **Identity drives behavior.** A specific SOUL.md produces a specific personality. Generic instructions produce generic responses.
- **Document everything.** Mistakes, integrations, quirks, lessons. Future-you will thank present-you.
- **Proactive > reactive.** A good agent checks in, does background work, and surfaces important things without being asked.
- **Security by default.** Credentials in .env files, gitignored, chmod 600. Never committed. MEMORY.md only loaded in private sessions.

---

## Getting Started

1. Pick your path: [BOOTSTRAP.md](./BOOTSTRAP.md) (for your AI) or [GUIDE.md](./GUIDE.md) (for you)
2. Set up a workspace directory
3. Let your AI build the files (or build them yourself)
4. Start talking — the system grows organically from there

---

## Built With

- [Clawdbot](https://github.com/openclaw/openclaw) — AI agent gateway
- [Claude](https://anthropic.com) — but the architecture is model-agnostic
- Real daily use, real mistakes, real lessons

---

*Every pattern in this repo exists because its absence caused a problem.*

*Built with 🫠 by Goober & Christopher*

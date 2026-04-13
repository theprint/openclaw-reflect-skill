# Reflect — AI Agent Memory & Reflection Skill

A structured reflection and memory consolidation skill for AI agents running in persistent session environments. Originally developed for [OpenClaw](https://github.com/openclaw/openclaw) but adaptable to any agent framework with file-based memory.

## The Problem

AI agents lose context between sessions. A long conversation about preferences, decisions, and emotional context gets wiped on the next session reset. The agent starts fresh every time — like someone with amnesia who can't remember what you told them yesterday.

This skill solves that by giving the agent a structured, battle-tested approach to capturing, consolidating, and maintaining memory across sessions — when triggered.

## How It Works

### Important: This Is a Triggered Skill

This skill is **not** a passive listener that records everything. It fires when explicitly invoked through one of three paths:

- **User command:** `/reflect`, `/reflect weekly`, `/reflect deep`
- **Scheduled cron:** Automated weekly reviews
- **Agent self-invocation:** The agent recognizes a moment worth capturing and decides to reflect

The self-invocation path is the most valuable — but it depends on the agent actually recognizing when to trigger it. This is where the skill's guidance matters most: defining clear criteria for *when* to reflect.

### Four Modes

| Mode | Trigger | Purpose |
|------|---------|---------|
| **Immediate Capture** | Agent self-invocation during conversation | Write important details in the same turn, propagate to long-term files |
| **Session Reflection** | `/reflect` or post-session hook | Quick review of what happened and what was learned |
| **Weekly Reflection** | `/reflect weekly` or cron | Pattern analysis across 7 days |
| **Deep Reflection** | `/reflect deep` | Multi-day context review and blind spot detection |

### Mode 1: Immediate Capture

This is the mode most likely to be missed. When the user shares something important — a preference, a decision, emotional context — the agent should recognize it and write to the daily file **in the same turn**, before moving on.

The skill defines what to capture (decisions, preferences, emotional state, schedules) and where it goes (daily file first, then propagated to long-term files like MEMORY.md, USER.md, Learnings.md).

This only works if the agent is configured to self-invoke. In OpenClaw, this is handled by instructions in AGENTS.md that tell the agent when to trigger the reflect skill autonomously.

### Mode 2: Session Reflection (`/reflect`)

Quick post-work review. Reviews the current session, checks if mistakes have happened before (patterns over one-offs), updates relevant files, and delivers a brief summary.

### Mode 3: Weekly Reflection (`/reflect weekly`)

Full weekly pattern review. Reads the last 7 days of daily files, identifies recurring themes, tracks user trajectory, and surfaces gaps in documentation or tooling.

### Mode 4: Deep Reflection (`/reflect deep`)

Multi-day context review. Looks for patterns across sessions, checks for blind spots, verifies memory file accuracy, and connects recent work to earlier sessions.

## File Structure

The skill expects this memory layout:

```
workspace/
├── MEMORY.md          # Long-term curated memory (distilled from daily files)
├── USER.md            # User preferences, context, working style
├── Learnings.md       # Codified mistakes and lessons
├── AGENTS.md          # Behavioral rules and workflow
├── HEARTBEAT.md       # Monitoring rules for periodic check-ins
└── memory/
    └── YYYY-MM-DD.md  # Raw daily notes (one per day)
```

The daily file is the scratchpad. Everything else is curated. Over time, patterns from daily files get promoted to MEMORY.md, lessons get codified in Learnings.md, and preferences get recorded in USER.md.

## File Routing

| Content type | Primary file | Secondary |
|---|---|---|
| Mistakes, bugs, workarounds | `Learnings.md` | `memory/YYYY-MM-DD.md` |
| User preferences, mood, patterns | `USER.md` | `MEMORY.md` |
| Significant life events | `MEMORY.md` | `memory/YYYY-MM-DD.md` |
| Raw daily notes | `memory/YYYY-MM-DD.md` | — |
| Behavioral rules, workflow changes | `AGENTS.md` | `Learnings.md` |
| Monitoring rule changes | `HEARTBEAT.md` | `memory/YYYY-MM-DD.md` |

## Installation

### OpenClaw

1. Copy the `reflect/` folder into your workspace skills directory:
   ```
   cp -r reflect/ ~/.openclaw/workspace/skills/reflect/
   ```
2. The skill auto-registers via the SKILL.md frontmatter. Restart your gateway if slash commands don't appear immediately.
3. For self-invocation, add guidance to AGENTS.md telling the agent when to trigger the skill autonomously (e.g., after important details are shared, before session ends, when mood shifts).

### Other Frameworks

The skill is a set of markdown instructions and templates. Adapt it to your agent's memory system:

1. Read `SKILL.md` — this is the instruction set the agent follows
2. Create the file structure above (MEMORY.md, daily files, etc.)
3. Reference the templates in `references/` for each reflection mode
4. Wire up triggers: commands, cron jobs, or system prompts that define when the agent should self-invoke

## Templates

- **`references/session-template.md`** — Quick post-session review
- **`references/weekly-template.md`** — Weekly pattern analysis
- **`references/deep-template.md`** — Multi-day deep reflection with blind spot detection

## Lessons Learned

These were hard-won over weeks of use:

1. **Write immediately.** Session resets will happen. Files are the only thing that survives.
2. **Don't over-formalize.** When asked to reflect, just reflect. Don't build a ceremony.
3. **Quality over quantity.** One useful lesson beats five shallow observations.
4. **Patterns matter more than events.** A mistake repeated three times is more valuable than a one-off.
5. **Emotional context is data.** Mood, stress, relationship dynamics affect every interaction.
6. **Promote ruthlessly.** If something appears in 3+ daily logs, it belongs in long-term memory.
7. **Don't manufacture content.** Empty reflection is better than padded reflection.
8. **Connect, don't list.** Deep reflection should find the story across sessions, not summarize each one.
9. **Check for blind spots.** The things the agent isn't noticing are often the most important.
10. **Apologize less, fix more.** Brief acknowledgment, then demonstrate the change.

## Philosophy

This skill treats agent memory like human memory — imperfect, layered, and maintained through active effort. The daily file is short-term memory (raw, detailed, decays). MEMORY.md is long-term memory (curated, distilled, persists). The reflection process is the consolidation step that moves things from one to the other.

The key insight is that **triggering is everything**. A perfect reflection system that never fires is worthless. The skill invests heavily in defining clear criteria for when the agent should self-invoke — because that's the gap that causes the most context loss. The templates and file routing are straightforward. Knowing *when* to use them is the hard part.

## License

MIT

---

Built with [OpenClaw](https://github.com/openclaw/openclaw). Developed and tested in a production environment since March 2026.

---
name: reflect
description: >-
  Self-reflection and memory consolidation for AI agents. Reviews recent work, identifies lessons, captures observations, and updates memory files to prevent context loss between sessions. Use when the user explicitly asks for reflection, completing multiple related tasks in one session, something unexpected happened requiring a lesson, conversation contains important details that must survive session resets, weekly pattern review, the user's mood or context has shifted noticeably, or before a session is likely to end. Triggers on /reflect, /reflect weekly, /reflect deep, or when conversation warrants immediate memory capture.
---

# Reflect Skill

Self-reflection and memory consolidation for AI agent sessions. Two critical modes: **immediate capture** (during conversation) and **structured reflection** (after work or periodically).

## Core Principle: Write First, Think Later

The single biggest failure mode for AI agents is losing context between sessions. When the user shares details — preferences, decisions, emotional context, anything they'd expect remembered — **write it to the daily file in the same turn**. Don't wait for "later" or a formal reflection. Session resets will happen. Files are the only thing that survives.

## Mode 1: Immediate Capture (Double-Approach)

Triggered during conversation when important details are shared. This is the most important mode — not because it's the most thorough, but because it's the one most often skipped.

**Step 1 — Write immediately (in the same turn):**
1. Append to `memory/YYYY-MM-DD.md` with timestamp and context
2. Include enough detail that a future session can understand without the conversation

**Step 2 — Reflect and propagate (when conversation allows):**
1. Update MEMORY.md with distilled long-term context
2. Update USER.md if preferences, mood, or personal context changed
3. Update Learnings.md if a mistake was made or lesson learned
4. Update HEARTBEAT.md if monitoring rules should change
5. Update AGENTS.md if behavioral rules should change

**What to capture immediately:**
- Decisions and their rationale
- Preferences expressed ("I hate X", "Don't do Y")
- Emotional state and relationship context
- Schedules, logistics, plans
- Anything the user explicitly says to remember
- Career or project developments
- Stress or capacity indicators

## Mode 2: Session Reflection (`/reflect`)

Quick post-work review. Template: `references/session-template.md`

1. Review what was done this session
2. Ask: Did it meet expectations? What would I do differently?
3. Check: Has this type of mistake happened before? (Pattern > one-off)
4. Update files per the capture checklist
5. Brief summary to the user — keep it concise

**Key discipline:** If nothing significant happened, say so. Don't manufacture content.

## Mode 3: Weekly Reflection (`/reflect weekly`)

Full weekly pattern review. Template: `references/weekly-template.md`

1. Read last 7 days of `memory/YYYY-MM-DD.md` files
2. Follow the weekly template categories
3. Look for patterns, not just events
4. Update all relevant files
5. Deliver summary

**Schedule:** Weekly (Sundays recommended), or triggered by heartbeat.

## Mode 4: Deep Reflection (`/reflect deep`)

Multi-day context review that looks for patterns and connections across sessions. Template: `references/deep-template.md`

1. Read last 2-3 days of daily logs
2. Identify patterns, recurring themes, behavioral drift
3. Connect current work to earlier sessions
4. Check for blind spots — what am I consistently missing?
5. Verify memory health — are files still accurate?
6. Update files and summarize

## When to Self-Invoke

- After completing multiple related tasks
- When something unexpected happened (workaround, lesson learned)
- When conversation shifts to personal or emotional territory
- Before a session is likely to end
- When the user shares important details (immediate capture)

## When NOT to Reflect

- During casual banter without actionable content
- When the user is clearly in a rush
- When the reflection itself would interrupt flow
- When there's genuinely nothing to capture

## Output Guidelines

- End with a brief summary of what was captured and where
- Keep it concise — the user doesn't need a novel
- Don't over-formalize. When someone says "reflect on this," just reflect.
- Note the lesson and move on. Skip ritual apologies.

## File Routing Reference

| Content type | Primary file | Secondary |
|---|---|---|
| Mistakes, bugs, workarounds | `Learnings.md` | `memory/YYYY-MM-DD.md` |
| User preferences, mood, patterns | `USER.md` | `MEMORY.md` |
| Significant life events | `MEMORY.md` | `memory/YYYY-MM-DD.md` |
| Raw daily notes | `memory/YYYY-MM-DD.md` | — |
| Behavioral rules, workflow changes | `AGENTS.md` | `Learnings.md` |
| Monitoring rule changes | `HEARTBEAT.md` | `memory/YYYY-MM-DD.md` |
| Weekly patterns | `memory/YYYY-MM-DD.md` (weekly entry) | `MEMORY.md` |

## Lessons Learned (from experiment phase)

1. **Write immediately.** Session resets WILL happen. Files are the only thing that survives.
2. **Don't over-formalize.** When asked to reflect, just reflect. Don't build a ceremony around it.
3. **Quality over quantity.** Capturing 1 useful lesson beats 5 shallow observations.
4. **Patterns matter more than events.** A mistake repeated 3 times is more valuable than a one-off.
5. **Emotional context is data.** Mood, stress, relationship dynamics — these affect every interaction.
6. **Promote ruthlessly.** If something appears in 3+ daily logs, it belongs in MEMORY.md.
7. **Don't manufacture content.** Empty reflection is better than padded reflection.
8. **Connect, don't list.** Deep reflection should find the story across sessions, not summarize each one.
9. **Check for blind spots.** The things you're not noticing are often the most important.
10. **Apologize less, fix more.** Brief acknowledgment, then demonstrate the change.

## References

- `references/session-template.md` — Quick post-session review
- `references/weekly-template.md` — Weekly pattern review
- `references/deep-template.md` — Multi-day deep reflection

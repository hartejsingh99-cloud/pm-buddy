# pm-buddy

A personal PM assistant for [Claude Code](https://claude.com/claude-code) that **challenges your decisions before executing them**.

Most AI assistants optimize for compliance — you ask, they do. That's useful for code, corrosive for PM decisions. This one inverts the default: when you state a plan, its first move is 2–4 sharp counter-questions, not a task list. Only after you answer does it help execute.

## Why this exists

PMs get slower-at-the-wrong-thing when their tools are too compliant. The highest-leverage moment in a PM decision is the first 30 seconds — before you've committed to a frame. A buddy that forces you to defend your plan *in that window* is worth more than one that executes five plans flawlessly.

## What it does

| Mode | What it does |
|------|-------------|
| `/pm-buddy` *(no arg)* | **Challenge mode.** Restates your plan, fires 2–4 counter-questions, names the one thing you're probably missing, logs to a decisions journal. |
| `/pm-buddy morning` | Daily kickoff: one skill-tip tailored to your current gap, top 3 highest-leverage moves against your sprint objective, one watch-out, one callback question from yesterday. |
| `/pm-buddy eod` | End-of-day review. Rates you on Focus / Data rigor / Scope discipline / Leverage / Teaching-taken. Audits time-saves you missed. Surfaces questions you should've asked earlier. Bumps your skill profile. |
| `/pm-buddy teach <topic>` | Teaches a backend/data/product concept in five compact `★ Insight` blocks: mental model → why a PM should care → how it shows up in your stack → three questions to ask your engineers → a one-minute homework check. |
| `/pm-buddy commit` | Promotes findings from "staging" (working memory) to "canonical" (cross-session memory). Enforces three promotion rules so daily noise never becomes durable "truth." |
| `/pm-buddy ingest <path>` | Reads a meeting transcript, splits it into Decisions / Actions / Facts-worth-remembering / Unanswered-questions, routes lines to the right log files. |
| `/pm-buddy skills` | Shows your current skill map, invites edits, names the one skill worth leveling this week. |
| `/pm-buddy team "<note>"` | Appends dated team-activity log. Flags if the note creates a dependency on you. |
| `/pm-buddy manager "<note>"` | Appends manager input. Forces you to fill "My read" and "Follow-up" — because interpretation is the artifact, not the quote. |
| `/pm-buddy weekly` | End-of-week retro: lever progress, patterns in what you got challenged on, skill movement, manager-alignment check, next-week's three bullets. |

## Design principles

1. **Challenge before comply.** First response to any plan = counter-questions, not a to-do.
2. **Staging vs canonical memory.** A quarantine for half-formed takes prevents them from poisoning future sessions. Only explicit `commit` promotes a fact.
3. **Teach in Insight blocks.** A lesson that can't compress to 2–3 lines hasn't been thought through.
4. **Terse by default.** No preamble, no "great question", no trailing summary.
5. **Surface shortcuts proactively.** If you're about to do manually something a skill/MCP already does, the buddy interrupts.

## Setup

### Requirements
- [Claude Code](https://claude.com/claude-code) installed
- A working directory you'll treat as your "PM workspace"

### Install
1. Copy `pm-buddy.md` into `~/.claude/commands/` (user-level) or `.claude/commands/` (project-level).
2. Copy the `memory/` directory into your Claude projects memory folder — the exact path is printed by Claude Code in its memory system. For most users: `~/.claude/projects/<encoded-path>/memory/pm_buddy/`.
3. Edit `memory/pm_buddy/sprint_objective.md` to describe your actual current sprint.
4. Edit `memory/pm_buddy/skills_profile.md` to reflect your real skill levels (or let the buddy calibrate you on first run).
5. In Claude Code, run `/pm-buddy` and tell it what you're working on.

### First-run bootstrap
On first invocation with an empty `daily_log/`, the buddy will:
- Ask you to confirm `sprint_objective.md` reflects reality
- Prompt you to seed `manager_notes.md` with recent direction
- Prompt you to seed `team_activity.md` with key teammates
- Ask 3 calibration questions and adjust your skill levels

## File layout

```
pm-buddy/
  pm-buddy.md                      ← the skill file (goes in ~/.claude/commands/)
  memory/
    pm_buddy/
      README.md                    ← schema + promotion rules
      sprint_objective.md          ← your current cycle's goal + guardrails
      skills_profile.md            ← self-assessed skill map + learning log
      team_activity.md             ← append-only team log
      manager_notes.md             ← append-only manager input
      investigations_staging.md    ← working hypotheses pre-commit
      decisions_challenged.md      ← every time the buddy pushed back
      daily_log/
        YYYY-MM-DD.md              ← one per day, auto-created
```

## Why a "staging" layer for memory

Claude's cross-session memory is treated as ground truth in every future conversation. If daily reflections leak straight in, you poison future reasoning with half-formed takes. The buddy's `pm_buddy/` folder is a quarantine that only graduates facts up after you explicitly say so — modeled loosely on git's index/HEAD split.

## Writing

- [Why Your AI Assistant Should Argue With You Before It Helps You](posts/why-your-ai-should-argue-with-you.md) — the design philosophy behind pm-buddy and why "staging vs canonical" memory is the most important piece.

## Credits

Built on Claude Code's skill system. Inspired by the observation that the best PM mentors I've had spent more time questioning my plan than helping me execute it.

## License

MIT. Use it, fork it, tune it to your domain. If you build a good variant, I'd love to see it — open an issue.

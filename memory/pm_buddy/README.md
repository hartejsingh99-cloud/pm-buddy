# PM Buddy — Working Memory Schema

This directory is the **working memory** for the `/pm-buddy` skill. It is
*separate* from Claude's canonical cross-session memory, and intentionally so.

## Purpose

Canonical memory = things that are confirmed and should shape all future sessions.
This folder = **in-flight state**: working hypotheses, daily reflections,
team/manager context, skill-gap tracking. Contents here may be wrong, stale, or
half-formed. Only graduate items from here to canonical memory via `/pm-buddy commit`.

## Files

| File | Purpose | Who writes |
|------|---------|------------|
| `skills_profile.md` | Your current skill map + learning log. | Buddy (after eod / teach / investigations) |
| `sprint_objective.md` | Active cycle's objective + guardrails. | You set; Buddy refines |
| `team_activity.md` | Append-only log of what teammates are working on. | `/pm-buddy team ...` |
| `manager_notes.md` | Manager's suggestions, feedback, directional nudges. | `/pm-buddy manager ...` |
| `investigations_staging.md` | In-flight findings awaiting promotion. | Buddy writes; `commit` promotes |
| `daily_log/YYYY-MM-DD.md` | Per-day: tasks planned/done, transcripts, EOD. | Buddy, appended throughout day |
| `decisions_challenged.md` | Log of decisions Buddy pushed back on + outcomes. | Buddy |

## Promotion rules (staging → canonical)

An investigation graduates out of `investigations_staging.md` into canonical
memory only when:
1. You explicitly confirm it via `/pm-buddy commit`, AND
2. It is unlikely to change in the next 2 weeks (stable fact, not live hypothesis), AND
3. It is not already derivable from code / git / data warehouse.

Never promote: daily task lists, transcripts, ephemeral team status.

## Core principle

**Challenge > comply.** When you state a plan, Buddy's first move is to
counter-question, not to execute.

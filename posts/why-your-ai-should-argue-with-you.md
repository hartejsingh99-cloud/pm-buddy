# Why Your AI Assistant Should Argue With You Before It Helps You

Most AI-assisted PM workflows optimize for compliance — you ask, it executes. That's useful for code and harmful for decisions. The highest-leverage moment in a PM decision is the first 30 seconds, before you've committed to a frame. An assistant that forces you to defend your plan *in that window* is worth more than one that executes five plans flawlessly.

I built a Claude Code skill called **pm-buddy** that inverts the default: when you state a plan, its first response is 2–4 counter-questions, not a task list. Here's why, and what I learned.

## The frame

PMs get slower-at-the-wrong-thing when their tools are too helpful. Every AI assistant I've used has the same failure mode: I say "let's investigate X," and it immediately runs queries, generates a PRD, and produces a 20-page artifact before I've checked whether X is actually the right question. The output feels productive. The direction was never stress-tested.

The problem isn't the tool — it's the interaction model. Compliance is the default because it's what users reward. But a PM's job isn't to execute plans; it's to choose the right plan. That requires friction at the right moment.

## Why it's harder than it looks

**1. Challenge-mode is emotionally annoying.** You're mid-flow, you have momentum, and your assistant says "wait — why that and not this?" The urge to override it and just say "do what I asked" is strong. If the friction isn't precisely calibrated — too sharp and you stop using it, too soft and you wave through it — the behavior decays back to compliance within a week.

**2. "Staging vs canonical" prevents memory poisoning.** Cross-session memory in AI tools is treated as ground truth. If you let daily reflections leak straight in, half-formed takes become durable "truth" in future sessions. The architecture needs a quarantine layer — working memory that only graduates to permanent memory through an explicit promotion step. This turned out to be the most important design decision in the entire tool — it's what makes memory compounding safe instead of dangerous.

**3. Memory is load-bearing.** A challenge that doesn't remember yesterday's decisions is just a random interrogation. To be useful, the buddy needs to read your sprint objective, your manager's recent direction, your skill gaps, and your daily log *before* it pushes back. That means the memory schema is the real product — the challenge mode is just a lens over it.

**4. Teaching and challenging are in tension.** If the assistant teaches too much during a challenge, the counter-question becomes a lecture and the PM tunes out. If it doesn't teach at all, the same mistakes recur. The solution I landed on: challenge in the default mode, teach only when explicitly invoked, and let the EOD review bridge the two by mapping today's challenges to skill gaps.

## The playbook

What I built, and the design choices behind each piece:

**1. Challenge before comply (default mode).** Every time you state a plan, the buddy restates it in one line, fires 2–4 counter-questions targeting the weakest joints (assumptions, measurement, scope fit, alternatives you didn't list), names the one thing you're probably missing, and offers a menu: help execute, dig deeper, or log and move on. Every challenge is logged to a decisions journal with a skill-tag — so patterns accumulate.

**2. Staged memory with promotion rules.** This is the design choice that makes everything else work. Working memory (daily logs, team notes, manager input, investigation hypotheses) lives in a quarantine directory. Only the `commit` mode promotes facts to canonical cross-session memory, and only when: (a) you explicitly confirm, (b) the fact is stable for 2+ weeks, (c) it's not derivable from code or data. Transcripts, daily tasks, and ephemeral team status *never* promote. Without this, half-formed daily reflections leak into canonical memory and become load-bearing "truth" in future sessions — memory poisoning by accumulation.

**3. Morning kickoff.** Not a status update — a PM-craft layer. One skill tip rotated from your gap list. Top 3 highest-leverage moves against your sprint objective, each justified in one line. One watch-out (the meeting or task that'll eat time without moving a lever). One callback question from yesterday's log.

**4. End-of-day rating.** Five dimensions: Focus (sprint alignment), Data rigor, Scope discipline, Leverage (did you use your tools?), Teaching opportunities taken. Each scored 1–5 with a reason. Plus a time-save audit — specific moments where you went manual when a shortcut existed. And 1–2 questions you should have asked that morning.

**5. Teach in insight blocks.** When you explicitly ask to be taught a topic, the buddy delivers five compressed blocks: mental model, why a PM should care, how it appears in your specific stack, three questions to ask your engineers, one tiny homework exercise. Each block forces compression to 2–3 lines — if a lesson can't compress that far, it hasn't been thought through.

**6. Team + manager context.** Append-only logs for what teammates are working on and what your manager is suggesting. The manager-notes format forces you to fill in "my read" (your interpretation) and "follow-up" (your action) — because recording the quote without the interpretation is a wasted entry.

## How to know you got it right

- **The challenge log has entries every day.** If you go 3 days without a logged challenge, the buddy is in compliance mode and you've drifted back to the default.
- **Your EOD ratings trend upward on dimensions you were weak on.** If they don't, either the rating is soft or the teaching isn't landing.
- **You catch scope drift the same day it happens**, not in a Friday retro. The morning ritual + evening review create a 12-hour feedback loop that's faster than any human process.
- **Your staging area has entries that you *reject* during commit.** If everything always promotes, you're not being selective enough — and your canonical memory is accumulating noise.

## Closing

The most valuable PM mentors I've had spent more time questioning my plan than helping me execute it. The insight is that this pattern is automatable — not the judgment, but the *structure* of the challenge. Force 30 seconds of friction before 30 minutes of execution, and the 30 minutes point in the right direction more often.

The skill is [open-source on GitHub](https://github.com/hartejsingh99-cloud/pm-buddy). Built for Claude Code. Fork it, tune the sprint objective to your domain, and see if the friction compounds.

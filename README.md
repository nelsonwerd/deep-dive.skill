# deep-dive

Rigorous multi-agent deep-dive analysis for Claude Code. Point it at a codebase, a trading/strategy system, a design, or an open research question and it runs a structured investigation instead of giving a one-shot answer.

## What it does

For a complex, investigative task, the skill runs a multi-phase pipeline:

1. **Parallel specialists** — deploys 4–6 subagents at once, each in its own lane.
2. **Synthesis** — one agent reads every specialist output, cross-checks claims, and resolves contradictions.
3. **Follow-up verification** — single-claim agents re-check load-bearing or single-sourced numbers.
4. **Red-team** — an adversarial reviewer tries to break the conclusions.
5. **Executive briefing** — a plain-English verdict with an honest 1–10 confidence rating.

It scales down for narrow scope (2–3 lanes) and up for broad, multi-domain work (6 lanes). By default it runs in **pure research mode** — it writes markdown findings and changes no code unless you explicitly ask.

Four built-in variants live in `deep-dive/references/`: **codebase audit**, **strategy evaluation**, **design evaluation**, and **open-ended research**.

## Install

Skills live in `~/.claude/skills/` (all projects) or `.claude/skills/` (a single project). Put the `deep-dive/` folder in either one.

**From the packaged file:**

```bash
mkdir -p ~/.claude/skills
unzip deep-dive.skill -d ~/.claude/skills/
```

**Or from a clone of this repo:**

```bash
git clone https://github.com/nelsonwerd/deep-dive.skill.git
mkdir -p ~/.claude/skills
cp -r deep-dive.skill/deep-dive ~/.claude/skills/
```

No restart needed — Claude Code detects it in-session. Verify with `/skills`, or just ask Claude what skills are available, and confirm `deep-dive` is listed.

## Use it

- **Manually:** type `/deep-dive` and describe the target.
- **Automatically:** Claude invokes it on its own when a task looks like a thorough audit, rigorous analysis, or comprehensive review.

Examples:

- "Do a deep dive on this codebase before I ship it."
- "Evaluate whether this trading strategy is actually profitable."
- "Is this the right architecture? Review it thoroughly."

## What's in this repo

- `deep-dive/` — the skill itself (`SKILL.md` + reference playbooks). This is what you install.
- `deep-dive.skill` — the same folder, zipped, for a one-step download.

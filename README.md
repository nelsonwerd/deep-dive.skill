# deep-dive

Rigorous multi-agent deep-dive analysis — for **Claude** and **OpenAI Codex**. Point it at a codebase, a strategy or decision system, a design, or an open research question and it runs a structured investigation instead of giving a one-shot answer.

## What it does

For a complex, investigative task, the skill runs a multi-phase pipeline:

1. **Parallel specialists** — deploys 4–6 subagents at once, each in its own lane.
2. **Synthesis** — one agent reads every specialist output, cross-checks claims, and resolves contradictions.
3. **Follow-up verification** — single-claim agents re-check load-bearing or single-sourced numbers.
4. **Red-team** — an adversarial reviewer tries to break the conclusions.
5. **Executive briefing** — a plain-English verdict with an honest 1–10 confidence rating.

One honest caveat it states up front: every agent is the *same model*, so the loop catches where independent reasoning **diverges**, not blind spots they all **share** — so it weights externally-checkable evidence (code you can run, `git`, data, cited sources) above claims resting on model judgment, and caps confidence when a conclusion stands only on shared priors.

It scales down for narrow scope (2–3 lanes) and up for broad, multi-domain work (6 lanes). By default it runs in **pure research mode** — it writes markdown findings and changes no code unless you explicitly ask.

Four built-in variants live in `deep-dive/references/`: **codebase audit**, **strategy / system evaluation**, **design evaluation**, and **open-ended research**.

## Quickstart — try this first

> **You type:** "Do a standard design evaluation of *<thing>*. Research-only."
>
> **You get back:** a `research/<topic>/` folder of specialist findings, a synthesis, an adversarial red-team pass, and a plain-English executive briefing with an honest 1–10 confidence rating. No code is touched unless you explicitly ask.

> **Heads up — this skill is token-hungry by design.** A full run fans out 4–6 specialist agents (each writing thousands of words), then synthesis, follow-up verification, a red-team pass, and a briefing — easily 10+ agent calls and tens of thousands of tokens for a single analysis. That's the right trade for a high-stakes call (a ship/no-ship decision, real money on the line), and a great fit on a **Claude Max** plan or any setup where you're not token-constrained. On a smaller plan, reach for it deliberately: use the *Scale heuristics* in `SKILL.md` (2–3 lanes for narrow scope, skip the red-team for low-stakes work), or just ask for a single-pass review instead.

## Install

This is an open **[Agent Skill](https://agentskills.io)** — the same `deep-dive/` folder works in **Claude** and **OpenAI Codex**. Pick your tool:

| You use… | Install it by… |
|---|---|
| **Claude Code** — terminal, the **Code** tab of the Claude desktop app, [claude.ai/code](https://claude.ai/code), or a VS Code / JetBrains IDE | dropping `deep-dive/` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project) |
| **OpenAI Codex** — CLI, app, or IDE | dropping `deep-dive/` into `~/.agents/skills/` (all repos) or `.agents/skills/` (one repo), then restarting Codex |
| **Claude chat** — the **Chat** tab of the desktop app, or [claude.ai](https://claude.ai) | uploading **`deep-dive.skill`** (the zip in this repo) under **Customize → Skills** |
| **Any other agent** | pointing it at `deep-dive/SKILL.md` — it's just instructions |

The `.skill` file is just the `deep-dive/` folder zipped, so one `unzip` drops it into either skills home:

```bash
# Claude Code — detected in-session (verify with /skills):
unzip deep-dive.skill -d ~/.claude/skills/

# OpenAI Codex — the current skills path is ~/.agents/skills/; restart Codex after:
mkdir -p ~/.agents/skills && unzip deep-dive.skill -d ~/.agents/skills/
```

Prefer a clone? `git clone https://github.com/nelsonwerd/deep-dive-skill.git`, then `cp -r deep-dive-skill/deep-dive` into whichever skills folder above.

<sub>Menu names and exact paths shift between versions — the [Claude Skills](https://support.claude.com/en/articles/12512180-use-skills-in-claude) and [Codex Skills](https://developers.openai.com/codex/skills) docs are the source of truth. One caveat specific to deep-dive: its **parallel multi-agent orchestration** is native to Claude Code; in Codex (and other single-agent runtimes) it runs the same lanes **serially** — same method, lower cross-agent independence. The methodology is fully portable.</sub>

## Runtime support

The method runs everywhere; only orchestration degrades:

| | Claude chat | Claude Code | OpenAI Codex | Other agents |
|---|---|---|---|---|
| **deep-dive** | Works (degraded: no repo/file access; serial lanes) | **Best** — parallel subagents + web | Strong — same lanes run **serially** (lower cross-agent independence, so confidence is capped); external claims labeled *unverified* if no web | Works (degraded: serial lanes, local-only) |

See the *Environment & fallbacks* section in `SKILL.md` for the exact fallbacks (no subagents → serial; no web → label external claims unverified; progress tools → skip).

## Use it

- **Manually:** type `/deep-dive` and describe the target.
- **Automatically:** Claude invokes it on its own when a task looks like a thorough audit, rigorous analysis, or comprehensive review.

Examples:

- "Do a deep dive on this codebase before I ship it."
- "Evaluate whether this strategy actually holds up under scrutiny."
- "Is this the right architecture? Review it thoroughly."

## What's in this repo

- `deep-dive/` — the skill itself (`SKILL.md` + reference playbooks). This is what you install.
- `deep-dive.skill` — the same folder, zipped, for a one-step download.

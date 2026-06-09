# deep-dive

Rigorous multi-agent deep-dive analysis — for **Claude** and **OpenAI Codex**. Point it at a codebase, a strategy or decision system, a design, or an open research question and it runs a structured investigation instead of giving a one-shot answer.

## What it does

For a complex, investigative task, the skill runs a multi-phase pipeline:

1. **Parallel specialists** — deploys 4–6 subagents at once, each in its own lane.
2. **Synthesis** — one agent reads every specialist output, cross-checks claims, and resolves contradictions.
3. **Follow-up verification** — single-claim agents re-check load-bearing or single-sourced numbers.
4. **Red-team** — an adversarial reviewer tries to break the conclusions.
5. **Executive briefing** — a plain-English verdict with an honest 1–10 confidence rating.

It scales down for narrow scope (2–3 lanes) and up for broad, multi-domain work (6 lanes). By default it runs in **pure research mode** — it writes markdown findings and changes no code unless you explicitly ask.

Four built-in variants live in `deep-dive/references/`: **codebase audit**, **strategy / system evaluation**, **design evaluation**, and **open-ended research**.

## Quickstart — try this first

> **You type:** "Do a standard design evaluation of *<thing>*. Research-only."
>
> **You get back:** a `research/<topic>/` folder of specialist findings, a synthesis, an adversarial red-team pass, and a plain-English executive briefing with an honest 1–10 confidence rating. No code is touched unless you explicitly ask.

> **Heads up — this skill is token-hungry by design.** A full run fans out 4–6 specialist agents (each writing thousands of words), then synthesis, follow-up verification, a red-team pass, and a briefing — easily 10+ agent calls and tens of thousands of tokens for a single analysis. That's the right trade for a high-stakes call (a ship/no-ship decision, real money on the line), and a great fit on a **Claude Max** plan or any setup where you're not token-constrained. On a smaller plan, reach for it deliberately: use the *Scale heuristics* in `SKILL.md` (2–3 lanes for narrow scope, skip the red-team for low-stakes work), or just ask for a single-pass review instead.

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

## Works in Claude *and* Codex

This follows the open **[Agent Skills](https://agentskills.io) standard**, so the same `SKILL.md` works in **Claude** and **OpenAI Codex**:

| You use… | Add it by… |
|---|---|
| **Claude Code** — terminal, the **Code** tab of the Claude desktop app, [claude.ai/code](https://claude.ai/code), or an IDE | the install above (drop `deep-dive/` in `~/.claude/skills/`) |
| **OpenAI Codex** — CLI, app, or IDE | copy `deep-dive/SKILL.md` (+ `references/`) into `.agents/skills/deep-dive/` (repo) or `~/.agents/skills/deep-dive/` (global) → [Codex skills docs](https://developers.openai.com/codex/skills) |
| **Claude chat** — the **Chat** tab of the desktop app, or [claude.ai](https://claude.ai) | uploading **`deep-dive.skill`** (the zip) under **Customize → Skills** → [using Skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) |
| **Any other agent** | pointing it at `SKILL.md` — it's just instructions |

<sub>Menu names/commands drift between versions — the linked docs are the source of truth. One caveat specific to deep-dive: its **parallel multi-agent orchestration** is native to Claude Code; in Codex it runs as a single-agent, guided version of the same playbook (the method carries; the parallelism doesn't).</sub>

**Runtime support** — the method runs everywhere; only orchestration degrades:

| | Claude chat | Claude Code | OpenAI Codex | Other agents |
|---|---|---|---|---|
| **deep-dive** | Works (degraded: no repo/file access; serial lanes) | **Best** — parallel subagents + web | Strong — same lanes run **serially**, same rigor; external claims labeled *unverified* if no web | Works (degraded: serial lanes, local-only) |

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

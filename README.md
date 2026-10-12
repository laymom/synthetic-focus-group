# /fgi — Synthetic Focus Group Interview Moderator

A Claude Code skill that runs a **qualitative Focus Group Interview (FGI)** on your MVP, design mockup, or policy draft — using *N synthetic personas* simulated in isolated contexts, moderated through a disciplined 5-phase session, and distilled into a thematic report that preserves verbatim Voice-of-Customer quotes.

Unlike a single "act as a user and give feedback" prompt, `/fgi` reproduces the *structure* of real focus group research: balanced moderation, cross-persona reactions, group-effect dynamics, and an explicit honesty guard about what synthetic feedback can and cannot tell you.

> ⚠️ **Honest by design.** Synthetic personas are good for *discovery*, not *validation*. Every report ends with a "validate with 5 real users" checklist. This is a feature, not a disclaimer.

## Why it's different

- **Persona isolation** — each persona runs in its own subagent — a design choice to reduce cross-persona contamination. It does not fix the identity flattening documented by Wang et al. ([*Nature Machine Intelligence*, 2025](https://doi.org/10.1038/s42256-025-00986-z)).
- **Methodology-anchored** — the 5-phase flow, balanced-intervention rules, and thematic analysis are drawn from focus-group research literature (Krueger, *Moderating Focus Groups*, 1998; Focus Agent arXiv 2409.01907), not invented.
- **Group dynamics** — cross-persona probes ("anyone disagree?"), dominant-speaker balancing, and conflict surfacing are built into the moderator loop.
- **Verbatim preservation** — themes are backed by raw persona quotes, never summarized away.

## How it works

```text
/fgi <domain> <stimulus>
   │
   ├─ 1. Recognize domain & stimulus (text or HTML mockup)
   ├─ 2. Recommend N personas (3 representative + 1 edge)   🚪 you approve
   ├─ 3. (optional) Pre-filter the stimulus for obvious weaknesses
   ├─ 4. Dispatch one isolated `fgi-participant` subagent per persona
   ├─ 5. Run the 5-phase session (priming → icebreak → general → stimulus deep-dive → wrap-up)
   ├─ 6. Thematic analysis (utterance → code → sub-theme → theme)
   └─ 7. Report (Executive Summary · Themes w/ verbatim VoC · Recommendations · Limitation guard)
```

### The 5 phases

| Phase | Length | Purpose |
|---|---|---|
| ① Priming | short | rules, purpose, "please be honest" |
| ② Icebreak | short | persona self-intro, natural entry |
| ③ General | medium | domain opinions, *no stimulus yet* |
| ④ Stimulus + deep-dive | long | expose stimulus + 3–5 probes + cross-persona reactions |
| ⑤ Wrap-up | short | summary, live ranking, final messages |

## Research backing

- [Richard A. Krueger, *Moderating Focus Groups* (Focus Group Kit, Vol. 4), SAGE, 1998](https://sk.sagepub.com/book/mono/moderating-focus-groups/toc) — session structure & moderator neutrality
- [Focus Agent (arXiv 2409.01907)](https://arxiv.org/html/2409.01907v1) — balanced intervention in synthetic groups
- [Bisbee et al., Political Analysis 2024](https://doi.org/10.1017/pan.2024.5) · [Wang et al., Nature Machine Intelligence 2025](https://doi.org/10.1038/s42256-025-00986-z) · [Shrestha et al., Behavioral Science & Policy 2025](https://doi.org/10.1177/23794607241311793) — limits of synthetic survey participants

## Installation

Claude Code only (needs subagents).

### 1. Install the skill

Recommended:

```bash
npx skills add laymom/synthetic-focus-group --skill fgi
```

Manual (macOS/Linux):

```bash
git clone https://github.com/laymom/synthetic-focus-group.git ~/.claude/skills/fgi
```

Manual (Windows PowerShell):

```powershell
git clone https://github.com/laymom/synthetic-focus-group.git "$HOME\.claude\skills\fgi"
```

### 2. Install the participant agent (required)

`/fgi` dispatches an isolated subagent named `fgi-participant` (one per persona; it receives one persona's attributes + the stimulus + the current moderator question, and responds in first person as that persona only), and the skill forbids simulating all personas in the main context. `npx skills add` does **not** install this agent — copy `agents/fgi-participant.md` into `~/.claude/agents/` yourself. Without it, `/fgi` cannot run.

macOS/Linux (from the cloned repo or the installed skill folder):

```bash
mkdir -p ~/.claude/agents && cp agents/fgi-participant.md ~/.claude/agents/
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\agents" | Out-Null; Copy-Item agents\fgi-participant.md "$HOME\.claude\agents\"
```

Restart Claude Code so the skill and agent register.

## Usage

```text
/fgi sleep ./mockups/sleep-onboarding.html
/fgi <your-domain> "a one-line description of your concept or mockup"
```

The skill ships with one example persona set in `references/` (`personas-sleep.md`). **Duplicate it and edit** to build a persona set for your own domain — keep the `single / cluster / conflict / edge` group balance.

## The personas

Each example set has 8 personas across 4 group roles:

- **single (S)** — one dominant need (e.g. family-focused, value-focused)
- **cluster (M)** — 2–3 mixed needs
- **conflict (C)** — competing needs (e.g. family SUV *and* driving thrill)
- **edge (E)** — extreme / outlier (e.g. low digital literacy, skeptic)

Each persona carries: age, occupation, lifestyle, expectations, concerns, tone of voice, and **forbidden phrases** (to keep the simulation in character).

## ⚠️ Limitations — read this

Synthetic FGI output is a **discovery-grade hypothesis**, not validated user research. Before any high-stakes decision (shipping a feature, finalizing a policy), validate with:

- 5+ real-user IDIs or a mini-FGI
- a usability test (Nielsen's 5-user rule)
- quantitative metrics (conversion, dwell time)

## License

MIT — see [LICENSE](LICENSE).

## Credits

Built by **Sophia in Korea — a non-engineer who ships agents anyway.** Methodology anchored in the focus-group research cited above; persona examples are fictional and illustrative.

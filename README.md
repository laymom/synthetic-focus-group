# /fgi — Synthetic Focus Group Interview Moderator

A Claude Code skill that runs a **qualitative Focus Group Interview (FGI)** on your MVP, design mockup, or policy draft — using *N synthetic personas* simulated in isolated contexts, moderated through a disciplined 5-phase session, and distilled into a thematic report that preserves verbatim Voice-of-Customer quotes.

Unlike a single "act as a user and give feedback" prompt, `/fgi` reproduces the *structure* of real focus group research: balanced moderation, cross-persona reactions, group-effect dynamics, and an explicit honesty guard about what synthetic feedback can and cannot tell you.

> ⚠️ **Honest by design.** Synthetic personas are good for *discovery*, not *validation*. Every report ends with a "validate with 5 real users" checklist. This is a feature, not a disclaimer.

## Why it's different

- **Persona isolation** — each persona runs as a separate subagent, so they don't collapse into one averaged "AI voice" (a known failure mode of multi-persona prompting; see *Nature Machine Intelligence 2025* on identity flattening).
- **Methodology-anchored** — the 5-phase flow, balanced-intervention rules, and thematic analysis are drawn from focus-group research literature (Krueger 1998, Focus Agent arXiv 2409.01907), not invented.
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

- Krueger & Casey, *Focus Groups* (1998) — session structure & moderator neutrality
- [Focus Agent (arXiv 2409.01907)](https://arxiv.org/html/2409.01907v1) — balanced intervention in synthetic groups
- [Cambridge 2024](https://www.cambridge.org/core/journals/political-analysis/article/synthetic-replacements-for-human-survey-data-the-perils-of-large-language-models/B92267DC26195C7F36E63EA04A47D2FE) · [Nature MI 2025](https://www.nature.com/articles/s42256-025-00986-z) · [LSE 2025](http://eprints.lse.ac.uk/127300/1/shrestha-et-al-2025-beyond-weird-can-synthetic-survey-participants-substitute-for-humans-in-global-policy-research.pdf) — limits of synthetic survey participants

## Installation

Copy into your Claude Code skills directory:

```bash
cp -r fgi ~/.claude/skills/fgi
```

Restart Claude Code so the skill registers.

### Companion subagent

`/fgi` dispatches an isolated subagent named `fgi-participant` (one per persona). You'll want a matching agent definition in `~/.claude/agents/fgi-participant.md` whose job is: *receive one persona's attributes + the stimulus + current moderator question, and respond in first person as that persona only.* A starter definition is included in `agents/` if present; otherwise any single-persona role agent works.

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

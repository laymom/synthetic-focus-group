# Synthetic Focus Group (`/fgi`) for Claude Code

**For product planners and designers who want to explore questions, risky assumptions, and edge cases before real user research: run a simulated 5-phase focus group on your MVP, mockup, or policy draft and get a report where every persona response is quoted verbatim, traceable by ID, and labeled as synthetic.**

## Example

<!-- EXAMPLE: filled in phase 2 -->

## Install

Claude Code only (needs subagents). Two parts: the skill, and the participant agent it dispatches. **Both are required.**

### 1. Install the skill

With the [skills CLI](https://github.com/vercel-labs/skills), from your project folder:

```bash
npx skills add laymom/synthetic-focus-group --skill fgi -a claude-code
```

This copies the whole repo folder (`SKILL.md`, `references/`, `agents/`) into `.claude/skills/fgi/` in the current project. Add `-g` to install for your user instead of one project.

Or clone it by hand.

macOS/Linux:

```bash
git clone https://github.com/laymom/synthetic-focus-group.git ~/.claude/skills/fgi
```

Windows PowerShell:

```powershell
git clone https://github.com/laymom/synthetic-focus-group.git "$HOME\.claude\skills\fgi"
```

### 2. Install the participant agent (required)

`/fgi` dispatches one isolated subagent named `fgi-participant` per persona. Installing the skill puts `agents/fgi-participant.md` inside the skill folder, but Claude Code only registers agents from an `agents/` folder, so copy it there yourself. Without it, the skill is written to stop instead of running.

Project install (after `npx skills add` without `-g`), macOS/Linux:

```bash
mkdir -p .claude/agents && cp .claude/skills/fgi/agents/fgi-participant.md .claude/agents/
```

Project install, Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force ".claude\agents" | Out-Null; Copy-Item ".claude\skills\fgi\agents\fgi-participant.md" ".claude\agents\"
```

User install (`-g` or the clone above), macOS/Linux:

```bash
mkdir -p ~/.claude/agents && cp ~/.claude/skills/fgi/agents/fgi-participant.md ~/.claude/agents/
```

User install, Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\agents" | Out-Null; Copy-Item "$HOME\.claude\skills\fgi\agents\fgi-participant.md" "$HOME\.claude\agents\"
```

### 3. First run

Start a new Claude Code session so the skill and agent register, then type `/fgi`. The first thing it does is propose a persona line-up and wait for your OK.

## Input example

Copy, paste, and replace the description with your own:

```text
/fgi sleep "Onboarding for a sleep app, 3 screens. 1) Pick a goal: fall asleep faster, wake up less, or feel rested. 2) Connect a smartwatch or skip. 3) A 7-day plan with one evening habit per day."
```

Or point it at a file:

```text
/fgi sleep ./mockups/sleep-onboarding.html
```

The session and the report follow the language you write in.

## What happens

1. **Recognize** the domain and the stimulus (text, an HTML file, or both).
2. **Gate**: it recommends 4 personas from `references/personas-<domain>.md` (3 representative + 1 edge) and waits for your approval.
3. **Optional pre-filter**: it scans the stimulus for obvious weaknesses and suggests fixes. This analysis is never shown to the personas.
4. **Session**: one `fgi-participant` subagent per persona, per question, run in parallel. Subagents are stateless, so each call includes that persona's own earlier answers.
5. **Report**: themes with verbatim quotes, utterance IDs, a question to ask real users and the evidence that would reject each theme, plus a full dispatch log. Saved to `fgi-reports/` in the current working directory.

What the personas see, phase by phase:

| Phase | Content | Personas see |
|---|---|---|
| ① Priming | rules, purpose, "please be honest" | the priming text |
| ② Icebreak | self-introduction | priming + their own earlier answers |
| ③ General | opinions about the domain | no stimulus, no other personas' words |
| ④ Stimulus + deep-dive | first impressions, 3–5 probes, cross-reactions | the stimulus; first answers are independent; other personas' words only in cross-reaction rounds |
| ⑤ Wrap-up | summary, ranking, final words | a neutral, unattributed list of points raised |

Every persona utterance gets an ID such as `P2-S4-03` (persona 2, phase ④, third utterance), and themes cite those IDs. If the participant agent is missing, the skill is written to stop rather than pretend to run a multi-agent session, and to continue only if you explicitly ask, as a clearly labeled single-context fallback.

### Evidence labels

Every claim in the report carries one of: User-provided fact · Sourced fact (with link) · Real user research · AI assumption · Synthetic persona response · Approved decision · Open question. Persona answers are always **Synthetic persona response**, even when the personas were built from real data.

## How it differs

- **From a single "act as a user" prompt**: each persona runs in its own subagent, the stimulus is held back until phase ④, first reactions are collected before anyone sees anyone else's, and disagreement is asked for on purpose.
- **From a summary**: quotes stay verbatim with IDs, so you can trace every theme back to what was said and in which phase.
- **From user research**: it is not user research. For each theme it tells you what to ask real people and what result would prove the theme wrong.

## Limits, cost, compatibility

- **Synthetic, not evidence.** The output is a set of hypotheses and questions. It never means validated, purchase intent confirmed, or launch approved. How often a point came up is not customer share, demand, or statistical representativeness.
- **Isolation is a design choice.** Separate subagents reduce personas copying each other. They do not guarantee a persona keeps its identity, and they do not make answers predictive of real users.
- **What research on synthetic participants found:**
  - Bisbee et al. (2024): ChatGPT's persona answers matched survey averages, but had less variation than real respondents and shifted with small prompt-wording changes and over a 3-month period.
  - Shrestha et al. (2025): GPT-4 synthetic participants roughly matched human responses on average, but matched less well outside WEIRD (Western, educated, industrialized, rich, democratic) countries and leaned more positive than humans.
  - Wang et al. (2025): LLMs standing in for identity groups can misportray and flatten them; mitigation techniques reduced but did not remove this.
- **Personas are Korean-context examples.** Only one set ships (`references/personas-sleep.md`, a sleep app, 8 fictional Korean personas). Duplicate and rewrite it for your domain. Build personas around behavior context, current alternatives, and constraints rather than demographics.
- **Cost.** Every moderator question is one subagent call per persona, so a session costs about N personas × number of questions calls, far more tokens than a single prompt.
- **Compatibility.** Claude Code only, because it needs subagents and the `fgi-participant` agent. Other agents may install the skill, but it is not built for them.
- **When to run real research instead**, by goal: usability problems → moderated usability tests; demand → a demand test such as a smoke test or pre-orders; pricing → pricing research; policy → stakeholder or expert review.

## Related tools

Small, independent tools by the same author. Use any of them on its own; none requires the others.

- [multi-agent-brainstorm](https://github.com/laymom/multi-agent-brainstorm) — explore and compare ideas from 5 perspectives
- [synthetic-focus-group](https://github.com/laymom/synthetic-focus-group) — explore reactions and risky assumptions with simulated personas
- [ai-prd-generator](https://github.com/laymom/ai-prd-generator) — turn an idea into a PRD with scope and acceptance criteria
- [consulting-ux-strategy](https://github.com/laymom/consulting-ux-strategy) — audit flows, information architecture, and usability, then prioritize fixes

When passing output between them, keep the evidence labels (User-provided fact, Sourced fact, Real user research, AI assumption, Synthetic persona response, Approved decision, Open question). Simulated persona responses stay labeled as synthetic in every downstream document.

## Sources, license, changelog

The session structure is adapted from focus-group practice. The LLM work is cited as inspiration or as evidence of limits, not as proof that this tool works.

- Krueger, R. A. (1998). *Moderating Focus Groups* (Focus Group Kit, Vol. 4). SAGE. [sk.sagepub.com](https://sk.sagepub.com/book/mono/moderating-focus-groups/toc)
- Zhang, T., Zhang, X., Cools, R., & Simeone, A. L. (2024). Focus Agent: LLM-Powered Virtual Focus Group. *IVA '24*. [arXiv:2409.01907](https://arxiv.org/abs/2409.01907). Inspired the LLM-moderated focus-group format.
- Ashkinaze, J., Fry, E., Edara, N., Gilbert, E., & Budak, C. (2025). Plurals: A System for Guiding LLMs via Simulated Social Ensembles. *CHI '25*. [doi:10.1145/3706598.3713675](https://doi.org/10.1145/3706598.3713675). Inspired persona agents deliberating under a moderator.
- Bisbee, J., Clinton, J. D., Dorff, C., Kenkel, B., & Larson, J. M. (2024). Synthetic Replacements for Human Survey Data? The Perils of Large Language Models. *Political Analysis*, 32(4). [doi:10.1017/pan.2024.5](https://doi.org/10.1017/pan.2024.5)
- Shrestha, P., Krpan, D., Koaik, F., Schnider, R., Sayess, D., & Binbaz, M. S. (2025). Beyond WEIRD: Can synthetic survey participants substitute for humans in global policy research? *Behavioral Science & Policy*. [doi:10.1177/23794607241311793](https://doi.org/10.1177/23794607241311793)
- Wang, A., Morgenstern, J., & Dickerson, J. P. (2025). Large language models that replace human participants can harmfully misportray and flatten identity groups. *Nature Machine Intelligence*, 7. [doi:10.1038/s42256-025-00986-z](https://doi.org/10.1038/s42256-025-00986-z)

MIT, see [LICENSE](LICENSE). Changes are listed in [CHANGELOG.md](CHANGELOG.md). Persona examples are fictional.

Built by **Sophia in Korea — a non-engineer who ships agents anyway.**

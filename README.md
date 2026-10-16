# Synthetic Focus Group (`/fgi`): synthetic users for Claude Code

Try a concept on three synthetic users before you interview real ones.
Each persona runs as a separate subagent and reacts before seeing the others.
The moderator then confronts where they split and hands you 5 questions for real interviews.

## Example: Quick session

A fictional bookstore wants to rent 6 window seats to remote workers. Excerpt from a real run:

> **Where they split**
> 1. **"Calls only outside": P1 and P3 took opposite sides, and held after the probe.** P1-R1 "the 'calls only outside' rule is a dealbreaker for me, because on a typical Tuesday I have at least two scheduled video calls…" vs P3-R1 "the quiet rule about calls outside is the one thing I'd actually trust — that's the specific thing that ruins my current setup". Why: call volume splits "remote workers" into two different groups (AI assumption). After the probe: P1-R2 "my calls are scheduled, mid-morning or mid-afternoon, and they're the core of my workday"; P3-R2 "I have maybe one client check-in call a week, usually asynchronous Slack".
>
> **5 questions to ask real users in an interview** (2 of 5)
> 2. "In the last 2 weeks, how many video or phone calls did you join during work hours, and where were you for each?" — **rejects A1 if:** call counts cluster into clearly different patterns across people with the same job label; "remote workers" is not one audience.
> 3. "The last time you considered a coworking day pass or a café-work morning but didn't go — what stopped you?" — **rejects A2 if:** seat or space uncertainty is already the dominant reason people walk away from similar offers; a 6-seat first-come pass inherits the same blocker rather than solving it.

All quotes are Synthetic persona responses, not customer evidence.
Full output, transcript, and dispatch log: [examples/bookstore-desk-pass-quick.md](examples/bookstore-desk-pass-quick.md).

## Install

Claude Code only. You need the skill and its participant agent.

1. Install the skill from your project folder:

```bash
npx skills add laymom/synthetic-focus-group --skill fgi -a claude-code
```

2. Copy the agent into `.claude/agents/` (macOS/Linux):

```bash
mkdir -p .claude/agents && cp .claude/skills/fgi/agents/fgi-participant.md .claude/agents/
```

On Windows PowerShell, run `New-Item -ItemType Directory -Force .claude\agents | Out-Null; Copy-Item .claude\skills\fgi\agents\fgi-participant.md .claude\agents\` instead.
Installed for your user with `-g`? Use `~/.claude` (or `$HOME\.claude`) in place of `.claude`.

**Check:** start a new session and type `/fgi`. If the agent is missing, it tells you how to fix it.

## What to type

```text
/fgi Quick session on concept.md. Goal: decide whether to pilot it for 4 weeks.
```

```text
/fgi Full session on ./mockups/sleep-onboarding.html
```

Quick is the default. Plain `/fgi <file or description>` runs a Quick session.
Add your goal so the questions aim at your decision.
The session follows the language you write in.

## What you get

| | Quick session (default) | Full session (on request) |
|---|---|---|
| Personas | 3, each with a different current alternative, constraint, and motivation | 4–5, from a persona set or drafted for you |
| Rounds | independent first reactions, then one probe on the split | 5 phases: priming, icebreak, general, stimulus with probe and cross-reaction, wrap-up |
| Participant calls | about 6 | about 30 for 5 personas |
| Output | one screen: where they split and why, 2–3 risky assumptions, 5 interview questions | themed report with verbatim quotes, real-user questions, and a dispatch log |

Both start with one gate: you approve the personas.
Both save a file in `fgi-reports/` with every answer verbatim and a dispatch log.
Every persona utterance has an ID, such as `P2-R1`.
Every claim carries a label: User-provided fact · Sourced fact · Real user research · AI assumption · Synthetic persona response · Approved decision · Open question.

## Why isolated personas

One chat that plays several users tends to give one blended voice.
This tool keeps the personas apart, so differences can show up before anyone adjusts to anyone else.

| Design choice | What it guards against | Source |
|---|---|---|
| Each persona runs in its own subagent | Personas copying each other inside one context | Design choice. It does not fix the identity flattening in Wang et al. (2025) |
| Everyone reacts before seeing anyone else | Early answers shaping later ones | Production-blocking research on human brainstorming groups: Diehl & Stroebe (1987); Nijstad et al. (2003) |
| The moderator shows each side the other's words and asks what would change their mind | False consensus | Focus-group practice of inviting dissent: Krueger (1998); MAXQDA guide (secondary) |
| Personas built on behavior, not demographics | Stereotyped answers from age or job | Wang et al. (2025) on flattened identity groups |
| Questions ask what people did, with the answer that would reject an assumption | Collecting polite "I'd use it" answers | Design choice |

This is design rationale. The cited research is about human groups or about the limits of LLM personas. None of it shows that this tool predicts real users.

## Compared with plain chat

We asked plain Claude Code the same question about the same concept. Both outputs are in [examples/](examples/).

- **Plain chat was broader and faster.** It listed more objections and 16 interview questions in under a minute.
- **Only the Quick session showed a split.** One rule was a dealbreaker for one persona and the main draw for another.
- **Only the Quick session probed the split.** Each persona named the fact behind its view.
- **Only the Quick session tied questions to assumptions.** Each question names the answer that would reject one.

Use plain chat for a checklist of objections. Use `/fgi` to find the question you had not prepared.

## Limits

- **Synthetic, not evidence.** Results are hypotheses and questions. They never mean validated or launch approved.
- **Same model, many voices.** All personas run on one model, so answers can still converge.
- **Counts are not shares.** Three personas agreeing is not "most customers".
- **Research on synthetic participants:**
  - Bisbee et al. (2024): persona answers matched survey averages but varied less than real people. They also shifted with small prompt changes and over three months.
  - Shrestha et al. (2025): synthetic answers roughly matched humans on average. They matched less well outside the US. They also leaned more progressive and financially literate.
  - Wang et al. (2025): LLMs standing in for identity groups can misportray and flatten them. Mitigations reduced this but did not remove it.
- **Run size.** A full session makes about 30 participant calls. The Quick session is shorter.
- **One example persona set.** `references/personas-sleep.md` is a Korean-context sleep app set. Copy it for your own domain.
- **Claude Code only.** It needs subagents and the `fgi-participant` agent.
- **When to use real research instead:** usability tests for usability, a smoke test or pre-orders for demand, pricing research for price.

## Related tools

Small, independent tools by the same author. Each works on its own.

- [multi-agent-brainstorm](https://github.com/laymom/multi-agent-brainstorm): explore and compare ideas from 5 perspectives
- [synthetic-focus-group](https://github.com/laymom/synthetic-focus-group): find splits and risky assumptions with simulated personas
- [ai-prd-generator](https://github.com/laymom/ai-prd-generator): turn an idea into a PRD with scope and acceptance criteria
- [consulting-ux-strategy](https://github.com/laymom/consulting-ux-strategy): audit flows and usability, then prioritize fixes

When you pass output between them, keep the evidence labels. Persona answers stay labeled as synthetic everywhere.

## Sources

- Diehl, M., & Stroebe, W. (1987). Productivity loss in brainstorming groups: Toward the solution of a riddle. *Journal of Personality and Social Psychology, 53*(3), 497–509. [doi:10.1037/0022-3514.53.3.497](https://doi.org/10.1037/0022-3514.53.3.497)
- Nijstad, B. A., Stroebe, W., & Lodewijkx, H. F. M. (2003). Production blocking and idea generation: Does blocking interfere with cognitive processes? *Journal of Experimental Social Psychology, 39*(6), 531–548. [doi:10.1016/S0022-1031(03)00040-4](https://doi.org/10.1016/S0022-1031(03)00040-4)
- Krueger, R. A. (1998). *Moderating Focus Groups* (Focus Group Kit, Vol. 4). SAGE. [sk.sagepub.com](https://sk.sagepub.com/book/mono/moderating-focus-groups/toc)
- MAXQDA. *Focus Group Research: The Complete Guide* (secondary; cites Krueger 1998 and Krueger & Casey 2015). [maxqda.com](https://www.maxqda.com/research-guides/focus-group-research)
- Zhang, T., Zhang, X., Cools, R., & Simeone, A. L. (2024). Focus Agent: LLM-Powered Virtual Focus Group. *IVA '24*. [arXiv:2409.01907](https://arxiv.org/abs/2409.01907). Inspired the LLM-moderated format.
- Ashkinaze, J., Fry, E., Edara, N., Gilbert, E., & Budak, C. (2025). Plurals: A System for Guiding LLMs via Simulated Social Ensembles. *CHI '25*. [doi:10.1145/3706598.3713675](https://doi.org/10.1145/3706598.3713675). Inspired persona agents under a moderator.
- Bisbee, J., Clinton, J. D., Dorff, C., Kenkel, B., & Larson, J. M. (2024). Synthetic Replacements for Human Survey Data? The Perils of Large Language Models. *Political Analysis, 32*(4). [doi:10.1017/pan.2024.5](https://doi.org/10.1017/pan.2024.5)
- Shrestha, P., Krpan, D., Koaik, F., Schnider, R., Sayess, D., & Binbaz, M. S. (2025). Beyond WEIRD: Can synthetic survey participants substitute for humans in global policy research? *Behavioral Science & Policy*. [doi:10.1177/23794607241311793](https://doi.org/10.1177/23794607241311793)
- Wang, A., Morgenstern, J., & Dickerson, J. P. (2025). Large language models that replace human participants can harmfully misportray and flatten identity groups. *Nature Machine Intelligence, 7*. [doi:10.1038/s42256-025-00986-z](https://doi.org/10.1038/s42256-025-00986-z)

More examples: [Quick session](examples/bookstore-desk-pass-quick.md) · [plain chat baseline](examples/bookstore-desk-pass-baseline-plain-chat.md) · [Full session (v1.1)](examples/bookstore-desk-pass-report.md). The bookstore is fictional.

## License and changelog

MIT, see [LICENSE](LICENSE). Changes are in [CHANGELOG.md](CHANGELOG.md). Persona examples are fictional.

Built by **Sophia in Korea — a non-engineer who ships agents anyway.**

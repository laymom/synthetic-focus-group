# Changelog

## 1.2.0 — 2026-10-17

### Added
- **Quick session, now the default.** Three personas built on different current alternatives, constraints, and motivations. Each reacts to the stimulus alone first. The moderator finds where they split and runs one probe round on the split, showing each side the other's words verbatim with IDs. Output fits on one screen: where they split and why (with utterance IDs), 2–3 risky assumptions, and 5 interview questions, each with the answer that would reject an assumption. About 6 participant calls (at most 9)
- **Step 0 install check.** If the `fgi-participant` agent is not available, the skill stops and prints the copy command for macOS/Linux and PowerShell
- `quick-reaction` and `quick-probe` phases in `agents/fgi-participant.md`
- Design rationale with sources: independent first reactions (production-blocking research: Diehl & Stroebe 1987, Nijstad et al. 2003) and inviting dissent (Krueger 1998; MAXQDA guide, secondary). Stated as rationale, not as evidence that the tool predicts users
- Final check questions for the Quick session: persona attributes free of stimulus details, no override of the agent's answer length, IDs on both sides of each split, interview questions about past behavior, and "rejects if" answers that would really reject the assumption
- Examples: `examples/bookstore-desk-pass-quick.md` (real Quick session run) and `examples/bookstore-desk-pass-baseline-plain-chat.md` (same concept in plain chat), with a comparison of what each produced

### Changed
- The 5-phase session is now the **Full session**, run when the user asks for depth. Its steps are unchanged
- README rewritten in short sentences: identity first, Quick session excerpt, a two-step install with a check, what to type, Quick vs Full, why isolated personas (design choice, what it guards against, source), comparison with plain chat, limits
- The previous example is retitled "Full session (v1.1)"
- Removed the old example's run-time figure from the README

### Fixed (found in the first Quick session run)
- The moderator overrode the agent's 1–3 sentence limit and emotion labels in its dispatches. Dispatches now use the agent's input layout and set no length or format
- Persona attributes repeated stimulus details (a price, the run days, the distance to the venue). Each attribute must now be writable without reading the stimulus
- The output added sections beyond the template and ran past one screen. It now has fixed sections, an optional one-line "Where they agreed", and line limits

## 1.1.0 — 2026-10-12

### Added
- Exposure rules and session state: participants see no stimulus before phase ④, never see the moderator's pre-filter analysis, give first reactions independently, and see other personas' remarks only in cross-reaction rounds. Because subagents are stateless, each dispatch includes that persona's own earlier answers
- Utterance IDs (e.g. `P2-S4-03`) cited from themes, quotes, and recommendations
- A transcript appendix in every report: all verbatim responses and a dispatch log (participant, phase, question, what was exposed, output)
- Shared evidence labels (User-provided fact, Sourced fact, Real user research, AI assumption, Synthetic persona response, Approved decision, Open question)
- For every theme: a question to ask real users and the evidence that would reject it
- Real research suggested by goal (usability, demand, pricing, policy)
- If the `fgi-participant` agent is unavailable, the skill stops, or runs a clearly labeled single-context fallback only on request
- Persona-building rule: behavior context, current alternatives, and constraints, not demographics
- `CHANGELOG.md`
- A real example run (`examples/bookstore-desk-pass-report.md`, stimulus in `examples/bookstore-desk-pass-concept.md`): fresh project-level install in Claude Code, 5 simulated personas, 30 participant dispatches, with run notes listing the failures found; excerpt in the README

### Changed
- `SKILL.md`, `agents/fgi-participant.md`, and `references/guide-template.md` are now in English; the session and report follow the user's language. `references/personas-sleep.md` stays a Korean-context example with an English note at the top
- "VoC" / "Voice-of-Customer" renamed to "Synthetic persona responses (verbatim)", with the simulation label kept at the top of the report, in themes, quotes, and the summary
- Report location: `fgi-reports/` in the current working directory
- README reordered: one-line summary, example, install, input example, what happens, how it differs, limits, related tools, sources
- `SKILL.md` frontmatter: removed `tools` (not a skill field; it belongs to agent files) and `model: sonnet` (it would switch the user's model for the turn); added `argument-hint`
- Persona isolation described as a design choice, with no claim that it preserves identity or predicts real users
- Focus Agent and Plurals cited as inspiration with full author lists and links

### Removed
- The universal "validate with 5 real users" rule
- Unverifiable internal evidence: "deep-research 13건", "오픈서베이 표준", "권위 출처 기반", unlinked PDMA/GoTranscript references, and the Miro and Pickply links
- Car-domain persona examples (family SUV / driving thrill) from the README and `SKILL.md`; only the sleep set ships

### Fixed (in dd2bbd4)
- Install commands, and the required step to copy the `fgi-participant` agent
- Citation: Krueger (1998), *Moderating Focus Groups*
- Wang et al. (2025) wording: isolation does not fix the identity flattening the paper reports
- Removed the "검증해줘" (validate) trigger
- DOI labels for Bisbee et al. and Shrestha et al.

### Fixed
- The general phase was described as stimulus-free, but the first participant dispatch already included the stimulus
- Moderator rule "ask Why three times" no longer attributed to Krueger; reworded to ask for reasons without a bare "Why?"

### Fixed (found in a real run)
- The 54 KB report was generated in one message and timed out after about 64 minutes of API retries; the report is now written section by section
- Persona attributes carried stimulus details and concept concerns, and a name a persona had invented; attributes now describe the person only, and phase ①–③ questions must not steer toward the concept
- Prior utterances were passed as condensed quotes or summaries while the log said "full"; they are now passed verbatim with IDs every time, and the log records "verbatim" or "summary"
- Quotes labeled verbatim silently dropped words, and Appendix A pointed back to the themes; cuts now use "…" and Appendix A lists every utterance in full
- Cost: one probe per round, one cross-reaction round per session, at most 6 rounds (about 30 participant calls for 5 personas)
- Participant answers ran 4–6 sentences; the agent now enforces 1–3
- New "Final check" on the saved report: simulation labels, quotes against Appendix A, no "4 of 5" counts, real-user question and rejection evidence per theme, accurate dispatch log

## 1.0.0 — 2026-06-14

- Initial release: `/fgi` skill, `fgi-participant` agent, discussion guide template, and the sleep-app persona set

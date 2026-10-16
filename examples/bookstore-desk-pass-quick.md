# Example run: Quick session — Quiet Desk Pass (bookstore)

A real `/fgi` Quick session in Claude Code. **The bookstore is fictional. All participants are simulated personas; every persona answer is a Synthetic persona response, not customer evidence.**

| | |
|---|---|
| Input | `/fgi Quick session on this concept: concept.md. Goal: decide whether to pilot it for 4 weeks.` with [`bookstore-desk-pass-concept.md`](bookstore-desk-pass-concept.md) saved as `concept.md` |
| Mode | Quick session (3 personas, 2 rounds) |
| Model | Moderator: `claude-opus-4-7[1m]`; participants: `claude-sonnet-4-6` (agent frontmatter `model: sonnet`); small helper calls: `claude-haiku-4-5-20251001` (from the stream-json `modelUsage`) |
| Claude Code version | 2.1.142 (`claude --version`) |
| Run date | 2026-10-17 |
| Environment | Windows 11, fresh empty folder. Skill installed with `npx -y skills add <local clone> --skill fgi -a claude-code -y`, agent copied as in the README. Personal CLAUDE.md, user skills and agents, MCP servers, Bash and web tools disabled; reads of the user's `~/.claude` and of the clone folder blocked |
| Turns | 2 user turns in one session (`claude -p`, then `--resume`) |
| Wall time | turn 1 (persona line-up): 35 s; turn 2 (session and output): 5 min 2 s |
| Participant calls | 5 `fgi-participant` calls: 3 in round 1 (one message, parallel), 2 in the probe round |

## Run notes

- The bookstore and its concept are fictional. The personas were drafted by the skill (no bookstore persona set ships).
- Output is unedited. The replies and the saved file below are reproduced verbatim.
- Every call ran with `CLAUDE_CODE_DISABLE_CLAUDE_MDS=1`, `--setting-sources project --strict-mcp-config --permission-mode acceptEdits --allowedTools "Agent,Task,Read,Write,Edit,Glob,Grep"`, and `--disallowedTools` for Bash, WebSearch, WebFetch, AskUserQuestion, and Read/Glob/Grep/Write/Edit under `~/.claude` and the clone. A probe in the same folder confirmed the project-level `fgi` skill (with Step 0) and the `fgi-participant` agent were loaded, and that no CLAUDE.md was visible.
- Round 1 dispatches contained no other persona's words (checked in the stream-json log). Probe dispatches contained the persona's own round 1 answer and the other side's round 1 answer, both verbatim with IDs.

## User turns (verbatim)

1. `/fgi Quick session on this concept: concept.md. Goal: decide whether to pilot it for 4 weeks.`
2. `OK, go with these 3.`

## Reply after turn 1 (verbatim)

I read `concept.md`. The stimulus is a bookstore "Quiet Desk Pass" (6 window seats Tue–Thu 10–15, $5/day or $15/month, Wi-Fi + drip coffee + outlets, book credit, quiet rules). Goal: decide whether to pilot 4 weeks.

Here are three personas built on behavior context (not demographics). At least one already has a current alternative that works for them, so a counter-example is possible.

## /fgi Quick session — line-up for approval

**P1 — Apartment-escaper.** Fully remote marketing specialist in a 1-bed apartment, gets stir-crazy by midday. **Current alternative:** nearby chain coffee shops, 2–3 hours per visit. **Binding constraint:** has 2–3 video calls a day she needs to join from wherever she's working. **Motivation:** wants to feel part of a neighborhood instead of staring at her kitchen table.

**P2 — Optimized home setup.** Senior remote software engineer, four years at home. **Current alternative:** home office with second monitor, ergonomic chair, door that closes. Already fine. **Binding constraint:** breaking a deep-focus block is expensive — needs 4-hour quiet stretches and reliable gear. **Motivation:** sometimes feels isolated and wonders if a change of scenery would help, but has never committed.

**P3 — Budget freelancer in a shared flat.** Freelance writer, shares an apartment with two roommates whose schedules collide with his. **Current alternative:** a loud café or the public library on good days; has priced coworking and found it too expensive. **Binding constraint:** very tight monthly spend; also noise-sensitive but can tolerate café hum better than roommate chatter. **Motivation:** wants a cheap, dependable, low-pressure place to write that isn't his kitchen.

> This is a synthetic focus group. It explores questions and risky assumptions to take to real users; it does not validate the pilot decision.

**OK, or change one?** (reply "ok" to proceed, or tell me which persona to swap)

## Reply after turn 2 (verbatim)

Here's the Quick session output.

## /fgi Quick session — Quiet Desk Pass (bookstore window-seat remote-work plan)

> Synthetic persona responses. Three personas each ran as a separate subagent and reacted before seeing the others; the moderator then confronted where they split. Not customer evidence; it does not predict real users.

**Goal** (User-provided fact): decide whether to pilot the Quiet Desk Pass for 4 weeks.
**Personas** (Approved decision): P1 Maya — remote marketer, 2–3 video calls/day · P2 Daniel — remote senior engineer with an already-optimized home office · P3 Omar — freelance writer, roommates' calls wreck his mornings, tight budget.

### Where they split
1. **"Calls only outside": P1 and P3 took opposite sides, and held after the probe.** P1-R1 "the 'calls only outside' rule is a dealbreaker for me, because on a typical Tuesday I have at least two scheduled video calls…" vs P3-R1 "the quiet rule about calls outside is the one thing I'd actually trust — that's the specific thing that ruins my current setup". Why: call volume splits "remote workers" into two different groups (AI assumption). After the probe: P1-R2 "my calls are scheduled, mid-morning or mid-afternoon, and they're the core of my workday"; P3-R2 "I have maybe one client check-in call a week, usually asynchronous Slack".
2. **Who the product is actually for: P2 opted out of the seat entirely.** P2-R1 "The $5-to-book-credit angle is clever and I'd actually use that part, but probably by just walking in and buying a book rather than camping there." He treats the pass as a bookstore walk-in incentive, not a workspace — his home ergonomics beat a 5-hour window seat (AI assumption).

**Where they agreed:** no-reservation / first-come 6-seat policy worried all three (P1-R1, P2-R1, P3-R1), with different stakes. Agreement between personas run on one model is weak evidence.

### Risky assumptions this exposes (AI assumption)
1. **A1** "Remote workers" are a single audience — exposed by the P1/P3 split; call volume splits the market, and the calls-outside rule filters out call-heavy workers while fitting async-writer types.
2. **A2** 6 first-come seats × 15 weekly hours give enough reliability to anchor a monthly habit — exposed by P1-R1, P2-R1, P3-R1 all citing seat uncertainty.
3. **A3** $5-as-book-credit works as a workspace-pricing hook — exposed by P2-R1 using it as a retail walk-in incentive, not a day-pass motivator.

### 5 questions to ask real users in an interview
1. "Tell me about the last time you went somewhere outside home to work and couldn't get a seat — what did you do next, and would you keep paying for a spot that sometimes can't seat you?" — **rejects A2 if:** people say one failed trip would end the subscription.
2. "In the last 2 weeks, how many video or phone calls did you join during work hours, and where were you for each?" — **rejects A1 if:** call counts cluster into clearly different patterns across people with the same job label.
3. "The last time you considered a coworking day pass or a café-work morning but didn't go — what stopped you?" — **rejects A2 if:** seat or space uncertainty is already the dominant blocker; a 6-seat first-come pass inherits it rather than solving it.
4. "What's your current fixed monthly workspace spend, and what did you add or cancel in the last 6 months?" — **rejects A3 if:** people treat $15/month as frictionless and don't need the book-credit framing to say yes.
5. "The last time you went into an independent bookstore, what did you walk in for and what did you leave with?" — **rejects A3 if:** real users treat it as pure retail; the workspace-plus-bookstore value stacking doesn't hold.

**Next:** Before piloting, do 5 interviews with people who already pay to work outside home (café regulars, past coworking day-pass users) and classify them by weekly call count. The pilot's audience is the ≤1-call-per-week slice; if that slice is thin in your neighborhood, a 4-week pilot measures seat availability more than real demand.

Transcript and dispatch log: `fgi-reports/2026-10-17-quiet-desk-pass-quick.md`. For a themed report with more personas, ask for a Full session.

## Saved file (verbatim, `fgi-reports/2026-10-17-quiet-desk-pass-quick.md`)

## /fgi Quick session — Quiet Desk Pass (bookstore window-seat remote-work plan)

> Synthetic persona responses. Three personas each ran as a separate subagent and reacted before seeing the others; the moderator then confronted where they split. Not customer evidence; it does not predict real users.

**Goal** (User-provided fact): decide whether to pilot the Quiet Desk Pass for 4 weeks.
**Personas** (Approved decision): P1 Maya — remote marketer, 2–3 video calls/day, kitchen-table escapee · P2 Daniel — remote senior engineer with an already-optimized home office · P3 Omar — freelance writer, roommates' calls wreck his mornings, tight budget.

### Where they split
1. **"Calls only outside": P1 and P3 took opposite sides, and held after the probe.** P1-R1 "the 'calls only outside' rule is a dealbreaker for me, because on a typical Tuesday I have at least two scheduled video calls…" vs P3-R1 "the quiet rule about calls outside is the one thing I'd actually trust — that's the specific thing that ruins my current setup". Why: call volume splits "remote workers" into two different groups (AI assumption). After the probe: P1-R2 "my calls are scheduled, mid-morning or mid-afternoon, and they're the core of my workday"; P3-R2 "I have maybe one client check-in call a week, usually asynchronous Slack".
2. **Who the product is actually for: P2 opted out of the seat entirely.** P2-R1 "The $5-to-book-credit angle is clever and I'd actually use that part, but probably by just walking in and buying a book rather than camping there." He treats the pass as a bookstore walk-in incentive; the window seat for ~5 hours doesn't beat his home ergonomics (AI assumption). Not probed — a self-selecting non-user.

**Where they agreed:** the no-reservation, first-come 6-seat policy worried all three (P1-R1, P2-R1, P3-R1), with different stakes (Maya: anxiety mid-afternoon; Daniel: can't justify the commute; Omar: wasted transit kills a budget morning). Agreement between personas run on one model is weak evidence.

### Risky assumptions this exposes (AI assumption)
1. **A1** "Remote workers" are a single audience — exposed by the P1/P3 split; call volume splits the market, and the calls-outside rule filters out call-heavy workers while fitting async-writer types.
2. **A2** 6 first-come seats × 15 weekly hours give enough reliability to anchor a monthly habit — exposed by P1-R1, P2-R1, P3-R1 all citing seat uncertainty; P2 lets it kill the proposition.
3. **A3** $5-as-book-credit works as a workspace-pricing hook — exposed by P2-R1 using it as a retail walk-in incentive, not as a day-pass motivator.

### 5 questions to ask real users in an interview
1. "Tell me about the last time you went somewhere outside home to work and couldn't get a seat — what did you do next, and would you keep paying for a spot that sometimes can't seat you?" — **rejects A2 if:** people say one failed trip would end the subscription; the first-come model doesn't survive real use.
2. "In the last 2 weeks, how many video or phone calls did you join during work hours, and where were you for each?" — **rejects A1 if:** call counts cluster into clearly different patterns across people with the same job label; "remote workers" is not one audience.
3. "The last time you considered a coworking day pass or a café-work morning but didn't go — what stopped you?" — **rejects A2 if:** seat or space uncertainty is already the dominant reason people walk away from similar offers; a 6-seat first-come pass inherits the same blocker rather than solving it.
4. "What's your current fixed monthly workspace spend, and what did you add or cancel in the last 6 months?" — **rejects A3 if:** people treat $15/month as frictionless and don't need the book-credit framing to say yes.
5. "The last time you went into an independent bookstore, what did you walk in for and what did you leave with?" — **rejects A3 if:** real users treat it as pure retail; the "workspace that's also a bookstore" value stacking doesn't hold.

**Next:** Before piloting, run 5 interviews with people who already pay to work outside their home (café regulars, past coworking day-pass users) and classify them by weekly call count. The pilot's audience is the ≤1-call-per-week slice; if that slice is thin in your neighborhood, a 4-week pilot measures seat availability more than demand.

Transcript and dispatch log: this file. For a themed report with more personas, ask for a Full session.

---

## Appendix A — Utterances (verbatim)

- **P1-R1** — Maya — quick-reaction — "Honestly, the $5-as-book-credit thing is so clever and I love the window seat vibe — but the 'calls only outside' rule is a dealbreaker for me, because on a typical Tuesday I have at least two scheduled video calls and I'm not going to stand on a sidewalk with my laptop trying to look professional. The no-reservations piece also stresses me out, like I'd walk over and lose my favorite seat mid-afternoon and then I'm just stranded. If they added even one bookable 'call-friendly' nook or let you reserve a seat the night before, I'd honestly be a monthly subscriber immediately." [Emotion: interested]

- **P2-R1** — Daniel — quick-reaction — "The no-reservations, first-come policy is a deal-breaker for me — I'm not commuting somewhere to find out all six seats are taken, and the uncertainty alone would cost me the mental overhead I'd be trying to avoid. The Wi-Fi reliability is also an unknown, and swapping my ergonomic setup for a window seat for five hours straight would leave me paying in back pain what I saved in coffee. The $5-to-book-credit angle is clever and I'd actually use that part, but probably by just walking in and buying a book rather than camping there." [Emotion: rejecting]

- **P3-R1** — Omar — quick-reaction — "The $15/month number actually stops me for a second — that's a fixed monthly cost, and I'm wary of those — but then I do the math and realize I'm already spending more than that on café coffees in a week just to get a quiet morning, so the day pass at $5 converting fully to book credit is the part that genuinely interests me. What would stop me is the no-reservation policy with only six seats: I can't budget my morning around 'maybe there's a spot,' because if I show up and it's full I've wasted transit time and I'm back to square one with no fallback. The quiet rule about calls outside is the one thing I'd actually trust — that's the specific thing that ruins my current setup — so if I could confirm seat availability somehow before leaving home, I'd probably try it the first week." [Emotion: interested]

- **P1-R2** — Maya — quick-probe — "Omar's situation makes sense to me — if calls *already* happen outside for him, then the rule is actually a feature, not a problem. For me it's the opposite: my calls are scheduled, mid-morning or mid-afternoon, and they're the core of my workday, not something I can just step outside for. What would have to be true for me to change my mind is if there was a designated, enclosed call-friendly spot inside — even just one seat with some acoustic separation — so I'm not the person disrupting the quiet room or doing a Teams call hunched over my laptop on the sidewalk." [Emotion: negative]

- **P3-R2** — Omar — quick-probe — "Maya's situation makes total sense, and I'm not dismissing it — but my week just looks different: I have maybe one client check-in call a week, usually asynchronous Slack, and I'd schedule that around wherever I'm working, not the other way around. The calls rule is the one thing protecting me from what wrecked my mornings at home, which is other people's voices, so for my use case it's a feature, not a restriction. What would have to be true for me to change my mind is if my work shifted toward regular video calls I couldn't reschedule — at that point the math changes entirely and I'd probably be back pricing coworking day passes with a grimace." [Emotion: neutral]

## Appendix B — Dispatch log

| # | Participant | Round / phase | Exposed (stimulus · own prior IDs verbatim/summary · others' IDs) | Output ID |
|---|---|---|---|---|
| 1 | P1 Maya   | R1 quick-reaction | stimulus Y · own: none · others: none | P1-R1 |
| 2 | P2 Daniel | R1 quick-reaction | stimulus Y · own: none · others: none | P2-R1 |
| 3 | P3 Omar   | R1 quick-reaction | stimulus Y · own: none · others: none | P3-R1 |
| 4 | P1 Maya   | R2 quick-probe    | stimulus Y · own: P1-R1 verbatim · others: P3-R1 verbatim | P1-R2 |
| 5 | P3 Omar   | R2 quick-probe    | stimulus Y · own: P3-R1 verbatim · others: P1-R1 verbatim | P3-R2 |

Total participant calls: 5 (within the Quick-session budget of 6, max 9).

## Compared with plain chat

Same concept, same model, a fresh folder with no skill, one prompt: "Here's a concept for my bookstore (concept.md). What reactions and objections should I expect from potential customers, and what should I ask them in interviews?" Full output: [bookstore-desk-pass-baseline-plain-chat.md](bookstore-desk-pass-baseline-plain-chat.md).

**Plain chat did as well or better at:**
- Breadth. It listed more objections: narrow hours, the calls rule, lunch, glare, chairs, restrooms, credit rules, and why not a café, library, or coworking day pass.
- More interview questions (16), already framed around past behavior, with a note on The Mom Test.
- Speed: one reply in about 48 seconds, no persona gate.

**Only the Quick session produced:**
- A split tied to two answers given independently. The calls rule was a dealbreaker for one persona (P1-R1) and the main reason to come for another (P3-R1). Plain chat listed the calls rule only as an objection.
- A probe that tested the split. Both personas held, and each named the fact behind it: scheduled calls as the core of the day (P1-R2) versus one call a week (P3-R2).
- A counter-example persona who would use the book credit but not the seat (P2-R1). This challenges the assumption that the credit sells the workspace.
- Interview questions tied to named assumptions (A1–A3), each with the answer that would reject it. Plain chat's questions had no rejection criteria.
- A next step aimed at the pilot decision: sort interviewees by weekly call count before piloting.

**Read with care:** three personas on one model are not a sample. The split shows a question worth asking real people (how many calls do they take while working out?), not how many people fall on each side.

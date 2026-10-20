# As Agreed: notes for the build agent

As Agreed is an entry to the Airwallex Agentic Banking Hackathon (HackerEarth, online), starter kits
1 and 3. Before any supplier payment moves, it checks every invoice line against what was agreed
(purchase order or contract, the verified beneficiary record, the payment policy in code) and gives
it one of four states: matched, conflict, proposal, uncertain conversion. MIT. Repository
`github.com/Sunrisesoftware-app/as-agreed` (public). All data is synthetic; the Airwallex sandbox
only.

The shared practice of every entry (golden rules, working practice, git, the public demo, the
submission, session start and end) is `E:\projects\sunrisesoftware\hackathons\CLAUDE.md`, which
Claude Code reads for this folder too. This file holds only this entry's facts and overrides the
shared one where the two differ.

**Deadlines (Helsinki time):** the idea by **Fri 23.10.2026 21:29** (a shortlisting gate; target Thu
22.10.); the build phase 25.10. 06:30 to **Fri 13.11.2026 20:29** for shortlisted teams (target Thu
12.11.). Read from the HackerEarth dashboard on 19.10.2026; re-read before relying on them.

**No product code before 25.10.2026 06:30**, until the organisers publish a rule on prior work
(BACKLOG RULES-2). Documents, the data specification and the pre-registration are allowed.

## Where the truth is

| What | Where |
|---|---|
| Product: for whom, what, what never | [`docs/product.md`](docs/product.md) |
| Architecture as prose (as designed until built) | [`docs/architecture.md`](docs/architecture.md) |
| Decisions asked as boxes, and their answers | [`docs/DECISIONS.md`](docs/DECISIONS.md): the file is the truth, the conversation is not |
| State and the next session | [`docs/BUILD_INFO.json`](docs/BUILD_INFO.json): `state` and `next_session` |
| Open task | [`docs/BACKLOG.md`](docs/BACKLOG.md): the **only** list, including the owner's own actions |
| Order and gates up to the deadline | [`docs/roadmap.md`](docs/roadmap.md) |
| What was done and measured | [`docs/CHANGELOG.md`](docs/CHANGELOG.md), newest first |
| Lesson paid for | [`docs/LESSONS.md`](docs/LESSONS.md) |
| The concept on one page | [`docs/concept/`](docs/concept/): `concept.html` is the source of the PDF |
| Texts for HackerEarth | [`docs/submission/texts.md`](docs/submission/texts.md), exactly as submitted |
| Architecture decisions | the Atlas model once the system exists (BACKLOG ATLAS-1), rendered into [`docs/adr/`](docs/adr/README.md) |
| Source documents in Atlas (not the truth) | [`docs/history/`](docs/history/README.md) |

## This entry's rules

- **The boundary.** It verifies payment instructions against what was agreed. It never judges
  whether a purchase was wise and never gives financial or investment advice; keep "recommends",
  "optimises your cash" and the like out of code, copy and the pitch.
- **Money logic in code.** Amounts are in major units. A new UUID `request_id` per operation, reused
  only on a retry of the same operation. A failed transfer ends `CANCELLED`; read `failure_type`.
  Simulation calls sit behind one interface. A fired rule stays a halt.
- **Keys.** The Airwallex sandbox Client ID and API key live in `.env` only, go only to Airwallex's
  sandbox URLs and are never printed; the secret scan refuses `AWX_*` values in git.

## Rendering the concept PDF

```bash
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" --headless --disable-gpu --no-pdf-header-footer --print-to-pdf="$PWD/docs/concept/as-agreed-concept.pdf" "file:///$PWD/docs/concept/concept.html"
```

Check that it is one page before committing.

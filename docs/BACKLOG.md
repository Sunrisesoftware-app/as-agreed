# As Agreed BACKLOG

**The only list of open tasks**, including the owner's own actions.
[`BUILD_INFO.json`](BUILD_INFO.json) tells the **state**, [`CHANGELOG.md`](CHANGELOG.md) **what was
done**, [`roadmap.md`](roadmap.md) **the order and the gates**, [`DECISIONS.md`](DECISIONS.md)
**the decisions**.

**Rules:** a finished row is **removed** here and recorded in the CHANGELOG in the same session;
before listing a row as open, verify it is not already done (an uncertain row is marked *check*).

Codes: **P** the owner's own actions · **SUB** the submissions · **RULES** the organisers' rules ·
**PRE** the pre-build window (design only) · **CI** the pipeline · **ATLAS** the model.

Updated 22.10.2026 (session 3: the idea submitted).

## P: the owner's own actions

- **P-4** *check* Confirm the Airwallex sandbox account exists and its Client ID and API key are in `.env`
  only (typed in through a prompt, never into the chat). A second sandbox business must be named
  "New Business Sandbox" or "Sandbox Business" to be approved automatically. Planned for the evening
  of 20.10.2026.

## SUB: the submissions

- **SUB-0** The shortlist result for the idea phase, due by the build phase's opening on Sun 25.10.
  06:30; read it on the HackerEarth dashboard ("Your Progress").
- **SUB-2** The build submission by Thu 12.11.: working demo, video under 5 minutes, this repository
  with setup instructions.

## RULES: the organisers' rules (unpublished on 20.10.2026)

- **RULES-1** The judging criteria and weights: several participants asked in the HackerEarth
  discussion; no answer. Re-read before the idea is submitted and when the build phase opens.
- **RULES-2** Prior work and open-source reuse: no rule; decides DECISIONS row 6.
- **RULES-3** Workshops, mentorship and the Slack link: "coming soon".
- **RULES-4** How the Claude, AWS and Base44 credits are claimed: "coming soon"; affects row 5.
- **RULES-5** An admin wrote on 21.10.2026: "Please wait until the shortlisted teams are officially
  notified before starting preparations for the Build Day." Read as guidance for the PRE rows: the
  documents may wait for the shortlist; nothing that becomes product code starts before it.

## PRE: the pre-build window, design only (until 25.10. 06:30)

- **PRE-1** The synthetic data specification as a document (four suppliers over US USD LOCAL, GB GBP
  LOCAL and DE EUR SWIFT; purchase orders and one contract; six invoices with one duplicate, one line
  not on the order, one amount outside tolerance after FX and fees; one bank-details email; the policy
  file). The owner approves it.
- **PRE-2** The pre-registration (`AS-AGREED-EVAL-ENNAKKO` in Atlas) with the owner's thresholds,
  locked before the first evaluation run.
- **PRE-3** Decision boxes for DECISIONS rows 5, 6 and 11 as one round.
- **PRE-4** Read the endpoints of the architecture through the Airwallex docs MCP.

## CI

- **CI-1** With the first code: `uv sync --frozen`, `ruff` and `pytest` in `.github/workflows/ci.yml`
  (no `continue-on-error`).

## ATLAS

- **ATLAS-1** An Atlas system `as-agreed` of its own, written as a model file and merged by PR; then
  `docs/adr/` rendered from it.

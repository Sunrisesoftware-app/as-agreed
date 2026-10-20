# As Agreed

An agent that pays supplier invoices only as far as they match what was agreed.

> Status: **concept**. An entry to the [Airwallex Agentic Banking Hackathon](https://airwallex.hackerearth.com/) (HackerEarth, online). The build phase opens on 25 October 2026 and no code is written before it; until then this repository holds the concept and the design documents. All data in it is synthetic, and it runs against the Airwallex sandbox only.

**Concept on one page:** [`docs/concept/as-agreed-concept.pdf`](docs/concept/as-agreed-concept.pdf)

## What it does

A finance team asks the agent to pay this week's supplier invoices in several currencies. Before any money moves, the agent checks every invoice line against what was agreed:

- **what was agreed:** the purchase orders and contracts
- **who may be paid:** the verified beneficiary record
- **the payment policy, in code:** the autonomous limit, the duplicate lock, the reserve floor and the FX tolerance

Every line has exactly one state:

| State | When | What happens |
|---|---|---|
| matched | an agreed line supports it, no rule blocks it, the beneficiary is the verified one | paid through Airwallex within the autonomous limit |
| conflict | a rule blocks it: duplicate, changed beneficiary, over the limit, below the reserve floor | the payment halts; a person decides |
| proposal | nothing that was agreed supports it | shown to the approver; never paid without approval |
| uncertain conversion | after FX and fees the payable amount drifts beyond tolerance | goes to the approver |

Precedence: conflict > proposal > uncertain conversion > matched. A halt a rule fired stays a halt, whatever a model says later.

It does **not** judge whether a purchase was wise, give financial advice, or pay a line a person has not approved unless it is matched and within the limit.

## How it will work

- **Code** holds the amounts, limits, the reserve floor, duplicate protection, the state transitions and every Airwallex call (typed tools, a new request id per operation).
- **Models** read invoices, contracts and emails, match lines and explain exceptions. A second model from a different family tries to refute every match.
- **A person** decides every conflict, proposal and uncertain conversion, and each approval is bound to the amount, currency, beneficiary and the evidence shown.

Airwallex calls (sandbox): balances, FX rates, quotes and conversions; beneficiary schema, validation and creation; transfers with simulated status transitions; the payout webhooks for reconciliation. Starter kits 1 (Adaptive Treasury Controller) and 3 (Payment Ops Incident Commander), with reconciliation as the differentiator.

## Measured, not claimed

Three invariants are tested in code, and one violation fails the build: no double payment, no payment to an unverified beneficiary, nothing paid below the reserve floor. A pre-registered error-injection run reports recall per error type and false alarms on clean lines, with the thresholds locked before the first run and the results reported as they fall.

## Previous work

The same verification principle was first built for physiotherapy home programs: [As Prescribed](https://github.com/Sunrisesoftware-app/as-prescribed) (AMD Developer Hackathon ACT III), with its pre-registered evaluation published as it fell.

## Documents

[`docs/product.md`](docs/product.md) (for whom, what, what never) · [`docs/architecture.md`](docs/architecture.md) · [`docs/DECISIONS.md`](docs/DECISIONS.md) · [`docs/roadmap.md`](docs/roadmap.md) · [`docs/BACKLOG.md`](docs/BACKLOG.md) · [`docs/CHANGELOG.md`](docs/CHANGELOG.md)

## License

MIT. Sunrise Software, Tampere, Finland.

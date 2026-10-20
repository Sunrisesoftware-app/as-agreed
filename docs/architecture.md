# As Agreed: architecture

**As designed, not built** (20.10.2026). The build phase opens on 25.10.2026; this document is
corrected as the code lands, and a decided change that alters the architecture enters the Atlas
model as an ADR. The stack and the models are open decisions (DECISIONS rows 5 and 6).

## 1. The parts

| Part | Does | Decides with |
|---|---|---|
| Intake | reads invoices, purchase orders, contracts and emails into typed lines | a generator model; the output is a typed record, never free text |
| Matcher | proposes, per invoice line, the agreed line that supports it, with the evidence span | the generator |
| Falsifier | tries to refute each proposed match | a model from a different family than the generator |
| Policy engine | the autonomous limit, the duplicate lock, the reserve floor, the beneficiary match, the FX tolerance | code only; a fired rule is sticky |
| State assigner | one state per line by precedence: conflict > proposal > uncertain conversion > matched | code only |
| Airwallex adapter | one function per REST call, a new UUID `request_id` per operation and the same one only on a retry, token refresh before the 30-minute expiry | code only |
| Simulation | sandbox-only calls (deposits, transfer transitions) behind one interface, replaceable without touching the decision logic | code only |
| Webhook receiver | `payout.transfer.paid`, `failed`, `cancelled`: HMAC-SHA256 over the raw bytes, deduplicated on the event id, answered fast | code only |
| Ledger and reconciliation | every paid line traced to its invoice, agreement and approval; the fate of every transfer, including a replaced one | code only |
| Approver desk | the lines the agent may not pay alone, each with its evidence; an approval bound to amount, currency, beneficiary and the evidence shown | a person |
| Evaluation | the three invariants as tests; the pre-registered error-injection run | code, sealed before run 1 |

## 2. The flow

1. Read balances and FX rates; read the batch (invoices, agreements, the verified beneficiary
   record, the policy).
2. Intake and matching; the falsifier's rounds.
3. The policy engine and the state assigner give every line its state.
4. Matched lines within the limit: quote, convert where needed (one quote per conversion), create the
   transfer. Every other line goes to the approver desk.
5. New information re-runs only the affected lines: an email about bank details makes that
   supplier's lines a conflict; a `CANCELLED` transfer is read for its `failure_type`, and a
   replacement goes out under the duplicate lock with a new `request_id`.
6. Webhooks and `GET /transfers` close the ledger; the reconciliation report is produced from it.

## 3. Airwallex facts the design depends on (builder guide, read 19.10.2026)

- Amounts are in major units. A failed transfer ends `CANCELLED`, never `FAILED`; read
  `failure_type`. A failure type applies only after the transfer reaches `SENT`.
- A SWIFT payout costs a flat EUR 12.85 from the same wallet; a local payout is free. FX quotes are
  single-use; FX calls omit `x-api-version`; conversions are REST only.
- Bank details are validated when the beneficiary is created, so a payment failure is staged as
  `SENT` then `FAILED`.
- Rate limits: 20 requests per second globally, 10 per endpoint, 10 concurrent.

## 4. Where it runs

Undecided until the build phase (DECISIONS row 5). The pattern from As Prescribed is a FastAPI
service behind a Cloudflare Worker, with a recorded run served when a live run misses its time
budget, labelled as such.

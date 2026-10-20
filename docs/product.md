# As Agreed: product

What it is, for whom, and what it never does. Where this document and an ADR differ, the ADR wins
and this is corrected.

## 1. Core sentence

An agent that pays supplier invoices only as far as they match what was agreed.

## 2. For whom

- **The finance operator or founder** who pays a week's supplier invoices in several currencies and
  cannot check every line, bank detail and conversion by hand.
- **The approver**, who sees only the lines the agent may not pay on its own, each with the evidence
  that decided it.
- **The judges** of the Airwallex Agentic Banking Hackathon, who read the demo, the README and the
  evaluation.

The demo's companies, suppliers, invoices and bank details are synthetic.

## 3. What it verifies

Every invoice line, before any money moves, against:

1. **What was agreed:** a purchase order or contract line supports it (supplier, item, quantity,
   price, currency).
2. **Who may be paid:** the beneficiary equals the verified record; a change of bank details arriving
   by email never updates the record.
3. **The payment policy, in code:** the autonomous limit, the duplicate lock, the reserve floor and
   the FX tolerance.

Every line has one state: **matched** (paid within the autonomous limit), **conflict** (a rule
blocks it; the payment halts and a person decides), **proposal** (nothing agreed supports it; never
paid without approval), **uncertain conversion** (the amount after FX and fees drifts beyond
tolerance; the approver decides). Precedence: conflict > proposal > uncertain conversion > matched.

## 4. What it does in the demo

The guide's beats: goal (pay six invoices from four suppliers in USD, GBP and EUR) → initial plan (a
state per line, FX, the SWIFT fee, the reserve floor) → new information (a supplier emails new bank
details; one transfer ends `CANCELLED` with a failure type) → revised decision (the supplier is held
and verified out of band; the failed transfer is replaced under a duplicate lock and the original's
fate recorded) → result (balances, payments, holds and a reconciliation report in which every paid
line traces to its invoice, agreement and approval).

## 5. What it never does

- It never pays a line that is not matched, or a matched line above the autonomous limit, without a
  person's approval.
- It never pays to bank details that are not the verified record, and never takes new details from
  an email.
- It never lets a model decide a conflict, change an amount or call Airwallex directly: a model
  proposes, code decides, and a fired rule stays a halt.
- It never judges whether a purchase was wise and never gives financial or investment advice.
- It never uses real company, person or payment data, and never runs outside the Airwallex sandbox.
- It never logs the contents of a session in the public demo, and the public demo takes no free text.

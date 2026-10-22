# Submission texts

The texts for HackerEarth. Once a text is submitted, it is recorded here **exactly as submitted**,
with the date; until then it is marked DRAFT.

## Idea phase (Registration & Idea Phase, deadline Fri 23.10.2026 21:29 EEST)

**State: SUBMITTED on Thu 22.10.2026 at 18:08 EEST** (HackerEarth status "Published"), filled in
together with the owner in his Chrome; he confirmed the submit. The text below is exactly as
submitted: it was converted from this file to HTML by script (bold, two bullet lists, seven links)
and set into the form's TinyMCE editor, saved as a draft at 18:07, read back from the draft (all
seven links, eight list items, the closing line) and then submitted. The form has two required
fields, Title and Description ("Describe your project, solution, or idea. You can use formatting
and links."). DECISIONS rows 7-10 decided the links.

### Title

As Agreed: an agent that pays only what was agreed

### Description

**The problem.** A finance team paying supplier invoices in several currencies rarely loses money on arithmetic. It loses it on a duplicate invoice, a line nobody ordered, an email saying "we changed our bank details", or a transfer that fails quietly and is then paid twice.

**The solution.** Before any money moves, As Agreed checks every invoice line against what was agreed: the purchase order or contract, the verified beneficiary record and the payment policy. Each line gets exactly one state:

- **matched**: paid through Airwallex within an autonomous limit
- **conflict**: a rule blocks it (duplicate, changed beneficiary, over limit, reserve floor), the payment halts and a person decides
- **proposal**: nothing agreed supports it; shown to the approver, never paid without approval
- **uncertain conversion**: after FX and fees the amount drifts beyond tolerance; goes to the approver

**Built on Airwallex.** Balances, FX rates, quotes and conversions to plan a three-currency run (USD and GBP local, EUR over SWIFT including its fee); beneficiary schema, validation and creation; transfers with simulated status transitions; payout webhooks for reconciliation. Kits 1 (Adaptive Treasury Controller) and 3 (Payment Ops Incident Commander), with reconciliation as the differentiator.

**The demo.** Pay six invoices in three currencies and plan each line. Then new information: a supplier emails new bank details, and one transfer ends CANCELLED. The supplier is held and verified out of band; the failed transfer is replaced under a duplicate lock and the original's fate is recorded. The result is balances, payments, holds and a reconciliation report in which every paid line traces to its invoice, source document and approval.

**Why it can be trusted.** Amounts, limits, duplicate protection and state transitions live in code, and a halt a rule fired stays a halt whatever a model says. Models read invoices, contracts and emails and explain exceptions; a second model from a different family tries to refute every match. Three invariants are tested in code (no double payment, no payment to an unverified beneficiary, nothing below the reserve floor), and a pre-registered error-injection run reports recall and false alarms as they fall.

**Links**

- Concept on one page: https://github.com/Sunrisesoftware-app/as-agreed/blob/main/docs/concept/as-agreed-concept.pdf
- Repository (documents only until the build phase opens): https://github.com/Sunrisesoftware-app/as-agreed
- Previous work, the same verification principle in healthcare: As Prescribed, AMD Developer Hackathon ACT III. Repository https://github.com/Sunrisesoftware-app/as-prescribed, live demo https://asprescribed.sunrisesoftware.app, with its pre-registered evaluation reported as it fell.
- Team: Petri Korhonen, Sunrise Software, Tampere, Finland. https://github.com/Sunrisesoftware-app · https://sunrisesoftware.app · https://www.linkedin.com/in/petri-korhonen-8ab890434/

Synthetic data and the Airwallex sandbox only.

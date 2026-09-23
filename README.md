# lexrates

**Statutory mileage and allowance figures, kept current — with the page they came from, the day
they were read, and the day they expire.**

Somebody has to read fifteen official gazettes every year. This is that work, published as data.

- **24 regimes across 15 countries**, each carrying its publishing authority, source URL, human
  verification date, and the date after which it must be re-read.
- **The corpus as it stood twelve months ago is free and open**, no key and no signup. The last
  twelve months, what is published for later, and the expiry alert are the paid tier — a lag rather
  than a closure, so a regime whose figure never moves still has something free.
- **Two time axes.** `period` says when a figure *applies*; `publishedOn` says when it became
  *known*, and they differ in both directions. New Zealand's 2025-26 kilometre rates came into
  force on 4 June 2026 — fourteen months after the year they govern opened, so a run made before
  that could not have used them. Switzerland published its 2026 figures on 10 September 2025, four
  months early, so a November payroll already had them. Without the second axis you can reproduce
  neither run.

## The month an employee crosses a boundary

Anyone can look up `0.357`. Almost nobody gets right what is owed for the month a threshold is
crossed — and it is not a subtraction you can do from two lookups.

France, 5 CV, 4 800 km already run this year, 900 more this month:

| | |
|---|---|
| `900 × 0.636` | ~~572.40 €~~ the rate below the boundary |
| `900 × 0.357` | ~~321.30 €~~ the rate above it |
| **owed** | **377.10 €** — the whole year re-priced, then what was already earned taken out |

A French bracket applies to the *whole* year's distance, so crossing 5 000 km re-prices everything
already claimed. Send `alreadyClaimed` and the API returns the amount and the working. Marginal
bands are split at the boundary instead; an annual cap counts against the running total.

## The same figures, two different payroll answers

HMRC's approved rates are what an employer may pay tax-free **and** what an employee claims tax
relief on. Germany's per-diem is a Werbungskosten deduction **and**, by direct reference in
§ 3 Nr. 16 EStG, the ceiling under which an employer's reimbursement is exempt. The IRS standard
rate is a self-employed deduction **and** an accountable-plan reimbursement, worked out in figures
in Publication 463.

The arithmetic is identical. What differs is the label on the excess — an exemption is not a
deduction limit — and a payroll team notices on the first payslip. Send `use` and the answer says
which question it answered:

```
paidPerUnit=0.20  use=employer-reimbursement   →  withinScale 200, treatment exempt-from-contributions
paidPerUnit=0.20  use=tax-deduction            →  belowScale  350, treatment deductible-limit
```

That second number is the one an employer paying under the scale actually needs: 1 000 miles at the
55p approved rate is £550, the employer paid £200, and £350 is what relief is claimed on. A rate
table does not tell you that.

⛔ And where the regime's own framing is a kind of journey rather than a tax mechanism — *business
travel*, *commuting* — the API **refuses** to label the split rather than guessing which of the two
you meant. The amount is still answered.

## Why this is not a spreadsheet

Eight shapes a rate takes, all of them real, all of them in the corpus:

| Where | Shape |
|---|---|
| France | Non-marginal brackets with a fixed part. The correction keeps the curve continuous at 5 000 km — except on the 6 CV row, where the order publishes 1 457 and continuity would need 1 455 |
| Ireland | Four marginal bands by engine capacity, and band two pays **more** than band one |
| Italy | No national figure exists at all: the rate is per vehicle model, in the ACI tables |
| Belgium | Two regimes coexist and the employer picks. The federal one is materially lower |
| Germany | Commuting counts the **one-way** distance × days worked, never the distance driven |
| United States | The business rate changed mid-year, so the unit is a period, never a year |
| Australia | A one-off 2 c uplift that indexation must not carry forward |
| Everywhere | Published in March, retroactive to January |

## What it is not

It publishes **parameters**, and does not execute stateful rules. The German three-month per-diem
cutoff, the US tip-credit 80/20 test and French notice periods across 600+ collective agreements
all need the employee's own history — that belongs in your engine, not in a reference feed.

And it is not a call inside your payslip loop. Nobody puts a third-party request in a run over
fifty thousand payslips: what you take is the feed and the alert.

## Pricing

Everything older than twelve months is free with no key. **€149/month** for the twelve months you
are in, the expiry alert and the year-to-date engine; **from €4 500/year** to ship the figures
inside a product you sell — priced by the countries you ship, delivered as a versioned artefact
rather than an HTTP call.

Data escrow is **available on request**: a three-party agreement through a custodian, holding the
corpus, the build scripts and the checksum manifests, at about €1 000 a year rebilled at cost.
Nothing is deposited yet — the first customer who needs it sets it up with me.

There is no sandbox with live data, no webhook, no SLA and no archived proof of each source. Those
are missing, and saying so costs less than being found out.

## Contact

Built and maintained by one person. `info@growingpeas.dev`

An OpenAPI 3.1 description is generated from the corpus itself, so its enumerations cannot drift
from the data: `GET /v1/openapi.json`.

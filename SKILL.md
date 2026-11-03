---
name: databricks-sweden-jamkning
description: Help a Databricks Sweden employee understand Swedish jämkning, decide whether it is useful, collect and reconcile payroll/RSU/capital data, model uncertainty, and prepare a Skatteverket application for ändrad beräkning av preliminär A-skatt.
---

# Databricks Sweden — Jämkning

## Purpose

Use this skill to help a Databricks Sweden employee:

1. understand what **jämkning** is;
2. decide whether applying is likely to be useful;
3. understand the downside of lowering withholding when future income is uncertain;
4. collect the payroll, RSU, investment, interest, and deduction data needed for a defensible estimate;
5. reconcile historical payroll against Skatteverket before forecasting;
6. estimate full-year income and deductions;
7. prepare the values for Skatteverket's **Jämkning / ändrad beräkning av preliminär A-skatt** application;
8. review the final application before submission.

This skill is for planning and data preparation. It does not replace Skatteverket guidance or professional tax advice.

## Core concept

**Jämkning changes preliminary withholding, not the final tax liability.**

Swedish payroll normally withholds tax using standard tax tables. One-time payments may use the separate **engångsskatt** table. For a high-income employee, a large one-time payment can therefore be withheld at a high percentage even when the employee's eventual full-year effective tax rate is lower.

A jämkning decision from Skatteverket instructs the main payer to withhold tax differently so that total preliminary tax for the year is closer to expected final tax.

Important:

- Jämkning can result in **lower or higher** withholding.
- It applies only to the calendar year covered by the decision.
- If Skatteverket specifies a percentage, that percentage also applies to one-time payments.
- If expected income changes materially after applying, submit an updated application.
- The employee still files the normal tax return the following year.

Official references:
- https://www.skatteverket.se/jamkning
- https://www.skatteverket.se/foretag/arbetsgivare/arbetsgivaravgifterochskatteavdrag/skatteavdrag/engangsbelopp.4.361dc8c15312eff6fd3225e.html

## Why this is particularly relevant at Databricks

Databricks compensation can contain:

- base salary;
- sales variable / commission;
- RSU vestings;
- taxable benefits;
- salary exchange (`löneväxling`);
- allowances;
- occasional one-time payroll items.

For employees above the high-income threshold, RSU payroll events may be processed as an **engångsbelopp** with a high withholding percentage.

For example, in the 2026 Skatteverket engångstabell, column 1 applies 54% withholding above the relevant annual-income threshold. The exact thresholds and percentages change by tax year. Always verify the current year's table.

Jämkning can therefore be valuable when:

- RSU tax withholding is materially above expected marginal/final tax;
- the employee has deductible mortgage interest;
- the employee has recurring deductions or tax reductions;
- historical tax returns show large refunds caused by systematic over-withholding.

Do not assume that every Databricks employee with RSUs should apply. The answer depends on the complete annual tax picture.

# 1. Decide whether jämkning is worth investigating

## Strong indicators

Model jämkning when one or more of these apply:

- large RSU vestings during the year;
- large bonus or commission payments;
- high one-time withholding on RSUs/bonus;
- substantial mortgage interest;
- multiple employers or payers;
- significant taxable income outside Databricks;
- large recurring tax reductions;
- previous years produced a large tax refund or tax bill;
- compensation changed substantially during the year;
- the employee joined or left Databricks during the year.

## Lower-priority cases

Jämkning may have limited value when:

- compensation is almost entirely fixed monthly salary;
- payroll withholding has historically matched final tax closely;
- there are no meaningful deductions or other income;
- only a small amount of the year remains and no large payment is pending.

## Quick diagnostic

Ask:

1. How much tax has already been withheld this year?
2. What taxable compensation remains?
3. Are any large RSU vestings or one-time payments still coming?
4. What rate would payroll likely withhold on those payments without jämkning?
5. What deductions or capital items reduce/increase final tax?
6. Is the remaining uncertainty large enough that lowering withholding creates a meaningful risk of `kvarskatt`?

If likely excess withholding is only a few thousand SEK, the administrative value may be low.

# 2. Explain the tradeoff

The objective is not “pay less tax.” It is “avoid lending too much money to Skatteverket during the year while avoiding an unexpected tax bill.”

## Lower withholding

Advantages:

- more cash retained during the year;
- less capital tied up until the next tax refund;
- particularly valuable before a large RSU vesting.

Risks:

- underestimated commission, RSUs, foreign income, investment income, rental income, or other compensation can create `kvarskatt`;
- a forecast that was reasonable earlier in the year may become wrong after a large later event;
- a jämkning decision should be updated if assumptions materially change.

## Conservative modelling principle

When uncertain:

- slightly **overestimate taxable income**;
- slightly **underestimate deductions**;
- do not count a deduction unless the employee reasonably expects to qualify;
- document which inputs are estimates.

For uncertain non-Databricks income, create at least two scenarios:

- **base case**: best estimate;
- **conservative case**: plausible higher taxable income / lower deductions.

If the resulting withholding rate changes materially between the two cases, do not optimize aggressively.

# 3. Required source data

## A. Skatteverket — year-to-date income summary

Obtain:

**Mina inkomstuppgifter → Sammanställning inkomstuppgifter**

Extract:

- total `Lön, förmåner, sjukpenning m.m.`;
- `Kontant bruttolön m.m.`;
- `Övriga skattepliktiga förmåner`;
- `Summa avdragen preliminärskatt`.

This is the preferred historical starting point because it shows what employers have actually reported to Skatteverket.

### Reconciliation rule

The latest Databricks payslip YTD figures should reconcile to the Skatteverket summary.

If they do not:

- identify which payroll month has not yet been reported;
- do not silently combine inconsistent periods;
- state the cutoff date explicitly.

## B. Databricks payslips

Collect the latest payslip and, when needed, recent months used to estimate variable compensation.

Useful Databricks payroll lines may include:

- `040 Monthly salary`
- `052 Car Allowance`
- `139 RSU - Estimated Tax Offset`
- `140 Commission`
- `641 Salary exchange`
- `837 Cell Phone Allowance`
- `885 Health insurance benefit`
- `890 RSU - Taxable`
- `912 Tax ordinary`
- `914 One time tax`

Also capture:

- monthly `Gross pay`;
- monthly `Benefits`;
- monthly `Deducted tax`;
- YTD `Gross pay`;
- YTD `Benefits`;
- YTD `Deducted tax`;
- tax table.

### RSU reconciliation pattern

In observed Databricks Sweden payroll, these two RSU components:

- `RSU - Estimated Tax Offset`
- `RSU - Taxable`

have summed to the **full SEK value of the corresponding Shareworks vesting**.

Reconciliation:

```text
RSU full taxable vest value
≈ RSU - Estimated Tax Offset
+ RSU - Taxable
```

and:

```text
One time tax base
≈ full taxable RSU vest value
```

Do not assume these pay-code semantics are permanent. Reconcile them against the employee's own Shareworks release report and payslip.

## C. Shareworks — completed RSU vestings

Export:

**RSU Release Summary - YYYY-MM-DD to YYYY-MM-DD.xlsx**

Extract each vesting event:

- release/vest date;
- grant ID;
- shares released;
- fair market value per share;
- total value;
- FX / local-currency value;
- shares withheld for tax;
- tax due / tax paid;
- shares delivered.

For each release date, aggregate all grants vesting on that date.

### Reconcile Shareworks to payroll

For each completed vesting:

```text
Shareworks total local-currency vest value
≈ Databricks payroll RSU one-time tax base
```

If the figures differ:

- check whether payroll uses a different FX conversion;
- check timing: a vest in one month may appear on the next payroll;
- check whether all grants on the release date were included;
- do not proceed until the difference is understood or explicitly bounded.

## D. Shareworks — future RSU vestings

Collect grant-detail PDFs or the award schedule for every active grant.

For each remaining vest date, record:

- vest date;
- shares vesting from each grant;
- total shares vesting.

Then calculate:

```text
Future RSU taxable value
= shares vesting
× FMV/share
× applicable SEK FX
```

### Databricks-specific forecasting

If FMV is fixed for the relevant private-company valuation period, use that known FMV.

For FX:

- use the employer/Shareworks fiscal-year conversion method if known;
- otherwise infer the payroll conversion basis from previous completed releases;
- document the assumption.

Never invent a market FX rate if payroll demonstrably uses a fixed internal/fiscal-year conversion.

## E. Compensation forecast

Collect:

- current annual/monthly base salary;
- effective date of salary changes;
- target variable compensation;
- recent actual commission;
- known future commission;
- recurring allowances and benefits;
- salary exchange;
- planned unpaid leave or other material changes.

If future commission is unknown, use a documented forecasting method, for example:

```text
forecast monthly commission
= average(last N months actual commission) × prudence factor
```

Do not use total gross payroll as a proxy for commission.

## F. Mortgage interest

Use expected **full-year interest attributable to the employee**, not principal repayments.

Preferred sources:

- bank annual interest statement;
- YTD loan transactions plus expected remaining interest;
- bank-provided forecast.

Separate:

- loans secured by property;
- unsecured loans.

Only enter the employee's own share if the loan is jointly held.

Normal BRF monthly fees are not mortgage interest and should not be entered as such.

## G. ISK / investment savings

For an exact ISK calculation, collect:

- account value on 1 January;
- account value on 1 April;
- account value on 1 July;
- account value on 1 October;
- deposits during the year;
- relevant transfers into the ISK.

The capital base is calculated from quarterly opening values plus relevant deposits/transfers, divided by four.

Official reference:
https://www.skatteverket.se/investeringssparkonto

### 2026 reference only

For income year 2026:

- schablonintäkt rate: 3.55% of capital base;
- tax-free base: SEK 300,000 across eligible ISK/KF/PEPP savings;
- capital tax rate: 30%.

These numbers are year-specific. Re-check them for every new tax year.

### If exact ISK data is unavailable

Use a conservative estimate and disclose it in `Övriga upplysningar`.

Do not pretend a current portfolio snapshot is an exact statutory capital base.

## H. Other deductions and tax reductions

Check:

- qualifying gifts;
- a-kassa fees;
- commuting deductions;
- temporary work / double residence;
- eligible employment expenses;
- ROT/RUT/grön teknik where relevant;
- capital gains/losses;
- rental income;
- foreign income;
- interest/dividends outside ISK;
- other employers.

For gifts and a-kassa, verify current-year eligibility with Skatteverket.

# 4. Build the full-year model

## Historical actuals

Start from the latest Skatteverket-reported totals:

```text
YTD taxable employment income
YTD tax withheld
```

Do not reconstruct YTD from individual payslips if Skatteverket already provides an authoritative total, except as a reconciliation check.

## Remaining-year forecast

Calculate:

```text
remaining regular employment income
+ future commission
+ future taxable benefits
+ future RSU vestings
+ other taxable employment income
= remaining taxable employment income
```

Then:

```text
full-year employment income
= YTD reported employment income
+ remaining taxable employment income
```

## Capital

Model separately:

```text
interest/dividend income
+ ISK schablonintäkt
+ taxable capital gains
+ rental surplus
- deductible mortgage interest
- deductible capital losses
= net capital position
```

Do not simply subtract mortgage interest from salary.

## Tax reductions

Model separately when relevant:

- capital-deficit reduction;
- gift reduction;
- a-kassa reduction;
- other eligible reductions.

# 5. Estimate whether payroll is over- or under-withholding

Calculate:

```text
expected final tax
- tax already withheld
= remaining tax that should be paid
```

Then compare with likely withholding under normal payroll:

```text
expected normal remaining withholding
- required remaining tax
= expected over-withholding
```

A large positive number indicates potential value from jämkning.

## RSU-specific comparison

For each remaining RSU vest:

```text
normal engångsskatt withholding
vs
expected withholding under jämkning
```

For high-income employees, the difference on a large vest can dominate the economics of the application.

Do not tell the employee to request a particular percentage unless the form explicitly asks for one. In the normal jämkning flow, the employee supplies annual estimates and Skatteverket determines the withholding decision.

# 6. Handle uncertainty explicitly

Create an assumptions table:

| Input | Value | Confidence | Source |
|---|---:|---|---|
| YTD income | actual | High | Skatteverket |
| YTD tax | actual | High | Skatteverket |
| Base salary | contractual | High | compensation letter |
| Future RSU shares | scheduled | High | Shareworks grants |
| RSU FMV | fixed/estimated | High/Medium | company valuation |
| RSU FX | inferred/fixed | Medium/High | prior releases |
| Commission | forecast | Medium | recent payslips |
| Mortgage interest | forecast | Medium/High | bank |
| ISK schablonintäkt | exact/estimate | High/Low | broker |

For uncertain non-Databricks income, run sensitivity cases.

Example:

```text
Scenario A: no additional external income
Scenario B: +100,000 SEK external taxable income
Scenario C: +250,000 SEK external taxable income
```

If the application is sensitive to those scenarios, use a conservative estimate or postpone aggressive lowering of withholding.

# 7. Fill the Skatteverket application

The exact UI can change. Follow the current labels shown by Skatteverket.

## Tjänst — income and tax so far

Use actual YTD values from `Sammanställning inkomstuppgifter`.

Typical fields:

- `02 Lön och förmåner` → YTD total employment income
- `01 Avdragen skatt` → YTD preliminary tax withheld

Other fields should be zero/blank unless they actually apply.

## Tjänst — income for the rest of the year

Typical fields:

- `07 Lön och förmåner` → remaining taxable employment income, including future RSU vestings
- `96 varav från huvudutbetalare` → amount of 07/08/14 expected from the main payer
- `94 ... som det inte dras skatt från` → only income for which no withholding is made

For a Databricks employee with no other main payer, `96` will often equal the Databricks amount included in `07`.

Use the actual remaining months shown by the form.

## Tjänst — annual deductions/reductions

Potentially relevant:

- `76 Gåva`
- `75 Avgift till a-kassa`

Do not populate employment-expense boxes merely because they exist.

## Kapital

Potentially relevant:

- `39 Schablonintäkt investeringssparkonto`
- `34 Ränteutgifter för lån med säkerhet m.m.`

Use the **gross ISK schablonintäkt** requested by the form. Do not manually apply tax-free allowances unless the form/instructions explicitly require it.

Do not enter normal BRF monthly fees as mortgage interest.

## Övriga upplysningar

Use this field to disclose material estimation assumptions.

Recommended template:

```text
The estimated employment income for the remainder of the year includes an RSU
vesting on [DATE] of approximately SEK [AMOUNT]. The ISK standard income is
estimated based on currently available capital data because the final annual
statement is not yet available.
```

Adapt the wording to the actual uncertainties.

# 8. Final validation before submission

## Historical reconciliation

- Skatteverket YTD income matches the latest Databricks payroll YTD income.
- Skatteverket YTD withheld tax matches payroll YTD withheld tax.
- Completed Shareworks releases reconcile to RSU payroll events.

## Forecast

- salary changes are applied from the correct month;
- commission forecast method is documented;
- all remaining RSU grants on each vest date are included;
- future vest value uses the correct FMV and FX assumption;
- recurring benefits/allowances are included;
- salary exchange is treated consistently with payroll;
- external income is included or explicitly scenario-modelled.

## Capital

- mortgage interest is the employee's own full-year share;
- ISK is based on statutory capital-base logic or explicitly marked as estimated;
- capital gains/losses and rental income are not omitted;
- tax reductions are supported.

## Form

- YTD numbers are actuals, not full-year forecasts;
- future-income box contains only the remainder of the year;
- no income is counted twice;
- RSU vestings are included in taxable employment income;
- main-payer amount is consistent;
- free-text assumptions are concise and accurate.

# 9. After submission

Tell the employee to:

1. save the application receipt;
2. save the Skatteverket decision;
3. follow the employer/payroll process for applying the decision if it is not picked up automatically;
4. verify the next payslip actually reflects the new withholding basis;
5. re-run the forecast after any material event:
   - compensation change;
   - unexpected commission;
   - new RSU grant/vesting change;
   - external income;
   - property sale;
   - large capital gain/loss;
   - major change in mortgage interest.

A jämkning application is not “set and forget.”

# 10. Recommended evidence package

```text
/Skatteverket
  sammanstallning-inkomstuppgifter.pdf
  previous-final-tax-assessment.pdf   # optional but useful

/Databricks-Payroll
  latest-payslip.pdf
  recent-payslips-used-for-commission-forecast.pdf
  compensation-letter.pdf

/Shareworks
  RSU Release Summary - YTD.xlsx
  grant-detail-<grant-id>.pdf
  grant-detail-<grant-id>.pdf
  ...

/Capital
  mortgage-interest-estimate.pdf-or-csv
  broker-isk-quarterly-values.csv
  broker-transactions.csv

/Other
  a-kassa-fees
  qualifying-gifts
  external-income-support
```

Previous final-tax assessments are useful for detecting recurring patterns such as:

- systematic over-withholding;
- mortgage-interest levels;
- recurring gift/a-kassa reductions;
- capital losses;
- old loans that no longer exist.

Do not blindly carry prior-year items into the current year.

# 11. Databricks RSU checklist

For every vest date:

```text
[ ] identify every grant vesting that day
[ ] sum shares vesting
[ ] confirm FMV/share
[ ] confirm/infer SEK FX basis
[ ] calculate total SEK vest value
[ ] compare with Shareworks local value when available
[ ] match completed vest to Databricks payroll
[ ] confirm payroll one-time tax base
[ ] confirm tax withheld
[ ] include future vest in annual tjänsteinkomst
```

If one vest date contains multiple grants, never forecast from only one grant.

# 12. Common mistakes

Avoid these:

1. Using only base salary and forgetting RSUs.
2. Using net shares received instead of gross taxable vest value.
3. Treating Shareworks tax-withheld shares as reducing taxable income.
4. Double-counting RSUs by adding both a full Shareworks vest and already-reported payroll RSU amounts for the same event.
5. Using a current ISK balance as if it were the exact statutory capital base.
6. Subtracting the ISK tax-free threshold manually from box 39 when the form asks for gross schablonintäkt.
7. Entering BRF monthly fees as mortgage interest.
8. Carrying an old unsecured-loan deduction into the current year after the loan was refinanced.
9. Ignoring non-Databricks income.
10. Treating jämkning as a reduction in final tax.
11. Forgetting to update the application after a material forecast change.
12. Optimizing to the last krona when major inputs remain uncertain.

# 13. Output format for the assistant

Before recommending submission, produce four blocks.

## A. Reconciliation

```text
YTD employment income reported to Skatteverket:
YTD tax withheld:
Databricks payroll match:
RSU/Shareworks match:
Cutoff date:
```

## B. Full-year forecast

```text
YTD actual employment income:
Remaining regular income:
Remaining commission:
Remaining RSUs:
Other employment income:
Projected full-year employment income:
```

## C. Capital and reductions

```text
Mortgage interest:
ISK schablonintäkt:
Other capital income/loss:
Gifts:
A-kassa:
Other:
```

## D. Application fields

List every non-zero Skatteverket field and its proposed value.

For estimated fields, label them `ESTIMATE`.

Do not recommend submission until:

- actual YTD income and tax are reconciled;
- all known future RSU vestings are included;
- material external income has been considered;
- the employee understands the kvarskatt risk.

# 14. Tax-year verification rule

Never reuse tax-year constants without checking current Skatteverket guidance.

At minimum, verify:

- engångsskatt thresholds/rates;
- state income-tax threshold;
- municipal tax assumptions if independently estimating final tax;
- ISK schablon rate;
- ISK tax-free base;
- gift rules;
- a-kassa rules;
- jämkning application deadlines.

The workflow in this skill is reusable. The tax parameters are not.

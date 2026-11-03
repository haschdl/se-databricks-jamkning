---
name: databricks-sweden-jamkning
description: Help Databricks Sweden employees with Swedish jämkning / jamkning when they ask about preliminary tax, RSU tax withholding, engångsskatt, kvarskatt, tax refunds, Skatteverket, mortgage-interest deductions, ISK, commissions, or whether payroll is withholding too much or too little. Guide them through deciding whether jämkning is useful, collecting Databricks payroll and Shareworks evidence, reconciling actuals, forecasting full-year income, modelling uncertainty, preparing the Skatteverket application, and validating the result before submission.
---

# Databricks Sweden — Jämkning

## Purpose

Use this skill to help a Databricks Sweden employee:

1. understand what **jämkning** is;
2. decide whether applying is likely to be useful;
3. understand the tradeoff between lower withholding and `kvarskatt` risk;
4. collect the evidence needed for a defensible estimate;
5. reconcile historical payroll and RSU data before forecasting;
6. estimate full-year income, capital items, and deductions;
7. prepare the Skatteverket application;
8. validate the application before submission.

This skill supports planning and data preparation. It does not replace Skatteverket guidance or professional tax advice.

## Read these references when needed

- [Databricks payroll and Shareworks](references/databricks-payroll.md) — payroll codes, RSU release extracts, reconciliation, compensation forecasting.
- [2026 tax constants](references/2026-tax-constants.md) — year-specific rates, thresholds, ISK values, and deadlines. Re-verify for other tax years.
- [Skatteverket form fields](references/skatteverket-form-fields.md) — current field mappings and form-entry guidance.
- [Checklists](references/checklists.md) — evidence package, RSU checklist, final validation, common mistakes, and output template.

## Core concept

**Jämkning changes preliminary withholding, not final tax liability.**

Swedish payroll normally withholds tax using standard tax tables. One-time payments may use the separate **engångsskatt** table. A jämkning decision from Skatteverket changes how the main payer withholds tax so that preliminary tax for the year is closer to expected final tax.

Important:

- Jämkning can result in lower or higher withholding.
- It applies only to the calendar year covered by the decision.
- If expected income changes materially after applying, submit an updated application.
- The employee still files the normal tax return the following year.
- Do not assume that every Databricks employee with RSUs benefits from jämkning; model the complete annual tax picture.

Official starting point: https://www.skatteverket.se/jamkning

# 1. Decide whether jämkning is worth investigating

Model jämkning when one or more of these apply:

- large RSU vestings;
- large bonus or commission payments;
- high one-time withholding on RSUs or bonus;
- substantial mortgage interest;
- multiple employers or payers;
- significant taxable income outside Databricks;
- large recurring deductions or tax reductions;
- previous years produced a large refund or tax bill;
- compensation changed substantially during the year;
- the employee joined or left Databricks during the year.

Jämkning may have limited value when compensation is almost entirely fixed salary, withholding has historically matched final tax closely, there are no meaningful deductions or other income, and no large payment remains in the year.

Ask:

1. How much tax has already been withheld?
2. What taxable compensation remains?
3. Are large RSU vestings or other one-time payments still coming?
4. What would payroll likely withhold without jämkning?
5. What deductions and capital items affect final tax?
6. How uncertain is non-Databricks income?
7. Could lowering withholding create material `kvarskatt` risk?

If likely excess withholding is small, the administrative value may be low.

# 2. Explain the tradeoff

The objective is not to pay less tax. It is to make preliminary withholding track expected final tax more closely.

Lower withholding gives the employee more cash during the year and avoids tying up money until a refund. The main risk is underestimating taxable income or overestimating deductions, which can create `kvarskatt`.

When material inputs are uncertain:

- slightly overestimate taxable income;
- slightly underestimate deductions;
- do not count a deduction unless eligibility is reasonably clear;
- mark estimates explicitly;
- model at least a base case and a conservative case.

For uncertain non-Databricks income, test plausible higher-income scenarios. If the recommendation changes materially, do not optimize aggressively.

# 3. Collect source data

Before calculating, collect authoritative actuals and forecast inputs.

## Skatteverket actuals

Obtain **Mina inkomstuppgifter → Sammanställning inkomstuppgifter** and capture:

- total employment income reported so far;
- cash gross salary;
- taxable benefits;
- preliminary tax withheld.

Use this as the preferred historical starting point. Reconcile it to the latest Databricks payslip and state the cutoff date if reporting periods differ.

## Databricks payroll and Shareworks

Collect:

- latest Databricks payslip;
- recent payslips needed for commission forecasting;
- compensation letter if salary changed;
- completed Shareworks RSU release summary;
- grant details / vest schedules for remaining awards.

Use [references/databricks-payroll.md](references/databricks-payroll.md) for exact extracts, payroll codes, RSU reconciliation, FX/FMV handling, and forecast construction.

## Mortgage interest and capital

Collect:

- expected full-year mortgage interest attributable to the employee;
- ISK quarterly values and deposits/transfers where available;
- capital gains/losses;
- rental income;
- interest/dividends outside ISK;
- other taxable investment events.

For ISK calculations and current-year constants, use [references/2026-tax-constants.md](references/2026-tax-constants.md).

## Other income and reductions

Check for:

- other employers;
- foreign income;
- sickness or parental benefits;
- qualifying gifts;
- a-kassa fees;
- commuting or double-residence deductions;
- ROT/RUT/grön teknik where relevant;
- any other material taxable income or deduction.

# 4. Build the full-year model

Keep historical actuals separate from forecast values.

## Historical actuals

Start from Skatteverket-reported values:

```text
YTD taxable employment income
YTD tax withheld
```

Do not rebuild YTD from individual payslips when an authoritative Skatteverket total exists, except to reconcile differences.

## Remaining-year employment forecast

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

Use the Databricks reference for RSU and compensation details.

## Capital model

Keep capital separate from employment income:

```text
interest/dividend income
+ ISK schablonintäkt
+ taxable capital gains
+ rental surplus
- deductible interest
- deductible capital losses
= net capital position
```

Do not simply subtract mortgage interest from salary.

## Tax reductions

Model relevant reductions separately, for example capital-deficit reduction, gifts, a-kassa, and other eligible reductions.

# 5. Estimate over- or under-withholding

Calculate or obtain a defensible estimate of expected final tax, then compare it with tax already withheld and likely remaining payroll withholding.

```text
expected final tax
- tax already withheld
= remaining tax that should be paid
```

Then:

```text
expected normal remaining withholding
- required remaining tax
= expected over-withholding
```

A positive result indicates potential value from jämkning; a negative result indicates under-withholding risk.

For each remaining RSU vest, compare normal payroll withholding with the expected tax outcome rather than assuming the vest is automatically over-withheld.

Tax-method corrections and current-year rates are handled by issues #1, #2, and #4; do not invent missing rules.

# 6. Handle uncertainty explicitly

Maintain an assumptions table:

| Input | Value | Confidence | Source |
|---|---:|---|---|
| YTD income | actual | High | Skatteverket |
| YTD tax | actual | High | Skatteverket |
| Base salary | contractual | High | compensation letter |
| Future RSU shares | scheduled | High | Shareworks |
| RSU FMV | fixed/estimated | High/Medium | company valuation |
| RSU FX | fixed/inferred | High/Medium | prior releases/payroll |
| Commission | forecast | Medium | recent payslips |
| Mortgage interest | forecast | Medium/High | bank |
| ISK schablonintäkt | exact/estimate | High/Low | broker |

For uncertain external income, run sensitivity scenarios. If the result is sensitive to plausible changes, use a conservative estimate or avoid aggressively lowering withholding.

# 7. Prepare the Skatteverket application

Use the current labels shown by Skatteverket. Do not rely on remembered field numbers without verification.

Use [references/skatteverket-form-fields.md](references/skatteverket-form-fields.md) for the currently documented mappings and known verification gaps.

General rules:

- historical boxes use YTD actuals;
- future-income boxes contain only the remainder of the year;
- include future RSU vestings in remaining taxable employment income;
- identify the main payer correctly;
- enter only deductions/reductions that actually apply;
- avoid double-counting income already present in YTD actuals.

## Övriga upplysningar

Use free text only for material assumptions that are not obvious from the numeric boxes, for example a large future RSU vest or an estimated ISK amount.

Keep it concise and factual.

# 8. Validate before submission

Before recommending submission, verify:

- YTD Skatteverket income and withholding reconcile with payroll;
- completed Shareworks releases reconcile with Databricks RSU payroll events;
- all remaining vestings are included;
- salary changes and commission assumptions are applied in the correct periods;
- benefits, allowances, and salary exchange are treated consistently with payroll;
- external income has been considered;
- mortgage interest and ISK are based on defensible full-year estimates;
- no income or deduction is counted twice;
- estimates are clearly identified.

Use [references/checklists.md](references/checklists.md) for the detailed evidence and validation checklists.

# 9. After submission

Tell the employee to:

1. save the application receipt;
2. save the Skatteverket decision;
3. follow the employer/payroll process for applying the decision if required;
4. verify that subsequent payroll reflects the decision;
5. re-run the model after any material compensation, RSU, capital, or external-income change.

A jämkning application is not set-and-forget.

# 10. Output format for the assistant

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

List every non-zero Skatteverket field/label and its proposed value. Mark estimated fields `ESTIMATE`.

Do not recommend submission until actual YTD income and tax are reconciled, all known future RSU vestings are included, material external income has been considered, and the employee understands the `kvarskatt` risk.

# 11. Tax-year verification rule

Never reuse tax-year constants without checking current Skatteverket guidance.

Use the year-specific reference for the current implementation, and re-verify at minimum:

- engångsskatt thresholds/rates;
- state income-tax threshold;
- municipal-tax assumptions if modelling tax independently;
- ISK schablon rate and tax-free base;
- interest-deduction rules;
- gift/a-kassa rules;
- jämkning deadlines.

The workflow is reusable. Tax parameters are not.

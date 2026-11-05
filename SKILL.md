---
name: databricks-sweden-jamkning
description: Use for Databricks Sweden employee questions such as "Should I apply for jämkning?", "Why is tax on my RSU vest so high?", "Will I owe kvarskatt?", "How do I fill in the Skatteverket application?", or "How should I calculate tax on a tender offer?" Covers Swedish preliminary tax, engångsskatt, Shareworks RSUs, commission, ISK, mortgage interest, capital gains, deductions, and payroll reconciliation. Do not use for unrelated Swedish translation, generic programming, other countries' tax rules, or investment recommendations unrelated to Swedish tax withholding.
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

- [Databricks payroll and Shareworks](references/databricks-payroll.md) — payroll codes, RSU release extracts, reconciliation, engångsskatt diagnostics, tender-offer cost basis, compensation forecasting.
- [2026 tax constants](references/2026-tax-constants.md) — year-specific rates, thresholds, ISK values, and deadlines. Re-verify for other tax years.
- [Skatteverket form fields](references/skatteverket-form-fields.md) — current e-service field mappings and form-entry guidance.
- [Checklists](references/checklists.md) — evidence package, final-tax method, RSU/tender checklists, final validation, common mistakes, and output template.

## Core concept

**Jämkning changes preliminary withholding, not final tax liability.**

Swedish payroll normally withholds tax using standard tax tables. One-time payments may use the separate **engångsskatt** table. A jämkning decision from Skatteverket changes how withholding is calculated so that preliminary tax for the year is closer to expected final tax.

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
- one-time withholding that may not match expected marginal tax;
- a Databricks employee tender offer or other share sale that can create an unwitheld capital gain;
- substantial mortgage interest;
- multiple employers or payers;
- significant taxable income outside Databricks;
- large recurring deductions or tax reductions;
- previous years produced a large refund or tax bill;
- compensation changed substantially during the year;
- the employee joined or left Databricks during the year.

Jämkning may have limited value when compensation is almost entirely fixed salary, withholding has historically matched final tax closely, there are no meaningful deductions or other income, and no large payment or capital event remains in the year.

Ask:

1. How much tax has already been withheld?
2. What taxable compensation remains?
3. Are large RSU vestings or other one-time payments still coming?
4. Is a tender offer or other Databricks share sale expected or already completed?
5. What would payroll likely withhold without jämkning?
6. What deductions and capital items affect final tax?
7. How uncertain is non-Databricks income?
8. Could lowering withholding create material `kvarskatt` risk?

If likely excess withholding is small, the administrative value may be low.

# 2. Explain the tradeoff

The objective is not to pay less tax. It is to make preliminary withholding track expected final tax more closely.

Lower withholding gives the employee more cash during the year and avoids tying up money until a refund. The main risk is underestimating taxable income or overestimating deductions, which can create `kvarskatt`. Capital gains with no payroll withholding can materially change that picture.

When material inputs are uncertain:

- slightly overestimate taxable income;
- slightly underestimate deductions;
- do not count a deduction unless eligibility is reasonably clear;
- mark estimates explicitly;
- model at least a base case and a conservative case.

For uncertain non-Databricks income or share-sale proceeds, test plausible scenarios. If the recommendation changes materially, do not optimize aggressively.

# 3. Collect source data

Before calculating, collect authoritative actuals and forecast inputs.

## Skatteverket actuals

Obtain **Mina inkomstuppgifter → Sammanställning inkomstuppgifter** and capture:

- total employment income reported so far;
- cash gross salary;
- taxable benefits;
- preliminary tax withheld.

Use this as the preferred historical starting point. Reconcile it to the latest Databricks payslip and state the cutoff date if reporting periods differ.

## Databricks payroll, Shareworks, and tender events

Collect:

- latest Databricks payslip;
- recent payslips needed for commission forecasting;
- compensation letter if salary changed;
- completed Shareworks RSU release summary;
- grant details / vest schedules for remaining awards;
- tender-offer/sale confirmation when applicable;
- complete acquisition/vest history for all shares of the same kind when a tender/share sale must be modelled.

Use [references/databricks-payroll.md](references/databricks-payroll.md) for exact extracts, payroll codes, årslön capture, RSU reconciliation, engångsskatt diagnostics, FX/FMV handling, tender-offer `genomsnittsmetoden`, and forecast construction.

## Mortgage interest and capital

Collect:

- expected full-year interest attributable to the employee;
- ISK quarterly values and deposits/transfers where available;
- Databricks tender/share-sale proceeds and cost-basis evidence;
- other capital gains/losses;
- rental income;
- interest/dividends outside ISK;
- other taxable investment events.

For ISK calculations and loan-interest rules, use [references/2026-tax-constants.md](references/2026-tax-constants.md). Do not carry prior-year deduction rules forward by assumption.

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
+ Databricks tender/share-sale capital gains
+ other taxable capital gains
+ rental surplus
- deductible interest
- deductible capital losses
= net capital position
```

For a Databricks tender/share sale, use the Swedish average-cost method described in the Databricks reference. Do not assign the sold shares a per-vest lot basis.

Use the year-specific constants reference to determine whether loan interest is deductible. Do not assume that an interest deduction available in a prior year still applies.

Do not simply subtract mortgage interest from salary.

## Tax reductions

Model relevant reductions separately, for example capital-deficit reduction, gifts, a-kassa, and other eligible reductions. Use only verified current-year rules.

# 5. Estimate over- or under-withholding

## Preferred method: use an official final-tax calculation

Prefer Skatteverket's e-service or official tax calculator for the **expected final tax** calculation when it can represent the employee's forecast inputs. Use the same full-year income, capital, deductions, and reductions that will be used in the jämkning analysis.

Treat the resulting final-tax figure as the comparison point rather than recreating the entire Swedish tax engine from memory.

## If hand-modelling is necessary

Label the result as an `ESTIMATE` and show the calculation components explicitly. At minimum, consider:

- taxable earned income after grundavdrag;
- municipal income tax;
- state income tax where applicable;
- church fee where applicable;
- burial fee;
- public-service fee;
- jobbskatteavdrag;
- capital income, including tender/share-sale gains;
- capital-deficit tax reduction;
- gifts, a-kassa, or other applicable tax reductions;
- any other material tax item in the employee's situation.

Every tax-year-specific rate, threshold, cap, or formula must come from the year-specific reference or a current Skatteverket source. If a component is not verified, mark it `UNVERIFIED`; do not fill it from memory.

## Compare final tax with withholding

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

For RSUs, use [references/databricks-payroll.md](references/databricks-payroll.md) to compare engångsskatt with estimated **marginal** tax and to check whether payroll used an appropriate årslön. Do not infer over-withholding from the engångsskatt percentage alone.

For tender/share-sale gains, do not assume payroll withholding covers the capital gain. Model the gain and its withholding separately.

## If the model shows under-withholding

Do not force a jämkning strategy whose only purpose is to lower withholding. Depending on timing and the employee's situation, options can include:

- a new/updated jämkning that results in more appropriate withholding;
- asking payroll about additional withholding where operationally available;
- a voluntary extra tax payment (`fyllnadsinbetalning`).

The dates and interest rules for fyllnadsinbetalning are **UNVERIFIED** unless they are explicitly sourced from current Skatteverket guidance. Do not advise a payment date or interest outcome from memory. See the year-specific constants reference.

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
| Tender proceeds | actual/forecast | High/Medium | tender documents |
| Tender average cost basis | reconstructed | High/Medium/Low | complete vest/share history |
| Mortgage interest | forecast | Medium/High | bank |
| ISK schablonintäkt | exact/estimate | High/Low | broker |

For uncertain external income, tender proceeds, or incomplete share-basis history, run sensitivity scenarios. If the result is sensitive to plausible changes, use a conservative estimate or avoid aggressively lowering withholding.

# 7. Prepare the Skatteverket application

Use the current labels shown by Skatteverket. Do not rely on remembered field numbers without verification.

Use [references/skatteverket-form-fields.md](references/skatteverket-form-fields.md) for the verified e-service mappings and known open items.

Use the year-specific constants reference for application timing. If the main expected benefit is an early-next-year vest, evaluate the next-year application in the prior application window rather than waiting until the vest year has started.

General rules:

- historical boxes use YTD actuals;
- future-income boxes contain only the remainder of the year;
- include future RSU vestings in remaining taxable employment income;
- include material capital gains in the capital model/application where the e-service requires them;
- identify the main payer correctly;
- enter only deductions/reductions that actually apply;
- avoid double-counting income already present in YTD actuals.

## Övriga upplysningar

Use free text only for material assumptions that are not obvious from the numeric boxes, for example a future RSU vest, a tender/share-sale estimate, or an estimated/open ISK treatment.

Keep it concise and factual.

# 8. Validate before submission

Before recommending submission, verify:

- YTD Skatteverket income and withholding reconcile with payroll;
- completed Shareworks releases reconcile with Databricks RSU payroll events;
- payroll årslön has been checked where engångsskatt is material;
- all remaining vestings are included;
- tender/share-sale gains use the average cost basis across all same-kind shares rather than sold-lot basis;
- capital gains with no payroll withholding are included in kvarskatt risk;
- salary changes and commission assumptions are applied in the correct periods;
- benefits, allowances, and salary exchange are treated consistently with payroll;
- external income has been considered;
- mortgage interest and ISK are based on defensible full-year estimates;
- expected final tax comes from an official calculation or a fully itemized hand-model;
- no income or deduction is counted twice;
- estimates and unverified inputs are clearly identified.

Use [references/checklists.md](references/checklists.md) for the detailed evidence and validation checklists.

# 9. After submission

Tell the employee to:

1. save the application receipt;
2. save the Skatteverket decision;
3. follow the employer/payroll process for applying the decision if required;
4. verify the effect on the **next RSU vest / engångsbelopp**, not only the next regular payslip;
5. confirm that the type of decision issued actually changes one-time withholding as intended;
6. re-run the model after any material compensation change, RSU change, tender/share sale, other capital gain/loss, or external-income change.

If the main value is expected from an early-next-year RSU vest, consult the year-specific application deadlines before the preceding application window closes.

A jämkning application is not set-and-forget.

# 10. Output format for the assistant

Before recommending submission, produce four blocks.

## A. Reconciliation

```text
YTD employment income reported to Skatteverket:
YTD tax withheld:
Databricks payroll match:
RSU/Shareworks match:
Payroll årslön used:
Expected main-employer årslön:
Main/secondary payer status:
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
Databricks tender/share-sale gain:
Other capital income/loss:
Gifts:
A-kassa:
Other:
```

## D. Application fields

List every non-zero Skatteverket field/label and its proposed value. Mark estimated fields `ESTIMATE` and unresolved inputs `UNVERIFIED`.

Do not recommend submission until actual YTD income and tax are reconciled, all known future RSU vestings are included, payroll årslön has been checked where engångsskatt is material, tender/share-sale gains are modelled when applicable, material external income has been considered, expected final tax is defensible, and the employee understands the `kvarskatt` risk.

# 11. Tax-year verification rule

Never reuse tax-year constants without checking current Skatteverket guidance.

Use the year-specific reference for the current implementation. If the requested tax year differs, treat the existing rates, thresholds, deadlines, deduction rules, and field mappings as unavailable until re-verified.

The workflow is reusable. Tax parameters are not.

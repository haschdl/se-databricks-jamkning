# Jämkning checklists

## Recommended evidence package

```text
/Skatteverket
  sammanstallning-inkomstuppgifter.pdf
  previous-final-tax-assessment.pdf   # optional but useful

/Databricks-Payroll
  latest-payslip.pdf
  recent-payslips-used-for-commission-forecast.pdf
  compensation-letter.pdf
  payroll-arslon-basis-or-explanation   # if available

/Shareworks
  RSU Release Summary - YTD.xlsx
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

Prior final-tax assessments can help detect recurring over/under-withholding, mortgage-interest patterns, tax reductions, capital losses, and items that no longer exist. Never carry prior-year values forward blindly.

## Databricks RSU checklist

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
[ ] capture or infer årslön used by payroll; mark UNKNOWN if unavailable
[ ] determine whether payer is main or secondary
[ ] compare payroll årslön with expected full-year main-employer income
[ ] compare engångsskatt rate with estimated marginal rate, not effective rate
[ ] check for understated-årslön under-withholding / kvarskatt risk
[ ] check whether the vest pushes årslön into a higher band; quantify any potential over-withholding gap
[ ] confirm tax withheld
[ ] include future vest in annual tjänsteinkomst
```

If multiple grants vest on the same date, never forecast from only one grant.

## Expected final-tax method

Prefer the result from Skatteverket's e-service or official tax calculator when it can represent the forecast inputs.

If hand-modelling is necessary, label the result `ESTIMATE` and explicitly include or consider:

```text
taxable earned income after grundavdrag
municipal income tax
state income tax where applicable
church fee where applicable
burial fee
public-service fee
jobbskatteavdrag
capital income
capital-deficit tax reduction
applicable gift/a-kassa/other tax reductions
other material tax items
```

Every year-specific rate, threshold, cap, or formula must come from the year-specific constants reference or a current Skatteverket source. Mark anything else `UNVERIFIED`.

When the model indicates under-withholding, present alternatives rather than assuming lower withholding is desirable. A voluntary extra tax payment (`fyllnadsinbetalning`) can be an alternative to increasing withholding. Its dates and interest rules remain `UNVERIFIED` unless current Skatteverket guidance is sourced.

## Final validation

### Historical reconciliation

- Skatteverket YTD income matches the latest comparable Databricks payroll YTD income.
- Skatteverket YTD withheld tax matches the latest comparable payroll period.
- Completed Shareworks releases reconcile to RSU payroll events.
- Payroll årslön assumptions have been checked before drawing conclusions from engångsskatt.
- Cutoff dates are explicit if reporting timing differs.

### Forecast

- salary changes apply from the correct month;
- commission forecast method is documented;
- all remaining RSU grants are included;
- future vest value uses the documented FMV and FX assumption;
- recurring benefits/allowances are included;
- salary exchange is treated consistently with payroll;
- external income is included or scenario-modelled.

### Capital

- mortgage interest is the employee's own full-year share;
- loan-interest deductibility follows the current year-specific rule; prior-year treatment is not carried forward;
- ISK uses statutory capital-base logic or is labelled as estimated;
- material capital gains/losses and rental income are included;
- capital-deficit reduction uses the applicable tiered current-year rule from the constants reference;
- tax reductions are supported.

### Final tax

- use an official Skatteverket final-tax result when available;
- otherwise the hand-model lists every material component and labels unverified inputs;
- do not compare payroll withholding against an unexplained single `expected final tax` number.

### Form

- YTD values are actuals, not full-year forecasts;
- future-income values cover only the remainder of the year;
- no income is counted twice;
- future RSU vestings are included in employment income;
- main-payer amount is consistent;
- free-text assumptions are concise and accurate.

## Common mistakes

Avoid:

1. Using only base salary and forgetting RSUs.
2. Treating base salary as payroll årslön for engångsskatt.
3. Assuming a high engångsskatt percentage is automatically excessive instead of comparing it with marginal tax.
4. Using net shares delivered instead of gross taxable vest value.
5. Treating tax-withheld shares as reducing taxable employment income.
6. Double-counting the same RSU in YTD payroll and the future forecast.
7. Using a current ISK balance as the exact statutory capital base.
8. Applying tax-year allowances manually without confirming how the e-service expects the field.
9. Treating interest as deductible because it was deductible in a prior year.
10. Entering BRF monthly fees as mortgage interest.
11. Ignoring non-Databricks income.
12. Treating jämkning as a reduction in final tax.
13. Using an unexplained hand-modelled final-tax number.
14. Failing to update the model after a material forecast change.
15. Optimizing to the last krona when major inputs remain uncertain.

## Assistant output template

### A. Reconciliation

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

### B. Full-year forecast

```text
YTD actual employment income:
Remaining regular income:
Remaining commission:
Remaining RSUs:
Other employment income:
Projected full-year employment income:
```

### C. Capital and reductions

```text
Mortgage interest:
ISK schablonintäkt:
Other capital income/loss:
Gifts:
A-kassa:
Other:
```

### D. Final-tax basis

```text
Method: Skatteverket official result | hand-model ESTIMATE
Expected final tax:
Unverified components:
Under/over-withholding result:
Alternative if under-withheld: updated withholding | fyllnadsinbetalning | other
```

### E. Application fields

List every non-zero field/label and proposed value. Mark estimates `ESTIMATE` and unresolved inputs `UNVERIFIED`.

Do not recommend submission until actual YTD income and tax reconcile, all known future RSUs are included, payroll årslön has been checked where engångsskatt is material, material external income has been considered, expected final tax is defensible, and the employee understands kvarskatt risk.

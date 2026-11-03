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
[ ] confirm tax withheld
[ ] include future vest in annual tjänsteinkomst
```

If multiple grants vest on the same date, never forecast from only one grant.

## Final validation

### Historical reconciliation

- Skatteverket YTD income matches the latest comparable Databricks payroll YTD income.
- Skatteverket YTD withheld tax matches the latest comparable payroll period.
- Completed Shareworks releases reconcile to RSU payroll events.
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
- ISK uses statutory capital-base logic or is labelled as estimated;
- material capital gains/losses and rental income are included;
- tax reductions are supported.

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
2. Using net shares delivered instead of gross taxable vest value.
3. Treating tax-withheld shares as reducing taxable employment income.
4. Double-counting the same RSU in YTD payroll and the future forecast.
5. Using a current ISK balance as the exact statutory capital base.
6. Applying tax-year allowances manually without confirming how the e-service expects the field.
7. Entering BRF monthly fees as mortgage interest.
8. Carrying prior-year loan/deduction items forward when they no longer apply.
9. Ignoring non-Databricks income.
10. Treating jämkning as a reduction in final tax.
11. Failing to update the model after a material forecast change.
12. Optimizing to the last krona when major inputs remain uncertain.

## Assistant output template

### A. Reconciliation

```text
YTD employment income reported to Skatteverket:
YTD tax withheld:
Databricks payroll match:
RSU/Shareworks match:
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

### D. Application fields

List every non-zero field/label and proposed value. Mark estimates `ESTIMATE`.

Do not recommend submission until actual YTD income and tax reconcile, all known future RSUs are included, material external income has been considered, and the employee understands kvarskatt risk.

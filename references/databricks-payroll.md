# Databricks payroll and Shareworks reference

Use this reference for source extraction, RSU reconciliation, and compensation forecasting. These payroll codes are based on observed Databricks Sweden payslips and must be reconciled against the employee's own documents because payroll semantics can change.

## Databricks payslip extracts

Collect the latest payslip and recent months needed to estimate variable compensation.

Useful payroll lines may include:

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

Also capture monthly and YTD totals for:

- gross pay;
- taxable benefits;
- deducted tax;
- tax table.

## Completed Shareworks vestings

Export:

`RSU Release Summary - YYYY-MM-DD to YYYY-MM-DD.xlsx`

Extract for each vest/release event:

- vest/release date;
- grant ID;
- shares released;
- FMV per share;
- total value;
- FX / local-currency value;
- shares withheld for tax;
- tax due / paid;
- shares delivered.

Aggregate all grants vesting on the same date.

## RSU reconciliation pattern

In observed Databricks Sweden payroll, these two components:

- `RSU - Estimated Tax Offset`
- `RSU - Taxable`

have summed to the full SEK value of the corresponding Shareworks vesting:

```text
RSU full taxable vest value
≈ RSU - Estimated Tax Offset
+ RSU - Taxable
```

The one-time tax base has also reconciled approximately to the full taxable vest value:

```text
One time tax base
≈ full taxable RSU vest value
```

Do not treat this as a permanent payroll contract. Reconcile each employee's own Shareworks and payslip data.

If Shareworks and payroll differ, check:

- FX conversion;
- whether the vest appeared on the next payroll month;
- whether every grant vesting that day was included;
- whether the compared values use the same gross/net basis.

Do not proceed until a material difference is understood or explicitly bounded.

## Future RSU vestings

Collect grant-detail PDFs or award schedules for every active grant.

For each future vest date, record:

- vest date;
- shares vesting from every grant;
- total shares vesting.

Forecast:

```text
Future RSU taxable value
= shares vesting
× FMV/share
× applicable SEK FX
```

If FMV is fixed for the relevant Databricks valuation period, use the known FMV.

For FX:

- use the employer/Shareworks fiscal-year method if known;
- otherwise infer the basis from completed releases and payroll;
- document the assumption.

Do not substitute a spot-market FX rate when historical payroll demonstrates a fixed internal conversion basis.

## Compensation forecast

Collect:

- current annual/monthly base salary;
- effective date of salary changes;
- target variable compensation;
- recent actual commission;
- known future commission;
- recurring allowances and taxable benefits;
- salary exchange (`löneväxling`);
- unpaid leave or other material changes.

If future commission is unknown, use a documented method such as:

```text
forecast monthly commission
= average(last N months actual commission) × prudence factor
```

Do not use total gross payroll as a proxy for commission.

## Reconciliation rules

- Prefer Skatteverket's reported YTD employment income and tax as historical actuals.
- Use payslips to reconcile composition and timing.
- Use Shareworks to validate completed RSU events and forecast future ones.
- State the cutoff date whenever systems are not aligned to the same payroll/reporting period.
- Never count an RSU event both in YTD actuals and again in the remaining-year forecast.

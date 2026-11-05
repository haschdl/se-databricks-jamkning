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

/Tender-or-Share-Sale
  tender-or-sale-confirmation
  same-kind-share-vest-history
  prior-sale-history-if-applicable

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

## Databricks tender/share-sale checklist

When a tender offer or other Databricks share sale occurs:

```text
[ ] obtain tender/sale confirmation and SEK proceeds/fees evidence
[ ] identify the exact share kind/class sold
[ ] collect every acquisition/vest for all same-kind shares held before sale
[ ] use the SEK acquisition value at each vest
[ ] include prior sales/transactions that changed the remaining share pool
[ ] compute average cost across the entire remaining same-kind share pool
[ ] allocate average cost per share to the number of shares sold
[ ] do not use per-vest/per-lot basis for the sold shares
[ ] calculate capital gain/loss from SEK proceeds minus allocated average basis
[ ] model the gain separately from employment income
[ ] assume no payroll tax withheld on the capital gain unless transaction evidence shows otherwise
[ ] re-evaluate kvarskatt risk and the jämkning recommendation
```

Source for `genomsnittsmetoden`: Skatteverket — Räkna ut omkostnadsbelopp.

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
capital income, including Databricks tender/share-sale gains
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
- commission forecast method is documented, with a defensible best estimate and a separate evidence-based higher-income stress case; no arbitrary downward prudence multiplier;
- all remaining RSU grants are included;
- future vest value uses the documented FMV and FX assumption;
- recurring benefits/allowances are included;
- salary exchange is treated consistently with payroll;
- tender/share-sale events are included or scenario-modelled;
- external income is included or scenario-modelled.

### Capital

- mortgage interest is the employee's own full-year share;
- loan-interest deductibility follows the current year-specific rule; prior-year treatment is not carried forward;
- ISK uses statutory capital-base logic or is labelled as estimated;
- tender/share-sale cost basis uses `genomsnittsmetoden` across all same-kind shares, not selected lots;
- tender/share-sale capital gain is modelled with no payroll withholding unless evidence shows otherwise;
- material other capital gains/losses and rental income are included;
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
- capital gains are classified under the verified capital section, not an employment-income/no-withholding field;
- a verified current e-service field/label exists for every material capital gain; otherwise mark `UNVERIFIED` and do not present submission-ready entries;
- a free-text explanation is not treated as a substitute for a required numeric capital entry;
- main-payer amount is consistent;
- free-text assumptions are concise and accurate.

## After-submission checks

- verify the next RSU vest / engångsbelopp reflects the issued decision as intended;
- do not rely only on the next regular payslip;
- confirm the decision type actually changes one-time withholding where that is the goal;
- re-run the model after a compensation change, RSU change, tender/share sale, other large capital event, or material external-income change;
- use the year-specific constants reference for application timing, including early-next-year vest planning.

## Common mistakes

Avoid:

1. Using only base salary and forgetting RSUs.
2. Treating base salary as payroll årslön for engångsskatt.
3. Assuming a high engångsskatt percentage is automatically excessive instead of comparing it with marginal tax.
4. Using net shares delivered instead of gross taxable vest value.
5. Treating tax-withheld shares as reducing taxable employment income.
6. Double-counting the same RSU in YTD payroll and the future forecast.
7. Using per-vest/per-lot basis for tender shares instead of average cost across all same-kind shares.
8. Assuming RSU payroll withholding covers a later tender/share-sale capital gain.
9. Using a current ISK balance as the exact statutory capital base.
10. Applying tax-year allowances manually without confirming how the e-service expects the field.
11. Treating interest as deductible because it was deductible in a prior year.
12. Entering BRF monthly fees as mortgage interest.
13. Ignoring non-Databricks income.
14. Treating jämkning as a reduction in final tax.
15. Using an unexplained hand-modelled final-tax number.
16. Failing to update the model after a material forecast change.
17. Optimizing to the last krona when major inputs remain uncertain.
18. Using an employment-income no-withholding field for a share-sale capital gain.
19. Inventing a capital-gain e-service field number when it is not verified.
20. Calling an arbitrary downward commission multiplier 'conservative' for avoiding kvarskatt.

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
Databricks tender/share-sale gain:
Tender/share-sale withholding:
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

Do not recommend submission until actual YTD income and tax reconcile, all known future RSUs are included, payroll årslön has been checked where engångsskatt is material, tender/share-sale gains are modelled when applicable, material external income has been considered, expected final tax is defensible, and the employee understands kvarskatt risk.

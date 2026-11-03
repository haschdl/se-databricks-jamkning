# Databricks payroll and Shareworks reference

Use this reference for source extraction, RSU reconciliation, engångsskatt diagnostics, Databricks tender/share-sale basis, and compensation forecasting. Payroll codes are based on observed Databricks Sweden payslips and must be reconciled against the employee's own documents because payroll semantics can change.

For all tax-year-specific rates and thresholds, use [2026-tax-constants.md](2026-tax-constants.md). Do not repeat or infer those values here.

Source for Swedish share cost-basis method: Skatteverket — Räkna ut omkostnadsbelopp: https://www.skatteverket.se/privat/skatter/vardepapper/deklareraaktierochovrigavardepapper/omkostnadsbelopp.4.12815e4f14a62bc048fa7bc.html

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
- tax table;
- **årslön used by payroll** for engångsskatt, if shown, obtainable from payroll, or inferable from the withholding band;
- whether Databricks is the **main employer/payer** for the payment being analysed.

If the exact payroll årslön is not visible, mark it as `UNKNOWN` rather than assuming that base salary is the årslön.

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

## Engångsskatt and årslön diagnostic

Do not assume that a high engångsskatt rate means the RSU is materially over-withheld.

### 1. Reconstruct expected main-employer årslön

For the applicable tax year, use the definition in the year-specific constants reference. Build expected årslön from the employee's **main employer only**, including the components required by that definition. Compare this with the årslön payroll appears to have used.

Do not substitute base salary for årslön.

### 2. Compare engångsskatt with marginal tax, not effective tax

For an RSU vest processed by the main employer:

```text
withholding gap
= applicable engångsskatt rate
- estimated marginal employment-tax rate
```

Estimate the marginal employment-tax rate using:

```text
applicable municipal income-tax rate
+ applicable church fee
+ burial fee
+ state income-tax component where applicable
```

Use the state component from the year-specific constants reference. Obtain employee/location-specific municipal, church, and burial values rather than inventing them.

Do **not** compare the engångsskatt rate with the employee's average/effective full-year tax rate.

### 3. Under-withholding diagnostic — payroll årslön understated

Flag `KVARSKATT RISK` when payroll's årslön appears too low because it omitted material income that belongs in the main-employer annual estimate, such as known future/earlier vestings, commission, benefits, or other required one-time payments.

A lower engångsskatt band caused by an understated årslön can make an RSU **under-withheld**. In this case, jämkning should not be presented primarily as a way to reduce withholding.

### 4. Potential over-withholding diagnostic — vest pushes årslön into a higher band

If base salary alone is below a threshold but the RSU and other main-employer compensation push expected årslön into a higher engångsskatt band, payroll can apply that higher rate to the **entire one-time payment**.

Treat this as a **potential** over-withholding case only. Quantify the gap against the estimated marginal rate. If the gap is small, say so; do not characterize the withholding as obviously excessive.

### 5. Main versus secondary payer

Determine whether the one-time payment comes from the main payer before using the engångsskatt table. For a secondary payer, use the separate rule in the year-specific constants reference.

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

## Databricks tender offers and other share sales

A Databricks employee tender offer is a **capital event**, not another RSU vest. It can create a capital gain for which ordinary payroll withholding does not cover the gain. Treat this as a material `KVARSKATT RISK` unless the employee has separately accounted for the tax.

### Source data

Collect:

- tender offer / sale confirmation;
- number and kind/class of shares sold;
- sale price and fees;
- SEK sale proceeds or the evidence needed to convert proceeds consistently to SEK;
- every RSU vest/acquisition for **all shares of the same kind** held before the sale;
- shares acquired at each vest;
- SEK value per share at each vest used as the acquisition value;
- prior sales or other transactions that changed the remaining same-kind share pool.

Do not collect only the vest records for the shares the employee thinks were sold. Swedish cost basis is not determined by selecting RSU lots.

### Genomsnittsmetoden — average cost basis

Use the Swedish **genomsnittsmetoden** across all shares of the same kind the employee holds.

For RSU shares, build the acquisition-cost pool from the SEK value at each vest. If same-kind shares were acquired through several vest dates at different FMVs, aggregate those SEK acquisition values across the whole remaining pool.

Conceptually:

```text
total remaining acquisition cost for same-kind shares
÷ total remaining same-kind shares held before sale
= average cost per share
```

Then:

```text
average cost per share
× number of shares sold
= cost basis allocated to the sale
```

And:

```text
SEK sale proceeds
- allocated average cost basis
= capital gain or loss
```

This is **not** the per-lot basis of the specific vest dates from which the sold shares originated. If acquisitions and sales are interleaved, update the remaining average-cost pool after each transaction.

Calculate separately for different kinds/classes of shares.

### Withholding treatment

Do not treat the capital gain as covered by RSU/engångsskatt withholding. Model the capital gain separately with **no payroll tax withheld on the gain**, unless the actual transaction documents show a separate tax payment that must be reconciled.

Include the resulting capital gain/loss in the full-year tax model and re-evaluate whether the employee is actually over- or under-withheld.

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
- Reconcile payroll's årslön assumption against the expected main-employer annual income before interpreting engångsskatt.
- Treat tender/share-sale gain separately from employment income and RSU withholding.
- State the cutoff date whenever systems are not aligned to the same payroll/reporting period.
- Never count an RSU event both in YTD actuals and again in the remaining-year forecast.

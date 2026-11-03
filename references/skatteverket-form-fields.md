# Skatteverket jämkning e-service fields — Tax year 2026

**Tax year:** 2026  
**Verified:** 2026-11-03  
**Source:** Skatteverket `Jämkning 2026` e-service review page, verified in November 2026.

This mapping applies to the **Jämkning 2026 e-service only**. Numbering on the paper form **SKV 4302 is UNVERIFIED** and must not be assumed to match.

## Tjänst — income and tax so far

- `02 Lön och förmåner` → employment income and benefits so far this year.
- `01 Avdragen skatt` → preliminary tax withheld so far this year.

Use actual YTD values from **Mina inkomstuppgifter → Sammanställning inkomstuppgifter** and reconcile the reporting cutoff with payroll.

## Tjänst — income for the rest of the year

- `07 Lön och förmåner` → employment income and benefits for the rest of the year, including future taxable RSU vestings.
- `96 – varav från huvudutbetalare` → portion from the main payer.
- `94 – inkomster från ruta 07, 08 och 14 som det inte dras skatt från` → income in those boxes from which no tax is withheld.

Use only income expected for the remainder of the year. Do not repeat amounts already included in YTD actuals.

## Kapital

- `39 Schablonintäkt investeringssparkonto`
- `34 Ränteutgifter för lån med säkerhet m.m.`

For box 39, follow the open-item handling in [2026-tax-constants.md](2026-tax-constants.md): enter **gross schablonintäkt** until the e-service treatment of the ISK tax-free deduction is verified.

### OPEN — ISK fribelopp handling

It is **UNVERIFIED** whether the jämkning e-service applies the ISK tax-free deduction automatically.

Until resolved, add a concise note in `Övriga upplysningar` stating that:

- box 39 contains gross schablonintäkt;
- automatic application of the ISK tax-free deduction in the jämkning e-service is unverified;
- the resulting estimate may conservatively overstate capital tax by up to **30% of the applicable deduction**.

Generic wording, with no personal application data:

```text
Box 39 contains gross ISK schablonintäkt. It is unverified whether the jämkning e-service applies the ISK tax-free deduction automatically, so the estimate may conservatively overstate capital tax by up to 30% of the applicable deduction.
```

Do not manually subtract the deduction from box 39 unless the e-service behavior is verified.

## Underlag för skattereduktion

- `76 Gåva`
- `75 Avgift till a-kassa`

Enter only when applicable and eligible. Eligibility rules and any tax-year-specific thresholds not listed in [2026-tax-constants.md](2026-tax-constants.md) remain **UNVERIFIED**.

## Övriga upplysningar

Use free text for material assumptions not obvious from the numeric entries, such as:

- a future RSU vest included in remaining employment income;
- an estimated ISK amount;
- the open ISK-fribelopp handling described above.

Keep wording short, factual, and free of unnecessary personal detail.

## Maintenance rule

Do not use these field codes for another tax year without checking that year's live e-service. For paper **SKV 4302**, field numbering remains **UNVERIFIED** until separately sourced.

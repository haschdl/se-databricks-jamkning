# Tax year 2026 — tax constants and timing

**Tax year:** 2026  
**Verified:** 2026-11-03

This file is the only source in this skill for 2026-specific rates, thresholds, and deadlines. Do not reuse any value in this file for another tax year without re-verifying it against current Skatteverket guidance.

## Sources

- Skatteverket — Engångsbelopp: https://www.skatteverket.se/foretag/arbetsgivare/arbetsgivaravgifterochskatteavdrag/skatteavdrag/engangsbelopp.4.361dc8c15312eff6fd3225e.html
- Skatteverket — Ränteavdrag: https://www.skatteverket.se/ranteavdrag
- Skatteverket — Investeringssparkonto: https://www.skatteverket.se/investeringssparkonto
- Skatteverket — Jämkning: https://www.skatteverket.se/jamkning
- Skatteverket — Statlig inkomstskatt 2026: https://www.skatteverket.se/funktioner/svarpavanligafragor/privat/beloppochprocent/privatbeloppfaq/narskamanbetalastatliginkomstskattochhurhogarden.5.10010ec103545f243e8000166.html

## Engångsskatt

For **column 1**, main income, employee under 66:

- `54%` applies when **årslön is above SEK 660,000**.
- **Årslön** for the engångsskatt table means total expected gross income from the **main employer** for the whole year, including taxable benefits, the current one-time payment, and earlier one-time payments during the year.
- The engångsskatt table applies only to the **main employer**.
- A **secondary employer must withhold 30%** on a one-time payment.
- If a jämkning decision sets a percentage, that percentage also applies to one-time payments.

Source: Skatteverket — Engångsbelopp.

## Marginal-rate comparison for RSU diagnostics

- The 2026 state income-tax marginal component is **20%** on taxable earned income above the applicable state-tax threshold.
- The threshold itself remains **UNVERIFIED in this skill** and must not be inferred from memory.
- Municipal tax, church fee, and burial fee vary by employee/location and are not constants in this file; obtain the applicable values rather than assuming them.

Source for the state income-tax rate: Skatteverket — Statlig inkomstskatt 2026.

When comparing RSU engångsskatt with estimated marginal tax, use the applicable municipal rate + applicable church fee + burial fee + the state income-tax component where applicable. Do not compare engångsskatt with the employee's average/effective tax rate.

## Interest deductions and capital-deficit reduction

- From income year **2026**, interest on **unsecured loans is not deductible**.
- Capital-deficit tax reduction is **30% up to SEK 100,000** of deficit and **21% above SEK 100,000**.

Source: Skatteverket — Ränteavdrag.

Normal BRF monthly fees are not mortgage interest.

## ISK

For income year 2026:

- schablonintäkt: **3.55% of the capital base**;
- tax-free base: **SEK 300,000**, shared across ISK, KF and PEPP;
- in the ordinary tax return, gross schablonintäkt is reported and the tax-free deduction is applied separately;
- maximum tax-free deduction for 2026: **SEK 10,650**.

Source: Skatteverket — Investeringssparkonto.

### OPEN — jämkning e-service treatment of the ISK tax-free deduction

It is **UNVERIFIED** whether the jämkning e-service applies the ISK tax-free deduction automatically when gross schablonintäkt is entered.

Until this is resolved:

1. enter the **gross schablonintäkt** requested by the e-service;
2. disclose in `Övriga upplysningar` that the e-service treatment of the ISK tax-free deduction is unverified;
3. note that this may conservatively overstate capital tax by up to **30% of the applicable deduction**.

Do not manually net the tax-free deduction against box 39 unless the e-service behavior is verified.

### ISK capital-base inputs

For an exact capital-base calculation, collect the statutory quarterly values and relevant deposits/transfers. The exact capital-base mechanics beyond the verified values above remain **UNVERIFIED** in this reference and must be checked against current Skatteverket guidance before being relied upon.

## Jämkning deadlines

- Current-year applications: **January through 30 November**.
- Next-year applications open **1 October**.
- File by **9 November** for a next-year decision to apply from January.

Source: Skatteverket — Jämkning.

If the main benefit is expected from an early-next-year RSU vest, evaluate the next-year application during the preceding October–November window.

## Values still UNVERIFIED in this reference

The following were not supplied as verified 2026 values and must not be inferred from memory:

- state income-tax threshold;
- municipal tax rate for any municipality;
- church fee rate;
- burial fee rate;
- public-service fee amount/cap;
- grundavdrag amounts/formula;
- jobbskatteavdrag amounts/formula;
- gift eligibility thresholds/rules beyond verified e-service field mapping;
- a-kassa eligibility/rate rules beyond verified e-service field mapping;
- fyllnadsinbetalning dates and interest rules;
- any tax-year-specific value not explicitly listed above.

## Maintenance rule

Before using this skill for any tax year other than 2026:

1. treat every rate, threshold, deadline, deduction amount, and tax-year rule in this file as unavailable;
2. verify the new-year values against Skatteverket;
3. create or update the year-specific reference;
4. do not copy 2026 values forward by assumption.

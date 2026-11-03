# Skatteverket jämkning form fields

This reference centralizes field mappings that were previously embedded in `SKILL.md`.

Some field numbers still require independent verification against the current SKV 4302 form/e-service. Issue #4 owns that verification. Prefer current labels in the live e-service over remembered field numbers.

## Tjänst — income and tax so far

Original mapping:

- `02 Lön och förmåner` → YTD total employment income.
- `01 Avdragen skatt` → YTD preliminary tax withheld.

Use actual YTD values from **Mina inkomstuppgifter → Sammanställning inkomstuppgifter**.

Other fields should be zero/blank unless they genuinely apply.

## Tjänst — income for the rest of the year

Original mapping:

- `07 Lön och förmåner` → remaining taxable employment income, including future RSU vestings.
- `96 varav från huvudutbetalare` → amount expected from the main payer.
- `94 ... som det inte dras skatt från` → income for which no withholding is made.

Use only the remainder of the year. Do not repeat income already included in YTD actuals.

For a Databricks employee with no other main payer, the amount attributed to the main payer will often equal the Databricks portion of remaining employment income.

## Tjänst — deductions/reductions

Original mapping:

- `76 Gåva`
- `75 Avgift till a-kassa`

Enter only if applicable and eligible for the current tax year.

## Kapital

Original mapping:

- `39 Schablonintäkt investeringssparkonto`
- `34 Ränteutgifter för lån med säkerhet m.m.`

Issue #4 must verify both field mappings and the e-service treatment of the ISK tax-free allowance.

Do not enter BRF monthly fees as mortgage interest.

## Övriga upplysningar

Use this only for material assumptions not obvious from the numeric entries, for example:

- a large future RSU vest included in remaining employment income;
- an ISK schablonintäkt that is estimated because final annual broker data is unavailable.

Keep free text short, factual, and free of unnecessary personal detail.

## Verification target

Issue #4 should independently verify at least:

- 01 Avdragen skatt;
- 02 Lön och förmåner hittills;
- 07 Lön och förmåner resten av året;
- 94 income with no withholding;
- 96 main payer;
- 34 secured-loan interest;
- 39 ISK schablonintäkt;
- 75 a-kassa;
- 76 gift.

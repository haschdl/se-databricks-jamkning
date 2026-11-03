# 2026 tax constants and timing

This file contains tax-year-specific values that were present in the original skill. They must be re-verified as part of issue #4 before being treated as authoritative.

## Engångsskatt

The original skill states that, for 2026, column 1 of the engångsskatt table reaches **54%** above the relevant annual-income threshold.

Official source to verify:
https://www.skatteverket.se/foretag/arbetsgivare/arbetsgivaravgifterochskatteavdrag/skatteavdrag/engangsbelopp.4.361dc8c15312eff6fd3225e.html

Detailed årslön and main/secondary-payer corrections are tracked in issue #1.

## ISK

The original skill uses these 2026 values:

- schablonintäkt rate: **3.55%** of capital base;
- tax-free base: **SEK 300,000** across eligible ISK/KF/PEPP savings;
- capital tax rate: **30%**.

Official source:
https://www.skatteverket.se/investeringssparkonto

For an exact ISK capital base, collect:

- account value on 1 January;
- account value on 1 April;
- account value on 1 July;
- account value on 1 October;
- relevant deposits/transfers during the year.

The original workflow calculates the capital base from quarterly opening values plus relevant deposits/transfers, divided by four.

If exact data is unavailable, use a conservative estimate and disclose that it is an estimate. Do not present a current portfolio snapshot as an exact statutory capital base.

Issue #4 must verify how the jämkning e-service handles the ISK tax-free allowance before the application guidance is considered final.

## Interest deductions

The original skill distinguishes secured and unsecured borrowing. The 2026 unsecured-loan rule requires correction/clarification under issue #2 and should not be inferred from this file until that issue is implemented.

Normal BRF monthly fees are not mortgage interest.

## Deadlines

Year-specific jämkning application timing is intentionally centralized here. Exact 2026/2027 dates are to be verified under issues #3 and #4 before being added as authoritative constants.

## Maintenance rule

For every new tax year, verify at minimum:

- engångsskatt thresholds and rates;
- state income-tax threshold;
- ISK schablon rate;
- ISK tax-free base;
- interest-deduction rules;
- gift and a-kassa rules;
- jämkning application deadlines.

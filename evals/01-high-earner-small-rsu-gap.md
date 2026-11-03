# Eval 01 — High earner, RSU vest, small withholding gap

All employee figures below are synthetic.

## Prompt

I work for Databricks Sweden and live in Stockholm. Databricks is my main employer. My expected full-year main-employer income is comfortably above the highest engångsskatt threshold in the skill's current-year reference.

I have an upcoming RSU vest with a taxable SEK value of 500,000.

For this synthetic case, use these supplied marginal components rather than looking up Stockholm rates:

- municipal income-tax component: 32.10%
- burial fee: 0.10%
- church fee: 0%
- state income-tax component: use the verified current-year value from the skill reference

Payroll will apply the verified high-band engångsskatt rate from the current-year reference to the vest.

Should I apply for jämkning because the RSU tax withholding is obviously too high?

## Expected data requests

The skill should confirm or request:

- Databricks is the main payer;
- payroll årslön / expected main-employer årslön;
- taxable RSU vest value;
- applicable engångsskatt band from the current-year reference;
- supplied municipal/church/burial components;
- other deductions/capital items before deciding whether jämkning is worthwhile.

## Expected calculation / diagnostic

Using the supplied synthetic local components plus the verified state component, the estimated marginal employment-tax rate is approximately 52.20%.

Using the verified high-band engångsskatt rate, the withholding gap is approximately 1.80 percentage points.

The skill should say that this is a **small gap**, not evidence that the RSU is obviously or materially over-withheld. It should then assess whether mortgage interest, other deductions, capital events, prior withholding, or other facts create a larger full-year mismatch.

## Expected recommendation direction

- Do **not** recommend jämkning merely because the RSU uses the high engångsskatt band.
- Quantify the gap against marginal tax.
- Explain that jämkning may still be useful if the full-year model shows material over-withholding for other reasons.

## Failure conditions

Fail this eval if the skill:

- compares the engångsskatt rate with average/effective tax instead of marginal tax;
- states that high-band RSU withholding is automatically excessive;
- omits the årslön/main-payer check;
- recommends reducing withholding without considering the full-year tax position.

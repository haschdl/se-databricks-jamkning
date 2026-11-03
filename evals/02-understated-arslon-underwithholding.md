# Eval 02 — Payroll årslön understated, RSU under-withheld

All employee figures below are synthetic.

## Prompt

I work for Databricks Sweden and Databricks is my main employer.

Payroll appears to have used an årslön of 640,000 SEK when calculating tax on an RSU vest. The vest had a taxable value of 200,000 SEK and payroll withheld 34%.

My expected full-year Databricks compensation is actually:

- regular cash salary: 600,000 SEK
- taxable benefits: 40,000 SEK
- commissions: 120,000 SEK
- RSU vest already processed: 200,000 SEK
- another known RSU vest later this year: 250,000 SEK

For this synthetic case, assume:

- municipal income-tax component: 31.70%
- burial fee: 0.10%
- church fee: 0%
- state income-tax component: use the verified current-year value from the skill reference

I thought jämkning was mainly for lowering my RSU withholding. Should I apply?

## Expected data requests

The skill should request/confirm:

- payroll årslön basis and whether the 640,000 figure is documented or inferred;
- all current-year main-employer salary, benefits, commission, and one-time payments;
- future known vestings;
- tax already withheld YTD;
- deductions/capital items before recommending a final action.

## Expected calculation / diagnostic

Expected main-employer årslön from the supplied facts is 1,210,000 SEK.

The skill should detect that payroll's 640,000 SEK årslön omitted material compensation that belongs in the annual estimate.

The supplied marginal components imply an estimated marginal employment-tax rate of approximately 51.80% once the state component applies. A 34% withholding rate on the vest is therefore a potential **under-withholding** signal, not an over-withholding signal.

The exact final tax must still be obtained from Skatteverket or a fully itemized model.

## Expected recommendation direction

- Flag **KVARSKATT RISK**.
- Do not frame jämkning primarily as a way to lower withholding.
- Consider an updated jämkning that corrects withholding, additional payroll withholding if operationally available, or fyllnadsinbetalning after checking current rules.

## Failure conditions

Fail this eval if the skill:

- treats base salary or payroll's stated årslön as authoritative without reconciling future vestings/commission/benefits;
- recommends lower withholding because RSUs are assumed to be over-taxed;
- fails to identify the 34% vest withholding as a possible under-withholding problem;
- gives fyllnadsinbetalning dates or interest rules from memory.

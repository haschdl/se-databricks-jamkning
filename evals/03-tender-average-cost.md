# Eval 03 — Tender offer after multiple RSU vests

All employee figures below are synthetic.

## Prompt

I work for Databricks Sweden and participated in an employee tender offer. All shares below are the same kind/class, and I had no prior sales.

My holdings before the tender came from three RSU vests:

- Vest A: 100 shares, SEK acquisition value at vest = 100 SEK/share
- Vest B: 100 shares, SEK acquisition value at vest = 200 SEK/share
- Vest C: 100 shares, SEK acquisition value at vest = 300 SEK/share

I sold 150 shares in the tender at 400 SEK/share. Assume no sale fees for this eval and no tax was withheld on the tender capital gain.

I selected shares from Vest A and Vest B in the tender portal. What capital gain should I include when evaluating jämkning?

## Expected data requests

The skill should confirm/request:

- all same-kind share acquisitions, not only the lots selected in the tender portal;
- SEK acquisition value for every vest;
- number of same-kind shares held immediately before sale;
- prior sales or transactions affecting the average-cost pool;
- SEK sale proceeds and fees;
- evidence of any tax separately paid on the capital gain.

## Expected calculation / diagnostic

The skill must use `genomsnittsmetoden`, not the selected tender lots.

Synthetic calculation:

```text
Total acquisition cost
= (100 × 100) + (100 × 200) + (100 × 300)
= 60,000 SEK

Total shares before sale
= 300

Average cost per share
= 60,000 / 300
= 200 SEK/share

Allocated cost basis for 150 shares sold
= 150 × 200
= 30,000 SEK

Tender proceeds
= 150 × 400
= 60,000 SEK

Capital gain
= 60,000 - 30,000
= 30,000 SEK
```

The fact that the tender portal selected Vest A and Vest B must **not** change the cost-basis calculation.

## Expected recommendation direction

- Include the 30,000 SEK gain in the capital model.
- Treat payroll withholding on the capital gain as zero in this supplied scenario.
- Flag the gain as a possible `kvarskatt` driver.
- Re-evaluate whether reducing payroll withholding through jämkning is still appropriate after adding the gain.

## Failure conditions

Fail this eval if the skill:

- uses FIFO or a per-lot basis from Vest A/Vest B;
- ignores Vest C when computing the average cost;
- treats the tender gain as employment income;
- assumes RSU/engångsskatt withholding covers the tender capital gain;
- omits the resulting kvarskatt risk.

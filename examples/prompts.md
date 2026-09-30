# Example agent prompts

Copy-paste these into Cursor or Claude after installing the LeaseIQ Pro MCP server.

## Equipment lease quote

```
Using LeaseIQ Pro, structure a commercial equipment lease:
- Cost: $175,000
- Term: 48 months
- Rate: 8.25%
- Residual: 15% FMV
- Payments in advance
- Monthly tax at 7%

Return the periodic payment, total of payments, and total interest.
```

## Buried yield / reverse solve

```
A vendor quoted $3,120/month for 60 months on $160,000 of equipment with a $16,000 residual.
Use LeaseIQ Pro to solve for the implied interest rate (yield). Explain the result in plain language for a broker.
```

## Skip / seasonal structure

```
Build a 36-month equipment lease on $90,000 at 9% that skips payments in January and February every year (seasonal shutdown). Use LeaseIQ Pro and summarize the payment in active months vs skip months.
```

## Full amortization + payoff

```
Generate the amortization schedule for a $120,000 / 60-month / 7% level lease with 10% residual. What is the remaining balance after payment 18?
```

## Auto lease forensic check

```
Verify this dealer lease with LeaseIQ Pro:
- MSRP $48,500, selling price $46,200
- 36 months, residual 57%, money factor 0.00195
- Acquisition fee $895, doc fee $350
- Tax 6.25% monthly, capitalize fees
- 3 MSDs

Is the money factor aggressive? What is drive-off and lease-end buyout?
```

## Lease vs buy

```
Compare lease vs buy for a $39,000 vehicle:
- Lease: 36 mo, residual 55%, APR 4.8% (convert to money factor), tax 7% monthly, $1,500 down
- Buy: 60 mo loan at 5.9%, $1,500 down

Use calculate_auto_lease and calculate_auto_loan, then summarize which is cheaper over 36 months assuming the buyer keeps the car.
```

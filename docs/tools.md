# LeaseIQ Pro tool reference

All tools are deterministic. Prefer them over model arithmetic for compounding interest, iterative solves, and amortization.

## `calculate_lease`

Commercial equipment lease / finance payment.

**Required:** `equipmentCost`, `leaseTerm`, `interestRate`

**Common optional:** `residualValue` or `residualPercent`, `paymentFrequency`, `paymentTiming` (`Arrears` | `Advance`), `taxMethod`, `taxRate`, `downPayment`, `paymentStructure` (`Level` | `Balloon` | `Step` | `Custom` | `InterestOnly` | `Skip`), structure-specific fields (`balloonPercentage`, `steps`, `skipMonths`, `interestOnlyTerm`), `fundingDate` / `leaseStartDate` for interim rent.

**Example prompt:** “60-month lease on $150k equipment at 7.5%, 10% residual, payments in advance.”

## `solve_lease_structure`

Newton-Raphson reverse solver.

**Required:** `solveFor`, `targetPayment`

**`solveFor`:** `Rate` | `Term` | `ResidualValue` | `PresentValue` | `Commission`

Omit the unknown you are solving for; provide the known deal inputs.

**Example prompt:** “Quoted payment is $2,850 on a $150k / 60-month deal with 10% residual. What yield am I really paying?”

## `generate_amortization_schedule`

Period-by-period schedule: payment, interest, principal, balance, totals.

Accepts the same structure inputs as `calculate_lease`.

**Example prompt:** “Give me the full amortization table and the payoff after month 24.”

## `calculate_auto_lease`

Captive-lender style consumer lease math.

**Required:** `msrp`, `term`, `residualPercent`

**Provide either** `moneyFactor` **or** `apr` (APR converts as `apr / 2400`).

**Optional:** `sellingPrice`, `mileage`, `taxRate`, `taxMethod` (`Monthly` | `SellingPrice` | `TotalLease` | `TAVT` | `Exempt`), fees, `capitalizeFees`, `isZeroDriveOff`, `msdCount`.

**Example prompt:** “36-month lease, MSRP $45k, selling $42.5k, residual 58%, money factor 0.00225, 6.625% monthly tax, capitalize fees, 2 MSDs.”

## `calculate_auto_loan`

Financed or cash purchase for lease-vs-buy.

**Required:** `vehiclePrice`

**For finance:** `term`, `rate`, plus optional `downPayment`, `tradeInCredit`, `salesTax`.

**Example prompt:** “Same car as a 60-month loan at 5.9% with $2k down — compare to the lease.”

## Agent tips

1. Always pass units the schema expects (rates as percentages like `7.5`, money factor like `0.00225`).
2. On validation errors, read the field name and accepted range, then retry once.
3. Return the tool’s normalized inputs to the user when they need an audit trail.
4. For broker “what rate is buried in this quote?” workflows, chain `calculate_lease` → `solve_lease_structure`.

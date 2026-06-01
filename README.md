# student-cme-ctd-repo-shock

Simple, honest, useful educational spreadsheet for modeling how repo shocks impact Treasury futures fair value, invoice price, and basis relationships through CTD financing mechanics.

---

# Purpose

This project demonstrates a core rates market structure idea:

> Treasury futures are financing-sensitive forward delivery instruments.

The model isolates how repo changes propagate into:

* CTD financing cost
* Fair value Treasury futures pricing
* Invoice price
* Gross basis
* Net basis

The goal is not prediction or alpha generation.

The goal is conceptual mastery of:

* financing,
* carry,
* delivery economics,
* and basis mechanics.

---

# Inputs

| Input              | Description                        |
| ------------------ | ---------------------------------- |
| Repo Rate          | Current repo financing rate        |
| Repo Shock         | Shock applied to repo              |
| CTD Par            | Face value of CTD bond             |
| CTD Clean          | Current clean price of CTD         |
| CTD Coupon Rate    | Annual coupon rate                 |
| CTD Frequency      | Coupon payments per year           |
| CTD Days In Period | Days in coupon period              |
| CTD Days Since     | Days since last coupon             |
| Futures CF         | Treasury futures conversion factor |
| Days To Delivery   | Days until futures delivery        |

---

# Outputs

| Output                     | Description                               |
| -------------------------- | ----------------------------------------- |
| Futures FRV Repo           | Fair value futures price at current repo  |
| Futures FRV Shock          | Fair value futures price after repo shock |
| Change In FV               | Repo-driven futures repricing             |
| Projected Accrued Interest | Accrued interest projected to delivery    |
| Invoice At Repo            | Invoice price at current repo             |
| Invoice At Shock           | Invoice price after repo shock            |
| Change In Invoice          | Repo-driven invoice repricing             |
| CTD Accrued Interest       | Current accrued interest                  |
| CTD Dirty Price            | Clean + accrued                           |
| Gross Basis At Repo        | Gross basis at current repo               |
| Gross Basis At Shock       | Gross basis after repo shock              |
| Change In Gross            | Change in gross basis                     |
| Net Basis At Repo          | Net basis at current repo                 |
| Net Basis At Shock         | Net basis after repo shock                |
| Change In Net              | Change in net basis                       |
| Financing Cost Repo        | Current repo financing cost               |
| Financing Cost Shock       | Financing cost after repo shock           |
| Change In Financing Cost   | Repo-driven financing increase            |

---

# Core Concepts

The project separates Treasury futures pricing into:

## Stable Carry Component

Projected accrued interest.

## Dynamic Financing Component

Repo-sensitive futures fair value.

This allows users to visualize how funding stress propagates through Treasury futures delivery economics.

---

# Formulas

---

## Coupon Payment

```text
CouponPayment =
(CouponRate × Par) / Frequency
```

---

## Current Accrued Interest

```text
CTDAccruedInterest =
(DaysSince / DaysInPeriod)
×
CouponPayment
```

---

## CTD Dirty Price

```text
CTDDirty =
CTDClean + CTDAccruedInterest
```

---

## Projected Accrued Interest

```text
ProjectedAI =
((DaysSince + DaysToDelivery) / DaysInPeriod)
×
CouponPayment
```

---

## Repo Financing Cost

```text
FinancingCostRepo =
CTDDirty
×
Repo
×
(DaysToDelivery / 360)
```

---

## Shocked Repo Financing Cost

```text
FinancingCostShock =
CTDDirty
×
(Repo + RepoShock)
×
(DaysToDelivery / 360)
```

---

## Change In Financing Cost

```text
ChangeInFinancing =
FinancingCostShock - FinancingCostRepo
```

---

# Fair Value Treasury Futures Formula

The model assumes Treasury futures represent a forward financed delivery relationship.

Projected accrued interest is removed from futures fair value because accrued coupon carry offsets financing economics during the delivery horizon.

```text
FuturesFRV =
(
CTDDirty
+
FinancingCost
-
ProjectedAI
)
/
FuturesCF
```

---

## Futures FRV At Current Repo

```text
FuturesFRVRepo =
(
CTDDirty
+
FinancingCostRepo
-
ProjectedAI
)
/
FuturesCF
```

---

## Futures FRV At Shocked Repo

```text
FuturesFRVShock =
(
CTDDirty
+
FinancingCostShock
-
ProjectedAI
)
/
FuturesCF
```

---

## Change In Futures FRV

```text
ChangeInFV =
FuturesFRVShock - FuturesFRVRepo
```

---

# Invoice Mechanics

Invoice price is the converted futures value plus projected accrued interest reimbursement paid to the deliverer.

```text
Invoice =
(Converted Futures)
+
ProjectedAI
```

---

## Converted Futures

```text
ConvertedFutures =
FuturesFRV × FuturesCF
```

---

## Invoice At Repo

```text
InvoiceAtRepo =
(FuturesFRVRepo × FuturesCF)
+
ProjectedAI
```

---

## Invoice At Shock

```text
InvoiceAtShock =
(FuturesFRVShock × FuturesCF)
+
ProjectedAI
```

---

## Change In Invoice

```text
ChangeInInvoice =
InvoiceAtShock - InvoiceAtRepo
```

---

# Gross Basis

Gross basis compares CTD dirty price to converted futures value.

```text
GrossBasis =
CTDDirty - ConvertedFutures
```

---

## Gross Basis At Repo

```text
GrossBasisAtRepo =
CTDDirty - (FuturesFRVRepo × FuturesCF)
```

---

## Gross Basis At Shock

```text
GrossBasisAtShock =
CTDDirty - (FuturesFRVShock × FuturesCF)
```

---

## Change In Gross Basis

```text
ChangeInGross =
GrossBasisAtShock - GrossBasisAtRepo
```

---

# Net Basis

Net basis compares CTD dirty price to full invoice value.

```text
NetBasis =
CTDDirty - Invoice
```

---

## Net Basis At Repo

```text
NetBasisAtRepo =
CTDDirty - InvoiceAtRepo
```

---

## Net Basis At Shock

```text
NetBasisAtShock =
CTDDirty - InvoiceAtShock
```

---

## Change In Net Basis

```text
ChangeInNet =
NetBasisAtShock - NetBasisAtRepo
```

---

# Educational Insight

The project demonstrates a key institutional relationship:

```text
Repo ↑
→ Financing Cost ↑
→ Futures Fair Value ↑
→ Invoice ↑
→ Basis Relationship Changes
```

This highlights how Treasury basis trades are fundamentally financing-sensitive balance sheet structures, not just directional rate trades.

---

# Disclaimer

Educational student project only.

This model simplifies real-world Treasury futures delivery mechanics and should not be used for trading or risk management decisions.

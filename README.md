# student-cme-ctd-repo-shock

Simple, honest, useful educational spreadsheet for modeling how repo shocks impact Treasury futures fair value, invoice price, and basis relationships through CTD financing mechanics.

---

# PURPOSE

This project demonstrates a core rates market structure concept:

> Treasury futures are financing-sensitive forward delivery instruments.

The model isolates how repo changes propagate into:

* CTD financing cost
* Treasury futures fair value
* Invoice price
* Gross basis
* Net basis

The goal is conceptual understanding of:

* financing,
* carry,
* delivery economics,
* and Treasury basis mechanics.

---

# INPUTS

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

# OUTPUTS

| Output                     | Description                            |
| -------------------------- | -------------------------------------- |
| Futures FRV Repo           | Fair value futures at current repo     |
| Futures FRV Shock          | Fair value futures after repo shock    |
| Change In FV               | Repo-driven futures repricing          |
| Projected Accrued Interest | Accrued interest projected to delivery |
| Invoice At Repo            | Invoice value at current repo          |
| Invoice At Shock           | Invoice value after repo shock         |
| Change In Invoice          | Repo-driven invoice repricing          |
| CTD Accrued Interest       | Current accrued interest               |
| CTD Dirty Price            | Clean + accrued                        |
| Gross Basis Repo           | Gross basis at current repo            |
| Gross Basis Shock          | Gross basis after repo shock           |
| Change In Gross            | Change in gross basis                  |
| Net Basis Repo             | Net basis at current repo              |
| Net Basis Shock            | Net basis after repo shock             |
| Change In Net              | Change in net basis                    |
| Financing Cost Repo        | Repo financing cost                    |
| Financing Cost Shock       | Shocked financing cost                 |
| Change In Financing        | Repo-driven financing increase         |

---

# CORE FORMULAS

## Coupon Payment

```text
CouponPay = (CouponRate × Par) / Frequency
```

---

## Current Accrued Interest

```text
CTDAccInt =
(DaysSince / DaysInPeriod)
× CouponPay
```

---

## CTD Dirty Price

```text
CTDDirty =
CTDClean + CTDAccInt
```

---

## Projected Accrued Interest

```text
ProjAccInt =
((DaysSince + DaysToDelivery) / DaysInPeriod)
× CouponPay
```

---

# FINANCING

## Repo Financing Cost

```text
FinancingCostRepo =
CTDDirty × Repo × (DaysToDelivery / 360)
```

---

## Shocked Financing Cost

```text
FinancingCostShock =
CTDDirty × (Repo + RepoShock)
× (DaysToDelivery / 360)
```

---

## Change In Financing

```text
ChangeInFinancing =
FinancingCostShock - FinancingCostRepo
```

---

# FAIR VALUE FUTURES

Treasury futures fair value represents the financed forward delivery economics of the CTD bond.

Projected accrued interest is removed from futures fair value because coupon carry offsets part of financing cost during the delivery horizon.

## Futures FRV At Repo

```text
FuturesFRVRepo =
(
CTDDirty
+ FinancingCostRepo
- ProjAccInt
)
/
FuturesCF
```

---

## Futures FRV At Shock

```text
FuturesFRVShock =
(
CTDDirty
+ FinancingCostShock
- ProjAccInt
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

# CONVERTED FUTURES

## Converted Futures At Repo

```text
ConvertedRepo =
FuturesFRVRepo × FuturesCF
```

---

## Converted Futures At Shock

```text
ConvertedShock =
FuturesFRVShock × FuturesCF
```

---

# INVOICE MECHANICS

Invoice price equals converted futures value plus projected accrued interest reimbursement paid to the deliverer.

## Invoice At Repo

```text
InvoiceRepo =
ConvertedRepo + ProjAccInt
```

---

## Invoice At Shock

```text
InvoiceShock =
ConvertedShock + ProjAccInt
```

---

## Change In Invoice

```text
ChangeInInvoice =
InvoiceShock - InvoiceRepo
```

---

# GROSS BASIS

Gross basis compares CTD dirty price to converted futures value.

## Gross Basis At Repo

```text
GrossBasisRepo =
CTDDirty - ConvertedRepo
```

---

## Gross Basis At Shock

```text
GrossBasisShock =
CTDDirty - ConvertedShock
```

---

## Change In Gross Basis

```text
ChangeInGross =
GrossBasisShock - GrossBasisRepo
```

---

# NET BASIS

Net basis compares CTD dirty price to full invoice value.

## Net Basis At Repo

```text
NetBasisRepo =
CTDDirty - InvoiceRepo
```

---

## Net Basis At Shock

```text
NetBasisShock =
CTDDirty - InvoiceShock
```

---

## Change In Net Basis

```text
ChangeInNet =
NetBasisShock - NetBasisRepo
```

---

# CORE INSIGHT

The project demonstrates the financing transmission mechanism inside Treasury futures delivery economics:

```text
Repo ↑
→ Financing Cost ↑
→ Futures FRV ↑
→ Converted Futures ↑
→ Invoice ↑
→ Basis Relationship Changes
```

This highlights how Treasury basis structures are highly sensitive to financing conditions and repo market stress.

---

# DISCLAIMER

Educational student project only.

This model simplifies real-world Treasury futures delivery mechanics and should not be used for trading or risk management decisions.

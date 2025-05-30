# 🏠 Housing Market Index — Methodology and Data Sources

To calculate a **Housing Market Index** (HMI) for each metropolitan area using data from [data.census.gov](https://data.census.gov), we compile a **composite score** capturing affordability, supply-demand balance, cost burdens, and new construction. Below is the full breakdown of **data sources**, **engineered features**, and the **composite index formula**.

---

## 📊 Source Tables from data.census.gov

| **Indicator**               | **Table Code / Name**                                              | **Use**                                      |
| --------------------------- | ------------------------------------------------------------------ | -------------------------------------------- |
| **Homeownership & Rent**    | B25003 – *Tenure*                                                  | Determines % Owner vs Renter occupied        |
| **Median Home Value**       | B25077 – *Median Value (Dollars) for Owner-Occupied Housing Units* | Home price for affordability ratio           |
| **Median Household Income** | B19013 – *Median Household Income in the Past 12 Months*           | Used in affordability and burden ratios      |
| **Gross Rent Burden**       | B25070 – *Gross Rent as % of Household Income*                     | Renters paying >30% or >50% of income        |
| **Owner Cost Burden**       | B25091 – *Monthly Owner Costs as % of Income*                      | Owners paying >30% of income on housing      |
| **Vacancy Status**          | B25002 – *Occupancy Status*                                        | Vacant vs occupied units                     |
| **Housing Units Total**     | B25001 – *Total Housing Units*                                     | Baseline denominator for many ratios         |
| **Year Structure Built**    | B25036 – *Year Built by Tenure*                                    | Used to calculate share of new units         |
| **Population Estimate**     | PEPANNRES (via Population Estimates Program)                       | Used for per capita / per unit normalization |

---

## 🛠️ Features Engineered for HMI

| **Feature**           | **Formula**                                                                       | **Description**                          |
| --------------------- | --------------------------------------------------------------------------------- | ---------------------------------------- |
| `Affordability_Ratio` | `Median_Home_Value / MedianHouseholdIncome`                                       | High = less affordable                   |
| `Vacancy_Rate`        | `Vacant / Total_Housing_Units`                                                    | Share of unoccupied housing              |
| `Rent_Burden_Rate`    | `High_Rent_Burden / Renter occupied` *(if `High_Rent_Burden` is not already a %)* | % renters burdened                       |
| `Owner_Burden_Rate`   | `Owner_Cost_Burden / Owner occupied` *(if not already a %)*                       | % homeowners burdened                    |
| `New_Units_Rate`      | `New_Housing_Units / Total_Housing_Units`                                         | Share of recently built housing stock    |
| `Pop_to_Unit_Ratio`   | `Population / Total_Housing_Units`                                                | High = higher pressure on housing supply |
| `Recent_Units_Share`  | Already calculated from B25036                                                    | % of housing built in 2010 or later      |

---

## 🧮 Composite Housing Market Index Formula

The **Housing Market Index** is a weighted average of standardized (z-score) features, structured as follows:

```python
Housing_Market_Index = (
    0.25 * Recent_Units_Share +
    0.20 * (New_Housing_Units / Total_Housing_Units) +
    0.20 * (MedianHouseholdIncome / Median_Home_Value) +
    0.15 * (1 - High_Rent_Burden) +
    0.10 * (1 - Owner_Cost_Burden) +
    0.10 * (1 - Vacancy_Rate)
)
```

* All percentages should be converted to decimals (e.g., 30% → 0.30)
* Inverses (`1 - ...`) are used so higher scores consistently indicate *better* housing conditions
* Optionally, scale each variable using z-scores for comparability before aggregation

---

## ✅ Notes

* This index balances **affordability**, **availability**, **housing stress**, and **supply growth**
* You can adjust weights to suit local policy emphasis (e.g., give more weight to affordability in high-cost cities)
* The result is a continuous index where **higher values indicate healthier housing markets**

---
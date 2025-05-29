Perfect — let’s now translate the **indicators** and **formula** into the **specific datasets** you’ll need to download from [data.census.gov](https://data.census.gov) or the Census Construction portal, to actually build your **Housing Market Index (HMI)** per metro area.

---

## ✅ Housing Market Index – Required Datasets

| **Index Component**                 | **Metric**                                   | **Dataset/Table to Use**                                                                                         | **Source**                |
| ----------------------------------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------- |
| **Homeownership Rate**              | % owner-occupied                             | `B25003` – *Tenure*                                                                                              | ACS (data.census.gov)     |
| **Affordability Ratio**             | Median Home Value / Median Income            | `B25077` – *Median Home Value*  <br> `B19013` – *Median Household Income*                                        | ACS                       |
| **Rent Burden**                     | Gross Rent as % of Household Income          | `B25070` – *Gross Rent as a % of Income*                                                                         | ACS                       |
| **Vacancy Rate**                    | % vacant units                               | `B25002` – *Occupancy Status*                                                                                    | ACS                       |
| **Construction Rate per Capita**    | New units / Total housing units / population | `C-40 / Building Permits Survey` <br> `B25001` – *Total Housing Units* <br> `PEPANNRES` – *Population Estimates* | Census BPS + ACS + PopEst |
| **Owner Cost Burden (Optional)**    | Monthly Owner Cost as % of Income            | `B25091` – *Owner Costs as % of Household Income*                                                                | ACS                       |
| **Rental Strain (Optional)**        | % of renters paying >30% of income on rent   | `B25070` (look for categories above 30% and 50%)                                                                 | ACS                       |
| **Units Built Recently (Optional)** | % of units built since 2010 or 2000          | `B25036` – *Year Structure Built*                                                                                | ACS                       |

---

## 🧮 Composite Index Formula Mapping

Here’s how your formula:

```python
HMI = avg(
  z(Homeownership Rate),
  -z(Median Home Value / Median Income),
  -z(Rent Burden),
  -z(Vacancy Rate),
  z(Construction Rate per 1,000 households)
)
```

Maps to **data sources**:

| **Formula Term**                        | **Derived From**                                                         |
| --------------------------------------- | ------------------------------------------------------------------------ |
| `z(Homeownership Rate)`                 | `B25003` – Owner / Total Occupied Housing Units                          |
| `-z(Median Home Value / Median Income)` | `B25077` / `B19013`                                                      |
| `-z(Rent Burden)`                       | `B25070` – % of households spending >30% of income on rent               |
| `-z(Vacancy Rate)`                      | `B25002` – Vacant / Total Housing Units                                  |
| `z(Construction Rate)`                  | (New Permits / Total Units or Pop) using `C-40` + `B25001` + `PEPANNRES` |

---

## 🔁 Time Period

* Use **ACS 1-Year** if you're working only with **large metro areas** (≥65,000 pop).
* Use **ACS 5-Year** if you want **coverage across all MSAs**, including small ones.
* Get **C-40 (Annual)** building permit data from [here](https://www.census.gov/construction/bps/).

---

## ✅ Final List of Tables to Collect

| **Table Code** | **Name**                                                                                                              |
| -------------- | --------------------------------------------------------------------------------------------------------------------- |
| B25003         | Tenure (Owner vs Renter Occupied)                                                                                     |
| B25077         | Median Home Value for Owner-Occupied Units                                                                            |
| B19013         | Median Household Income                                                                                               |
| B25070         | Gross Rent as % of Income                                                                                             |
| B25002         | Occupancy Status (to calculate vacancy rate)                                                                          |
| B25001         | Total Housing Units                                                                                                   |
| B25091         | Selected Monthly Owner Costs as % of Household Income (optional)                                                      |
| B25036         | Year Structure Built (optional)                                                                                       |
| PEPANNRES      | Annual Population Estimates (from Population Estimates Program)                                                       |
| C-40           | Annual Building Permits Survey ([https://www.census.gov/construction/bps/](https://www.census.gov/construction/bps/)) |

---

Would you like a Python script template that pulls these datasets (or reads them from CSVs) and builds the index step-by-step?

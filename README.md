# Housing_Market_Index
To calculate a **Housing Market Index** per metropolitan area using data from [data.census.gov](https://data.census.gov), you’ll need to compile a **composite index** from a variety of housing-related variables. Below is a structured table of relevant **topics and tables** you should search for, based on commonly used indicators in housing market health assessments (e.g., affordability, homeownership, construction, vacancy, rents, prices).

---

### 📊 Recommended Tables from data.census.gov for Housing Market Index (per metro area)

| **Indicator**                       | **Table Code / Name**                                                              | **Use**                                      |
| ----------------------------------- | ---------------------------------------------------------------------------------- | -------------------------------------------- |
| **Homeownership Rate**              | B25003 – *Tenure*                                                                  | % of owner-occupied vs renter-occupied units |
| **Median Home Value**               | B25077 – *Median Value (Dollars) for Owner-Occupied Housing Units*                 | Measures affordability & appreciation        |
| **Median Gross Rent**               | B25064 – *Median Gross Rent*                                                       | Rental affordability indicator               |
| **Housing Vacancy Rate**            | B25002 – *Occupancy Status* (calculate % vacant)                                   | Measures supply vs demand balance            |
| **Median Household Income**         | B19013 – *Median Household Income in the Past 12 Months*                           | Used to calculate affordability ratios       |
| **New Construction / Permits**      | Building Permits Survey (via Annual Construction Report) or C-40 (external to ACS) | Measure housing supply growth                |
| **Housing Units**                   | B25001 – *Total Housing Units*                                                     | Baseline housing supply                      |
| **Population Estimates**            | PEPTCOMP or PEPANNRES (via Population Estimates Program)                           | Normalize metrics per capita or household    |
| **Gross Rent as % of Income**       | B25070 – *Gross Rent as a Percentage of Household Income*                          | Captures rent burden                         |
| **Monthly Owner Costs as % Income** | B25091 – *Selected Monthly Owner Costs as a Percentage of Household Income*        | Captures ownership burden                    |
| **Units Built Recently**            | B25036 – *Year Structure Built*                                                    | Shows share of new construction              |

---

### Optional Enhancements:

| **Metric**                 | **Table**                            | **Purpose**                                       |
| -------------------------- | ------------------------------------ | ------------------------------------------------- |
| Cost-Burdened Renters      | B25070 (thresholds: >30%, >50%)      | High proportions indicate rental strain           |
| Foreclosure or Delinquency | External (FHFA, HUD, or Zillow)      | Not available directly via census, but often used |
| House Price Index          | FHFA House Price Index (metro-level) | Can be merged externally                          |

---

### Suggested Composite Index Formula Example

To create a **Housing Market Index**, you might standardize and average components such as:

```
HMI = avg(
  z(Homeownership Rate),
  -z(Median Home Value / Median Income),
  -z(Rent Burden),
  -z(Vacancy Rate),
  z(Construction Rate per 1,000 households)
)
```

Let me know if you'd like help building this in Python or joining these datasets!

To create a **Frozen Job Market Index** similar in structure to your **Housing Market Index**, you'll want to pull relevant labor market metrics from [data.census.gov](https://data.census.gov) and potentially supplement with BLS data. Below is a clear breakdown in the same style, mapping datasets, engineered features, and the composite formula.

---

# 🧊 Frozen Job Market Index — Methodology and Data Sources

To quantify **how frozen** a metropolitan job market is, we create a **composite index** that captures labor demand, employment flow, and labor force friction. A “frozen” market reflects low mobility, low hiring, high slack, and high distress.

---

## 📊 Source Tables from data.census.gov & BLS

| **Indicator**                    | **Table Code / Source**                         | **Use**                                         |
| -------------------------------- | ----------------------------------------------- | ----------------------------------------------- |
| **Employment Status**            | B23025 – *Employment Status for Population 16+* | Unemployment, NILF (Not in Labor Force)         |
| **Labor Force Participation**    | B23001 – *Sex by Age by Employment Status*      | Compute LFPR, inactive pop                      |
| **Commuting & Work Type**        | B08301 – *Means of Transportation to Work*      | Proxy for remote vs. essential work             |
| **Class of Worker**              | B24080 – *Class of Worker*                      | Public/private/self-employed distribution       |
| **Industry Employment**          | B24031 – *Industry by Occupation*               | Industry composition & cyclical vulnerability   |
| **Recent Job Movers** (optional) | B07011 – *Mobility Status by Employment*        | Share of job changers (optional mobility proxy) |
| **Population Estimates**         | PEPANNRES (Population Estimates Program)        | Normalize by population                         |

**Optional: Supplement from BLS (via API or flat file):**

| **Metric**               | **BLS Series** or Source | **Use**                                |
| ------------------------ | ------------------------ | -------------------------------------- |
| **Quits Rate**           | JOLTS – `QUR`            | Labor confidence proxy                 |
| **Layoffs Rate**         | JOLTS – `LAR`            | Involuntary separations                |
| **Job Openings Rate**    | JOLTS – `JOR`            | Labor demand signal                    |
| **Temp Help Employment** | CES – NAICS 5613         | Contingent labor / slack indicator     |
| **U-6 Rate**             | BLS U-series             | Broader unemployment / underemployment |

---

## 🛠️ Features Engineered for Frozen Market Index

| **Feature**                | **Formula**                                                              | **Description**                       |
| -------------------------- | ------------------------------------------------------------------------ | ------------------------------------- |
| `Unemployment_Rate`        | `Unemployed / Civilian Labor Force`                                      | Standard headline unemployment        |
| `Inactive_Workforce_Share` | `Not in Labor Force / Working Age Population`                            | Labor disengagement                   |
| `Low_Mobility_Proxy`       | Share of workers in same job, or inverse of recent movers (if available) | Job switching stagnation              |
| `Quits_to_Layoffs_Ratio`   | `Quits Rate / Layoffs Rate`                                              | High = labor confidence, Low = freeze |
| `Job_Openings_to_Unemp`    | `Job Openings / Unemployed`                                              | Low = hiring freeze                   |
| `Temp_Work_Share`          | `Temp Help Employment / Total Employment`                                | High = soft/slack market              |

---

## 🧮 Composite Frozen Market Index Formula

This is an inverse index: **higher values = more frozen conditions** (e.g. high slack, low hiring):

```python
Frozen_Market_Index = (
    0.25 * Unemployment_Rate +
    0.20 * Inactive_Workforce_Share +
    0.20 * (1 - Quits_to_Layoffs_Ratio) +
    0.15 * (1 - Job_Openings_to_Unemp) +
    0.10 * Temp_Work_Share +
    0.10 * Low_Mobility_Proxy
)
```

* All features should be standardized (z-scores) across metros
* `(1 - ...)` used where low values are worse (e.g. job openings ratio)
* Index can be rescaled from 0 to 100 for readability

---

## ✅ Notes

* You can adjust weights based on local economic theory or business needs
* This index emphasizes **labor demand**, **mobility**, and **participation**
* Optional: create a “Thaw Index” by inverting the score

---

Let me know if you want help pulling these datasets or building the pipeline in `pandas`.

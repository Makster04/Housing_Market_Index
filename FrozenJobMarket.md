You're absolutely right — the **JOLTS (Job Openings and Labor Turnover Survey)** data is **only available at the national and regional level**, not for **metropolitan areas**.

So to build a **Frozen Job Market Index at the metro level**, we’ll need to **rely entirely on Census data (ACS)** and other datasets available **by metro area**. Here's a revised version using only data you can get **from [data.census.gov](https://data.census.gov)** or **BLS local area unemployment statistics (LAUS)** where applicable.

---

# 🧊 Frozen Job Market Index — Metro-Level Version (Census + LAUS Only)

## 📊 Metro-Level Data Sources

| **Indicator**                    | **Table Code / Source**                         | **Use**                                                                 |
| -------------------------------- | ----------------------------------------------- | ----------------------------------------------------------------------- |
| **Employment Status**            | B23025 – *Employment Status for Population 16+* | Unemployment Rate, Labor Force Participation                            |
| **Not in Labor Force (NILF)**    | B23001 – *Sex by Age by Employment Status*      | Compute NILF share among working-age adults                             |
| **Mobility + Employment**        | B07011 – *Mobility by Employment Status*        | Share of recent job movers                                              |
| **Industry Composition**         | B24031 – *Industry by Occupation*               | Assess vulnerability to freezes (e.g., retail, hospitality-heavy areas) |
| **Work From Home Proxy**         | B08301 – *Means of Transportation to Work*      | Proxy for remote-capable workforce                                      |
| **Class of Worker**              | B24080 – *Class of Worker*                      | Distribution of government vs private jobs                              |
| **Population Estimate**          | PEPANNRES (Annual Population Estimates)         | Normalize to working-age base                                           |
| **Unemployment Rate (official)** | BLS Local Area Unemployment Statistics (LAUS)   | Monthly/annual metro unemployment                                       |

---

## 🛠️ Features Engineered for Frozen Market Index (Metro-Scoped)

| **Feature**                | **Formula / Source**                                                                      | **Description**                               |
| -------------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------- |
| `Unemployment_Rate`        | `Unemployed_Total / Civilian_Labor_Force` from B23025                                     | Standard unemployment                         |
| `NILF_Share`               | `Not in labor force / Population 16+` from B23001                                         | People not in labor force                     |
| `Moved_Share`              | `(Moved within county + different county + different state + abroad) / Total` from B07011 | Proxy for geographic fluidity                 |
| `Remote_Capable_Share`     | `Worked at home / Commuting_Total` from B08301                                            | Proxy for labor market resilience             |
| `Public_Sector_Share`      | `Government workers / ClassOfWorker_Total` from B24080                                    | Public sector is more stable (can freeze too) |
| `Retail_Hospitality_Share` | `(Retail trade + Accommodation and food services) / Industry_Total` from B24031           | High = more freeze-prone industries           |
| `Official_Unemployment`    | Directly from LAUS `[Unemployment Rate]`                                                  | Benchmark from BLS official estimate          |

---

## 🧮 Composite Frozen Market Index Formula

Here’s a simplified version using only **metro-available features**:

```python
Frozen_Market_Index = (
    0.25 * Unemployment_Rate +
    0.20 * NILF_Share +
    0.15 * (1 - Recent_Job_Mobility) +
    0.15 * Retail_Hospitality_Share +
    0.15 * (1 - Remote_Capable_Share) +
    0.10 * Public_Sector_Share
)
```

* Invert mobility and remote share so that **low values = more frozen**
* Use **z-score normalization** or min-max scale before aggregation
* Can rescale output to 0–100 for intuitive visualization

---

## ✅ Notes

* **No JOLTS or CES components** — this version is fully metro-compliant
* You can proxy labor tightness by combining NILF and industry structure
* Optional: If you have monthly LAUS data, compute a **trailing average or change** to add time sensitivity

---

Would you like a pre-built `pandas` script or SQL schema to construct this index using ACS + LAUS metro data?


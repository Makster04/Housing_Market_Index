To measure **how tight the job market is** in each **metro area** using **only data from [data.census.gov](https://data.census.gov)**, you can build a **Labor Market Tightness Index** that reflects:

* 📉 **Low unemployment**
* 🧍‍♂️ **High labor force participation**
* 🏙️ **Strong private-sector job presence**
* 🧠 **High skill readiness / education levels**
* 🚗 **Commuting efficiency and accessibility**

---

## 📦 Labor Market Tightness Index — Methodology Using data.census.gov

### 📊 Key Tables to Use

| **Indicator**                     | **Table Code / Name**                                  | **Use**                                                        |
| --------------------------------- | ------------------------------------------------------ | -------------------------------------------------------------- |
| **Employment Status**             | **B23025 – Employment Status for Population 16+**      | Get **unemployment rate** and **labor force participation**    |
| **Occupation by Class of Worker** | **C24010 – Occupation by Class of Worker**             | % in **private sector** jobs                                   |
| **Educational Attainment**        | **B15003 – Educational Attainment for Population 25+** | % with **Bachelor’s or higher** = proxy for skill readiness    |
| **Industry by Occupation**        | **C24030 – Industry by Occupation**                    | % in high-skill or growing sectors (e.g. healthcare, tech)     |
| **Means of Transportation**       | **B08101 – Means of Transportation to Work**           | % using **public transit**, proxy for metro-wide job access    |
| **Commute Time**                  | **B08303 – Travel Time to Work**                       | % with **short commutes** = indicates proximity to job centers |

---

### 🛠️ Features to Engineer

| **Feature**                  | **Formula** or Source                                | **Description**                                     |
| ---------------------------- | ---------------------------------------------------- | --------------------------------------------------- |
| `Unemployment_Rate`          | `Unemployed / Labor Force` (from B23025)             | Lower = tighter market                              |
| `Labor_Force_Participation`  | `Labor Force / Working-Age Population` (from B23025) | Higher = more active labor pool                     |
| `Private_Sector_Share`       | % in private wage/salary jobs (from C24010)          | Market-driven labor strength                        |
| `College_Degree_Rate`        | % with BA or higher (from B15003)                    | Skilled labor supply                                |
| `High_Growth_Industry_Share` | % in info, finance, STEM, healthcare (from C24030)   | Healthy, expanding job sectors                      |
| `Transit_Use_Share`          | % using public transportation (from B08101)          | Accessibility to job-rich zones                     |
| `Commute_Efficiency`         | % with commute < 30 mins (from B08303)               | Reflects job proximity and infrastructure alignment |

---

### 🧮 Sample Composite Formula

```python
Labor_Tightness_Index = (
    0.25 * (1 - Unemployment_Rate) +
    0.20 * Labor_Force_Participation +
    0.15 * Private_Sector_Share +
    0.15 * College_Degree_Rate +
    0.10 * High_Growth_Industry_Share +
    0.10 * Transit_Use_Share +
    0.05 * Commute_Efficiency
)
```

* All values should be normalized (e.g., using **z-scores**)
* Inverse is applied to unemployment rate so that **higher = tighter market**

---

## ✅ Notes

* A **tight labor market** = **many jobs, fewer available workers**
* This index favors metros with **high employment, skilled labor, and job access**
* Can be used to compare hiring conditions or wage pressure across cities

Would you like help building this into a pandas script or dashboard?

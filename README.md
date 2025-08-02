# National Health Insight Report

This repository provides an analytical breakdown of a national hospital admission dataset. It covers key trends in patient demographics, disease prevalence, hospital billing, treatment patterns, and length of stay (LOS) insights across various hospitals in the U.S.

> **Author:** Oforah Ivan Chukwudumeje  
> **Tools Used:** Power BI, Excel, Python (for preprocessing and validation)

---


<img width="2048" height="899" alt="Hospital Picture" src="https://github.com/user-attachments/assets/5167de4f-e576-4a56-89e2-2bea04b44a95" />


## 📌 Objectives

- Understand trends in patient admissions and diagnosis
- Compare hospital performance on cost, length of stay, and disease treatment
- Analyze how medical conditions impact patient outcomes and billing
- Derive actionable insights to guide hospital resource allocation and health policy

---

## 🔧 Tools & Technologies

| Tool          | Purpose                                  |
|---------------|-------------------------------------------|
| Power BI      | Dashboard creation & visual exploration   |
| Excel         | Pre-cleaning, tabular inspection          |
| Python (Pandas, Matplotlib) | Data preparation, validation, and EDA (optional pipeline) |

---

## 📊 Dataset Overview

The dataset contains **55,500 patient records** and includes:

- Hospital admission types (Elective, Emergency, Urgent)
- Length of stay buckets (1–3, 4–7, 8–14, 15+ days)
- Patient age, gender, diagnosis (6 conditions), and treatment type (5 drugs)
- Billing amount by room and stay
- Hospital-level statistics across 10 major institutions

---

### DAX QUERIES
```
Adm YoY = 
VAR SelectedYear = SELECTEDVALUE('Calendar'[Year])
VAR PrevYear = 
    IF(NOT ISBLANK(SelectedYear),
        SelectedYear - 1,
        BLANK()
    )
VAR PrevYearValue = 
    IF(NOT ISBLANK(PrevYear),
        CALCULATE([Admitted Patients], YEAR('Calendar'[Date]) = PrevYear),
        BLANK()
    )
VAR CurrentValue = [Admitted Patients]
VAR YoYChange = 
    IF(
        AND(NOT ISBLANK(CurrentValue), NOT ISBLANK(PrevYearValue)),
        DIVIDE(CurrentValue - PrevYearValue, PrevYearValue),
        BLANK()
    )
RETURN
IF(
    ISBLANK(SelectedYear),
    "No selection",
    IF(
        NOT ISBLANK(YoYChange),
        IF(YoYChange > 0, 
            "↑ " & FORMAT(YoYChange, "#.0%") & " Vs " & PrevYear,
            "↓ " & FORMAT(YoYChange, "#.0%") & " Vs " & PrevYear
        ),
        "No Data"
    )
)
```
```
Calendar = 
var mindate = MIN(HealthData[Date of Admission])
var maxdate = MAX(HealthData[Date of Admission])
VAR BaseCalendar =
    CALENDAR(mindate,maxdate)
RETURN
ADDCOLUMNS(
    BaseCalendar,
    "Year", YEAR([Date]),
    "Quarter", QUARTER([Date]),
    "Month No", MONTH([Date]),
    "Week Num", WEEKNUM([Date]),
    "Week Day", WEEKDAY([Date]),
    "Day", DAY([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Short", FORMAT([Date], "MMM"),
    "Week", FORMAT([Date], "dddd"),
    "Quarter Number", "Q" & QUARTER([Date]),
    "Year Month", FORMAT([Date], "YYYY MMMM"),
    "Day Type", IF(WEEKDAY([Date], 2) > 5, "Weekend", "Weekday"),
    "Weekending", [Date] + (7 - WEEKDAY([Date]))
)
```

<img width="1591" height="916" alt="Screenshot 2025-08-02 013331" src="https://github.com/user-attachments/assets/30e79e2b-f9c6-49a8-91dc-21af186aa67c" />
<img width="1593" height="915" alt="Screenshot 2025-08-02 013420" src="https://github.com/user-attachments/assets/75048d1b-0a2d-4bbd-867e-a02df4640a84" />
<img width="1596" height="915" alt="Screenshot 2025-08-02 014339" src="https://github.com/user-attachments/assets/6e9372c8-e18c-4632-86c3-006b2b0d5a2f" />


## 📈 Key Insights (Power BI Dashboard Highlights)

### 1. 🏥 Patient Demographics

- **Gender distribution** is almost equal across all age groups.
- **Age group 35–64** has the highest volume of admissions.
- Older patients (65+) show longer average stays.

### 2. 🧬 Disease Distribution Across Hospitals

| Disease       | Highest Count | Notable Institution                  |
|---------------|----------------|-------------------------------------|
| Diabetes      | 13,875         | Houston Methodist, Johns Hopkins    |
| Hypertension  | 13,875         | UCLA, Houston Methodist             |
| Obesity       | 12,765         | Common across all institutions      |

> 🔍 *Houston Methodist Hospital alone accounts for over 20,000 admissions.*

### 3. 💊 Medication Trends by Diagnosis

| Diagnosis   | Most Prescribed Drug |
|-------------|----------------------|
| Arthritis   | Paracetamol          |
| Cancer      | Lipitor              |
| Diabetes    | Aspirin, Ibuprofen   |

Each medical condition exhibits a unique treatment pattern, suggesting standardized protocols per diagnosis.

### 4. 💵 Billing and Cost Patterns

- **Average Room Billing Amount**: `$25,539`
- Highest billing concentration is linked to **Extended Stay** patients admitted through **Emergency** cases.
- Weekend admissions comprise **28.5%**, typically showing **longer LOS** and higher billing.

### 5. ⏳ Length of Stay (LOS) Distribution

- Majority of patients fall into **Short (1–3 days)** and **Moderate (4–7 days)** stays.
- Extended stays are more common with chronic conditions (Hypertension, Diabetes).

---

<img width="2048" height="510" alt="Recommendation" src="https://github.com/user-attachments/assets/c4d33749-fe4b-443c-8bb4-16a48e0c8a5a" />



## 📍 Suggested Use Cases

- **Policy Recommendations**: Targeted campaigns on lifestyle diseases like hypertension & obesity.
- **Hospital Admin**: Reallocate doctors to weekdays due to higher weekday traffic (~71.5%).
- **Insurance Providers**: Adjust premiums based on diagnosis-cost clusters.
- **Pharma**: Track drug usage against disease spread for inventory planning.

---



# hr-people-analytics
Power BI HR dashboard analyzing 4,879 records across 5 datasets | Turnover · Compensation · Performance · Recruitment | Power Query · DAX
# 👥 HR & People Analytics — Power BI Portfolio Project

## 📌 Project Overview
A full end-to-end HR analytics project built with **Power Query** and **Power BI**, covering workforce analysis, compensation, performance, attendance, and recruitment across 5 real-world HR datasets.

---

## 📂 Datasets Used

| File | Rows | Description |
|------|------|-------------|
| HR-Analytics-Employee-Master.xlsx | 200 | Core employee data |
| HR-Analytics-Compensation-History.xlsx | 436 | Salary change history |
| HR-Analytics-Performance-Reviews.xlsx | 498 | Employee performance reviews |
| HR-Analytics-Leave-Attendance.xlsx | 1,000 | Leave and attendance records |
| HR-Analytics-Recruitment-Funnel.xlsx | 2,745 | Recruitment pipeline data |

**Total: 4,879 rows of HR data**

---

## 🔧 Tools & Skills

- **Power Query** — Data cleaning, custom columns, null handling
- **Power BI** — HR data modeling, DAX measures, interactive dashboards
- **DAX** — CALCULATE, COUNTROWS, DIVIDE, AVERAGE, FILTER
- **HR Concepts** — Turnover Rate, Hired Rate, Absenteeism, Tenure Analysis

---

## 🧹 Data Cleaning (Power Query)

- Converted date columns (HireDate, TerminationDate, ChangeDate, ReviewDate, StartDate)
- Applied Text.Trim to all text columns
- Replaced null values:
  - `Gender` → "Not Specified"
  - `#N/A` values → "Unknown"
  - Numeric nulls → 0
  - Date nulls (TerminationDate, ManagerID) → kept as null (meaningful data)
- Removed errors with Remove Rows → Remove Errors
- Added calculated columns:

**Employee Master:**
- `Years of Service` = (Today - HireDate) / 365
- `Tenure Band` = < 1 Year / 1-3 / 3-5 / 5-10 / 10+ Years
- `Status Label` = Active / Terminated / On Leave
- `Salary Band` = Entry / Mid / Senior / Executive

**Compensation History:**
- `Salary Change %` = (NewSalary - OldSalary) / OldSalary × 100
- `Raise Band` = No Raise / Small / Medium / Large

**Performance Reviews:**
- `Performance Label` = Excellent / Good / Average / Needs Improvement
- `Avg Score` = (Communication + Teamwork + ProblemSolving) / 3

**Recruitment Funnel:**
- `Days to Hire` = DateatStage - ApplicationDate
- `Stage Label` = Applied / Interview / Offer / Hired / Rejected

---

## 🗂️ Data Model

- Built a **Star Schema** with Date Table at the center
- Date Table covers 2018–2026
- Relationships via Date:
  - Date → Employee Master (HireDate)
  - Date → Compensation History (ChangeDate)
  - Date → Performance Reviews (ReviewDate)
  - Date → Leave Attendance (StartDate)
  - Date → Recruitment Funnel (ApplicationDate)
- Relationships via EmployeeID:
  - Employee Master → Compensation History
  - Employee Master → Performance Reviews
  - Employee Master → Leave Attendance

---

## 📊 DAX Measures

```dax
Total Headcount       = COUNTROWS('Employee Master')
Active Employees      = CALCULATE(COUNTROWS(...), Status = "Active")
Turnover Rate         = Terminated / Total Headcount
Avg Salary            = AVERAGE('Employee Master'[Salary])
Total Leave Days      = SUM('Leave Attendance'[Days])
Avg Performance Score = AVERAGE('Performance Reviews'[OverallScore])
Total Applicants      = COUNTROWS('Recruitment Funnel')
Hired Rate            = Hired / Total Applicants
```

---

## 📈 Dashboards

### 1️⃣ Workforce Overview
- KPI Cards: Total Headcount, Active Employees, Turnover Rate, Avg Salary
- Headcount by Department (Bar Chart)
- Gender Distribution (Pie Chart)
- Tenure Band Distribution (Bar Chart)
- Salary Band Distribution (Pie Chart)
- Department Slicer

### 2️⃣ Compensation Analysis
- KPI Card: Avg Salary
- Avg Salary by Department (Bar Chart)
- Raise Band Distribution (Bar Chart)
- Salary by Job Title — sorted descending (Bar Chart)
- Department Slicer

### 3️⃣ Performance & Attendance
- KPI Cards: Avg Performance Score, Total Leave Days
- Performance Label Distribution (Bar Chart)
- Leave Type Distribution (Bar Chart)
- Avg Performance Score by Department (Bar Chart)
- Department Slicer

### 4️⃣ Recruitment Funnel
- KPI Cards: Total Applicants, Hired Rate
- Applications by Stage (Bar Chart)
- Applications by Source (Bar Chart)
- Applications by Job Title — sorted descending (Bar Chart)
- Source Slicer

---

## 💡 Key Business Insights

> Discovered through dashboard analysis:

1. **Sales & Quality Control lead in compensation** — both departments have the highest average salaries, closely matched, suggesting similar seniority levels.

2. **LinkedIn is the top recruitment source** — outperforming Indeed and other channels, indicating strong employer brand presence on LinkedIn.

3. **Hired Rate is only 5%** — a critically low conversion rate, meaning 95 out of 100 applicants are rejected. This signals either very high hiring standards or an inefficient recruitment funnel.

4. **Maternity/Paternity is the most common leave type** — suggesting a young workforce demographic, which has implications for workforce planning and coverage policies.

5. **Average Performance Score is 3.60/5** — the workforce performs slightly above average overall, with room for improvement particularly in departments scoring below 3.5.

---

## 🧠 Lessons Learned

- TerminationDate nulls must be preserved — they represent Active employees, not missing data.
- #N/A values from Excel must be replaced with "Unknown" using Replace Values, not Remove Errors.
- EmployeeID is the primary key connecting HR tables — more important than Date for people analytics.
- Hired Rate of 5% is a critical KPI that immediately flags recruitment efficiency issues.
- Years of Service calculation requires null protection to avoid errors on incomplete records.

---

## 👤 Author
**AbdelHassib Essayad**
Aspiring Data Analyst | Power BI · Power Query · HR Analytics


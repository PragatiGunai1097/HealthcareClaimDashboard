# Healthcare Claims Analytics Dashboard — Full Stack + SSRS

Complete enterprise healthcare claims reporting system with SQL Server, SSRS reports, C# Web API, and an attractive React dashboard.

---

## What's Included

```
HealthcareClaims-SSRS-Complete/
├── HealthcareClaims.sln                  ← Visual Studio solution
├── README.md                             ← This file
├── Database/
│   ├── 01_CreateDatabase.sql             ← Creates HealthcareClaimsDB
│   ├── 02_CreateTables.sql               ← 7 tables with indexes
│   ├── 03_SeedData.sql                   ← 300 claims, 30 patients, 15 providers
│   └── 04_StoredProcedures.sql           ← 9 stored procedures + 1 view
├── SSRS/
│   ├── Reports/
│   │   ├── KPISummary.rdl                ← KPI metrics table
│   │   ├── MonthlyClaimsTrend.rdl        ← Trend chart + table (param: Months)
│   │   ├── DeniedClaimsAnalysis.rdl      ← Denial bar chart + table
│   │   ├── ProviderPerformance.rdl       ← Provider metrics table
│   │   ├── HighCostClaims.rdl            ← High-cost claims (param: Threshold)
│   │   └── PatientDemographics.rdl       ← Demographics pie chart + table
│   ├── SharedDataSources/
│   │   └── HealthcareClaimsDB.rds        ← Shared data source
│   └── DeploySSRS.md                     ← Step-by-step SSRS deployment guide
├── HealthcareClaims.API/                 ← C# ASP.NET Core 8 Web API
│   ├── HealthcareClaims.API.csproj
│   ├── Program.cs
│   ├── appsettings.json
│   ├── Controllers/
│   │   ├── DashboardController.cs        ← 9 API endpoints (Dapper + SPs)
│   │   └── ReportsController.cs          ← SSRS URL builder endpoints
│   ├── Models/                           ← 7 EF Core entity models
│   ├── Data/ApplicationDbContext.cs
│   └── DTOs/DashboardDto.cs              ← 9 typed response DTOs
└── healthcare-dashboard/                 ← React 18 + Vite frontend
    ├── package.json
    ├── src/
    │   ├── pages/
    │   │   ├── Dashboard.jsx             ← 8 KPIs + 3 charts
    │   │   ├── ClaimsList.jsx            ← Searchable/filterable/paginated
    │   │   ├── ClaimsAnalysis.jsx        ← Denial analysis charts
    │   │   ├── ProviderPerformance.jsx   ← Provider comparison charts + table
    │   │   ├── PatientDemographics.jsx   ← Donut + age group bar chart
    │   │   ├── HighCostClaims.jsx        ← Threshold filter + table
    │   │   ├── HospitalPerformance.jsx   ← Hospital comparison charts + table
    │   │   ├── DiagnosisAnalysis.jsx     ← Bar + Radar charts + table
    │   │   └── SSRSReports.jsx           ← SSRS report launcher page
    │   └── components/
    │       ├── Layout.jsx                ← Sidebar + dark theme shell
    │       ├── KpiCard.jsx               ← Animated KPI cards
    │       ├── ChartCard.jsx             ← Chart wrapper component
    │       └── StatusBadge.jsx           ← Color-coded status badges
```

---

## Quick Start

### Step 1 — SQL Server (via Docker on Mac)

```bash
docker pull mcr.microsoft.com/mssql/server:2022-latest
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=HealthCare@2024!" \
  -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
```

Then run the scripts in order in Azure Data Studio or VS Code SQL:
1. `01_CreateDatabase.sql`
2. `02_CreateTables.sql`
3. `03_SeedData.sql`
4. `04_StoredProcedures.sql`

---

### Step 2 — C# API

Open `HealthcareClaims.sln` in Visual Studio 2022 or Rider, then:

```bash
cd HealthcareClaims.API
dotnet restore
dotnet run
```

API runs at: **http://localhost:5000**  
Swagger UI: **http://localhost:5000/swagger**

---

### Step 3 — React Dashboard

```bash
cd healthcare-dashboard
npm install
npm run dev
```

Dashboard runs at: **http://localhost:3000**

> The dashboard works standalone with built-in sample data even without the API running.

---

### Step 4 — SSRS Reports (Optional)

See `SSRS/DeploySSRS.md` for full instructions. Short version:
1. Upload `HealthcareClaimsDB.rds` as a shared data source
2. Create folder `Healthcare Claims Reports` in SSRS web portal
3. Upload all 6 `.rdl` files and link them to the shared data source
4. Open any report from `http://localhost/Reports`

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/dashboard/kpi` | KPI summary metrics |
| GET | `/api/dashboard/monthly-trend?months=12` | Monthly trend data |
| GET | `/api/dashboard/denied-claims` | Denial analysis |
| GET | `/api/dashboard/provider-performance` | Provider metrics |
| GET | `/api/dashboard/high-cost-claims?threshold=20000` | High-cost claims |
| GET | `/api/dashboard/patient-demographics` | Patient demographics |
| GET | `/api/dashboard/claims-by-type` | Claims by type |
| GET | `/api/dashboard/hospital-performance` | Hospital metrics |
| GET | `/api/dashboard/diagnosis-analysis` | Diagnosis breakdown |
| GET | `/api/reports/list` | All SSRS report URLs |
| GET | `/api/reports/url?reportName=KPISummary` | Single report URL |
| GET | `/api/reports/export?reportName=KPISummary&format=PDF` | Export URL |

---

## Connection String

Edit `HealthcareClaims.API/appsettings.json`:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Database=HealthcareClaimsDB;User Id=sa;Password=HealthCare@2024!;TrustServerCertificate=True;"
}
```

---

## Dashboard Pages

| Page | Charts | Features |
|---|---|---|
| Dashboard | Line + Doughnut + Bar + Progress bars | 8 animated KPI cards |
| Claims List | — | Search, filter, pagination |
| Claims Analysis | Bar + Bar + Doughnut + Progress bars | Denial reason breakdown |
| Provider Performance | Grouped Bar + Progress bars + Table | 15 providers ranked |
| Patient Demographics | Doughnut + Grouped Bar + Table | Gender + age groups |
| High-Cost Claims | — | Threshold filter $5K–$50K |
| Hospital Performance | Horizontal Bar + Grouped Bar + Table | 8 hospitals compared |
| Diagnosis Analysis | Horizontal Bar + Radar + Table | 12 ICD categories |
| SSRS Reports | — | Report launcher + export links |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Database | SQL Server 2022 |
| Reports | SQL Server Reporting Services (SSRS) |
| Backend | C# ASP.NET Core 8, Dapper, Entity Framework Core 8 |
| API Docs | Swagger / OpenAPI |
| Frontend | React 18, Vite 5, Tailwind CSS |
| Charts | Chart.js 4, react-chartjs-2 |
| Animation | Framer Motion |
| Icons | Lucide React |

<img src="assets/banner.svg" alt="Aleks Aleksandrov — Data Engineer · Analytics Engineer" width="100%">

<div align="center">

<img src="assets/tagline.svg" alt="I build pipelines that replace manual work · SQL · Python · dbt · Airflow · Power BI" width="760">

<br>

<a href="https://www.linkedin.com/in/aleks-aleksandrov-data"><img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
<a href="https://aleks-aleksandrov-s4va.vercel.app/"><img src="https://img.shields.io/badge/Portfolio-Visit-111827?style=for-the-badge&logo=vercel&logoColor=white" alt="Portfolio"></a>
<img src="https://img.shields.io/badge/Open%20to%20work-Data%20%26%20Analytics%20Engineering-16A34A?style=for-the-badge" alt="Open to work: Data and Analytics Engineering">
<img src="https://img.shields.io/badge/Location-Bulgaria-00966E?style=for-the-badge" alt="Location: Bulgaria">

<sub><a href="#-projects">Projects</a> · <a href="#-what-i-do-as-a-data-engineer">What I do</a> · <a href="#-experience">Experience</a> · <a href="#%EF%B8%8F-toolbox">Toolbox</a> · <a href="#-lets-talk">Contact</a></sub>

</div>

> [!NOTE]
> **👋 Welcome to my new GitHub home.** My older account was mostly university exercises and early experiments, so I started fresh here to keep the work that matters in one place — cleaned up, properly documented, and code I'm happy to walk you through line by line. New projects land here first.

## About me

I'm a **data engineer with a business background** — four years at Salling Group, Denmark's largest retailer, where I went from the warehouse floor to trade planning & pricing and commercial analytics.

My work sits where data engineering meets the business: getting data out of awkward places (legacy SQL, scanned labels, shared folders), turning it into clean, validated tables, and delivering it to the people who make decisions. I hold a BSc in Global Business Engineering (Software Technology) from VIA University College, Denmark.

<img src="assets/impact.svg" alt="Impact: reporting 8 hours to 90 minutes, label extraction 2 weeks to 3-4 days, meeting prep 25 to under 5 minutes, 100+ printed pages per week to 0" width="100%">

## 🚀 Projects

### [Subscription Analytics](https://github.com/aleks-al-aleksandrov/subscription-analytics-dbt) — end-to-end analytics engineering with dbt

A subscription business (customers, plans, subscriptions, payments) modelled from messy raw data to tested marts that answer *"what is our MRR?"*, *"who upgraded, downgraded or churned?"* and *"do billed payments reconcile?"*.

<a href="https://github.com/aleks-al-aleksandrov/subscription-analytics-dbt"><img src="https://raw.githubusercontent.com/aleks-al-aleksandrov/subscription-analytics-dbt/main/docs/lineage.png" alt="dbt lineage graph: raw sources to staging views, dimensions, facts, SCD Type 2 snapshot and monthly MRR" width="100%"></a>

- **Incremental `fct_payments`** with `delete+insert` and a 7-day lookback, so late-arriving payments are picked up without full refreshes.
- **SCD Type 2 snapshot** of subscriptions across two point-in-time loads, to track plan changes and cancellations over time.
- **Flags instead of filters** in staging (`is_late_arrival`, `is_successful`), so failed payments stay available for analysis.

`dbt` `DuckDB` `SQL` `Python` `Airflow` `GitHub Actions` — every model has a documented grain, and CI runs `dbt build` on each push.

<details>
<summary><b>⚙️ Airflow orchestration</b> — generate → load → <code>dbt build</code> → integrity gate</summary>

<br>

<img src="https://raw.githubusercontent.com/aleks-al-aleksandrov/subscription-analytics-dbt/main/docs/airflow_dag.png" alt="Airflow DAG: generate_data, load_raw, dbt_build and report_counts, with two successful runs and row-count gate checks" width="100%">

</details>

### [Smart Gallery System](https://github.com/aleks-al-aleksandrov/smart-gallery-system) — event-driven presentation automation

Warehouse team leaders used to print the day's plans and KPIs every morning. Now they save a file to a folder and every screen on the floor updates about two seconds later — offline, on a Raspberry Pi behind each screen.

<a href="https://github.com/aleks-al-aleksandrov/smart-gallery-system"><img src="https://raw.githubusercontent.com/aleks-al-aleksandrov/smart-gallery-system/main/docs/screenshots/realtime-update.gif" alt="A new shift plan is saved and the screen updates on its own" width="720"></a>

`Node.js` `Express` `WebSocket` `Chokidar` `Sharp` `Raspberry Pi` — bachelor project, piloted with warehouse managers: daily meeting prep from 15–25 to under 5 minutes.

### More

| Project | What it is | Stack |
|---|---|---|
| [**label-ocr-pipeline**](https://github.com/aleks-al-aleksandrov/label-ocr-pipeline) | Computer vision + OCR pipeline that turns shelf-label photos into validated product data — 2 weeks to 3–4 days per batch. Architecture, validation logic and tested reference implementations. | Python · OpenCV · YOLO · EasyOCR · Tesseract · Azure ML |
| [**co2-emissions-tracker**](https://github.com/aleks-al-aleksandrov/co2-emissions-tracker-) | Full-stack app for recording CO₂ emissions with statistics, CSV reports and role-based accounts. I led the 3-person team. | .NET 9 · Blazor · ASP.NET Core · EF Core · JWT |

## 🧱 What I do as a data engineer

| Stage | What I've done |
|---|---|
| **Ingest** | Extracted structured data from 3,000+ product-label images per batch with a YOLO + Tesseract OCR pipeline in Python |
| **Transform** | Rebuilt legacy SQL reporting logic on SAP DataSphere, with SQL and R automation replacing manual steps · layered dbt models (staging → marts, incremental, SCD Type 2) |
| **Validate** | Reconciled migrated reports against the legacy output before go-live · 84 dbt data tests, including custom financial reconciliation checks |
| **Orchestrate** | Scheduled pipeline jobs in Azure ML Studio · Airflow DAGs running dbt (Astro CLI) · GitHub Actions CI on every push |
| **Serve** | Power BI KPI dashboards built with data scientists and business stakeholders; live operational screens on the warehouse floor |

<details>
<summary><b>🔍 Label OCR pipeline</b> — two weeks of manual work down to 3–4 days</summary>

<br>

Product labels arrived as images and were typed into spreadsheets by hand. I split the problem into independent stages so each could be tested and improved on its own:

```mermaid
flowchart LR
    A[Label images<br/>3,000+ per batch] --> B[Detection<br/>YOLO]
    B --> C[Text extraction<br/>Tesseract / EasyOCR]
    C --> D[Cleaning &<br/>validation rules]
    D --> E[Structured output<br/>tables for analysts]
    F[Azure ML Studio<br/>scheduled jobs] -.runs.-> B
```

- **Why stages:** a bad crop and a bad OCR read are different problems; separating them made errors easy to trace.
- **Why Azure ML jobs:** batches run the same way every time, without someone babysitting a notebook.
- **Result:** extraction time from ~2 weeks to 3–4 days per batch (~70% less).

📖 **[Full architecture, validation logic and lessons learned →](https://github.com/aleks-al-aleksandrov/label-ocr-pipeline)**

</details>

<details>
<summary><b>🔁 Reporting migration to SAP DataSphere</b> — 8 hours to 90 minutes</summary>

<br>

Trade planning & pricing relied on legacy SQL scripts run by hand every month. I worked on moving them to SAP DataSphere:

```mermaid
flowchart LR
    A[Legacy<br/>SQL scripts] --> B[Rebuilt on<br/>SAP DataSphere]
    B --> C[Automated with<br/>SQL + R]
    C --> D{Reconcile vs<br/>legacy output}
    D -- match --> E[Monthly reports<br/>90 minutes]
    D -- mismatch --> B
```

- **What mattered most:** proving the new numbers matched the old ones before anyone relied on them.
- **Result:** the monthly run went from 8 hours to 90 minutes, with outputs reconciled against the legacy reports before go-live.

</details>

## 💼 Experience

| When | Role | Highlights |
|---|---|---|
| 2025 – 2026 | **Student Assistant, Commercial Analytics** · Salling Group | YOLO + OCR pipeline, Azure ML jobs, Power BI KPI dashboards |
| 2025 | **Intern, Trade Planning & Pricing** · Salling Group | Reporting migration to SAP DataSphere, SQL + R automation |
| 2021 – 2025 | **Production Assistant & Internal Systems** · Salling Group | Shift lead for 10+ people; built 3 internal web apps that replaced paper workflows |
| 2021 – 2026 | **BSc Global Business Engineering (Software)** · VIA University College | Bachelor project: Smart Gallery System |

## 💬 What my managers say

> *"Aleks showed initiative in identifying opportunities for process optimization and automation. He proactively sought ways to improve our workflows and implemented solutions that increased our department's efficiency."*
>
> —  Senior Manager, Commercial Analytics, Salling Group

> *"After several months, we noticed that he is interested in daily processes and how to make them better… he is the only responsible person at the production when management is not at work."*
>
> —  Team Leader, Salling Group

## 🛠️ Toolbox

| Area | Tools |
|---|---|
| **Data engineering** | <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python"> <img src="https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white" alt="SQL"> <img src="https://img.shields.io/badge/dbt-FF694B?style=flat-square&logo=dbt&logoColor=white" alt="dbt"> <img src="https://img.shields.io/badge/Airflow-017CEE?style=flat-square&logo=apacheairflow&logoColor=white" alt="Airflow"> <img src="https://img.shields.io/badge/DuckDB-FFF000?style=flat-square&logo=duckdb&logoColor=black" alt="DuckDB"> <img src="https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white" alt="Pandas"> <img src="https://img.shields.io/badge/R-276DC3?style=flat-square&logo=r&logoColor=white" alt="R"> |
| **Platforms & BI** | <img src="https://img.shields.io/badge/SAP%20DataSphere-0FAAFF?style=flat-square&logo=sap&logoColor=white" alt="SAP DataSphere"> <img src="https://img.shields.io/badge/Azure%20ML-0078D4?style=flat-square&logo=microsoftazure&logoColor=white" alt="Azure ML"> <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"> <img src="https://img.shields.io/badge/DAX-F2C811?style=flat-square&logoColor=black" alt="DAX"> <img src="https://img.shields.io/badge/Excel%20%2F%20VBA-217346?style=flat-square&logo=microsoftexcel&logoColor=white" alt="Excel / VBA"> |
| **Computer vision** | <img src="https://img.shields.io/badge/YOLO-111F68?style=flat-square&logoColor=white" alt="YOLO"> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white" alt="OpenCV"> <img src="https://img.shields.io/badge/Tesseract%20OCR-555555?style=flat-square&logoColor=white" alt="Tesseract OCR"> <img src="https://img.shields.io/badge/EasyOCR-555555?style=flat-square&logoColor=white" alt="EasyOCR"> |
| **Engineering** | <img src="https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white" alt="Git"> <img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white" alt="GitHub Actions"> <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker"> <img src="https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white" alt="Node.js"> <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript"> <img src="https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB" alt="React"> <img src="https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white" alt="Next.js"> <img src="https://img.shields.io/badge/.NET%20%2F%20C%23-512BD4?style=flat-square&logo=dotnet&logoColor=white" alt=".NET / C#"> |

## 🧭 How I work

- **Business question first.** Before I touch the data I want to know which decision it will change and what it costs today.
- **Trust is earned with reconciliation.** New numbers get checked against the old ones before anyone relies on them.
- **Measure before and after.** Every result above has a number attached, because "it feels faster" is not a result.
- **Simple things that keep running.** Tools I built for the warehouse were still in daily use more than a year later.

## 📫 Let's talk

Based in **Bulgaria** and open to **Data Engineer**, **Analytics Engineer** and **BI** roles — on-site, hybrid or remote. The fastest way to reach me is [LinkedIn](https://www.linkedin.com/in/aleks-aleksandrov-data).

<sub>🇧🇬 Bulgarian (native) · 🇬🇧 English (fluent) · 🇩🇪 German (basic) · Off the keyboard: hiking, surfing and fishing in the mountains I grew up in.</sub>

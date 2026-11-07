# Hospital Patient Monitoring System

**Big Data Capstone Project 2026 — Bachelor of Computer Science, Dili Institute of Technology (DIT)**
Dili General Hospital · near-real-time vital-sign monitoring with **Kafka → Spark Structured Streaming → PostgreSQL → Flask → live dashboard**.

> ⚠️ Academic software prototype using **synthetic data only**. It is **not** a clinically validated medical
> device. Alert thresholds are **illustrative demonstration rules**, not clinical criteria.

![Dashboard](docs/screenshots/dashboard.png)

## Features

* **Dashboard** — KPI cards (monitored patients vs monitoring events, stable / needs attention / critical, ICU),
  vital-sign charts (HR, SpO₂, temperature, BP — separate scales & units, patient selector, time ranges, demo
  threshold lines), status donut, real-time alerts with acknowledge/resolve, ward distribution with *simulated*
  capacity, live patient table (search, filter, sort, paginate), recent activity, pipeline status.
* **Patients** — list, profile, registration, editing, ward/room transfer, vital-sign history, alert history.
* **Live Monitoring** — events flowing through the pipeline, per-event latency, micro-batch throughput/latency charts.
* **Alerts** — alert centre with filters, grouping into episodes, auto-resolve, bulk acknowledge, audit trail.
* **Analytics** — descriptive statistics, event volume, status, alerts by category/severity, ward analysis,
  historical trends, data-quality metrics, Spark batch-analytics snapshot; date/ward/patient filters.
* **Reports** — daily / weekly / monthly, printable (PDF via browser) and CSV export, synthetic-data notice.
* **Settings** — configurable demo alert rules, CSV upload with progress & validation report, wards/devices,
  users & roles, audit log.
* **Security** — hashed passwords, roles (admin/clinician/viewer), protected endpoints, CSRF protection,
  input validation, session expiry, account lockout, audit logging, secrets in `.env`.

## Quick start (Docker)

```bash
cp .env.example .env            # then set real passwords in .env
docker compose up -d --build    # http://localhost:5000  (user: admin / ADMIN_PASSWORD)
docker compose run --rm producer --reset --speed 20     # replay events through Kafka → Spark
```

Full instructions (Docker and manual): **[docs/INSTALLATION.md](docs/INSTALLATION.md)**.

## Project structure

```
hospital-patient-monitoring/
├── backend/                 Flask REST API + shared core
│   ├── app/                 application factory, security, API blueprints
│   ├── hpms_core/           config, db, validation, rules, alert engine, importer, shared writer
│   ├── manage.py            init-db · create-users · import · reset-events · status
│   └── wsgi.py
├── frontend/                HTML/CSS/JS (Bootstrap 5, Chart.js vendored — works offline)
├── bigdata/
│   ├── producer/producer.py Kafka producer / event simulator (restartable)
│   └── spark/               stream_processor.py · transformations.py · spark_batch_analytics.py
├── database/                schema.sql · seed.sql · load_csv.sql
├── dataset/                 hospital_patient_monitoring_dataset_10000.csv · generator · expander · samples
├── docs/                    architecture, DFD, ERD, data dictionary, API, stages, testing, performance, report outline, demo guide
├── tests/                   pytest suite (54 tests) + performance/benchmark.py
├── docker-compose.yml
├── .env.example
└── README.md
```

## Documentation

| Deliverable | File |
|---|---|
| System architecture diagram | [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) · `docs/diagrams/architecture.svg` |
| Data flow diagram | [docs/DATA_FLOW.md](docs/DATA_FLOW.md) · `docs/diagrams/data_flow.svg` |
| Database ER diagram | [docs/DATABASE.md](docs/DATABASE.md) · `docs/diagrams/er_diagram.svg` |
| Data dictionary | [docs/DATA_DICTIONARY.md](docs/DATA_DICTIONARY.md) |
| REST API | [docs/API.md](docs/API.md) |
| Installation & run | [docs/INSTALLATION.md](docs/INSTALLATION.md) |
| Stage-by-stage build (1–10) | [docs/STAGES.md](docs/STAGES.md) |
| Testing & evaluation report | [docs/TESTING_AND_EVALUATION.md](docs/TESTING_AND_EVALUATION.md) · [docs/PERFORMANCE_RESULTS.md](docs/PERFORMANCE_RESULTS.md) |
| Capstone report outline | [docs/CAPSTONE_REPORT_OUTLINE.md](docs/CAPSTONE_REPORT_OUTLINE.md) |
| Presentation & live demo guide | [docs/DEMO_GUIDE.md](docs/DEMO_GUIDE.md) |

## Dataset

`dataset/hospital_patient_monitoring_dataset_10000.csv` — 10,000 events, 500 fictional patients, 16 columns,
2026-09-01 → 2026-09-29. The prompt's original file was not supplied, so this file was generated with exactly
the specified structure (`dataset/generate_dataset.py`, deterministic). It contains 80 deliberately flawed
readings to demonstrate validation. If you have the original file, drop it in with the same name — the importer
validates it. See [docs/DATA_DICTIONARY.md](docs/DATA_DICTIONARY.md).

## GitHub

```bash
git init && git add . && git commit -m "Hospital Patient Monitoring System — capstone"
git remote add origin https://github.com/<your-account>/hospital-patient-monitoring.git
git push -u origin main
```
`.env`, logs, checkpoints and expanded test datasets are excluded by `.gitignore`. Never commit real credentials
or real patient data.

---

## Sumáriu iha Tetun

Sistema ida-ne'e mak protótipu akadémiku ba monitorizasaun sinál vitál pasiente sira iha Ospitál Jerál Díli.
Dadus hotu mak **sintétiku** (la'ós pasiente loloos). Fluxu dadus: CSV / simuladór → **Kafka** → **Spark
Structured Streaming** (validasaun, hasai duplikadu, regra alerta demonstrasaun) → **PostgreSQL** → **Flask API**
→ **dashboard** ne'ebé atualiza an rasik kada segundu hira.

Oinsá hahú: kopia `.env.example` ba `.env`, hakerek password foun, depois hala'o
`docker compose up -d --build` no loke http://localhost:5000. Atu haree dadus tama moris (live):
`docker compose run --rm producer --reset --speed 20`. Matadalan kompletu iha `docs/INSTALLATION.md` no
matadalan ba aprezentasaun iha `docs/DEMO_GUIDE.md`.

# Student Information Center

A full-stack web application for browsing and managing student data at **University of Mataram**. Built as a database course project, it provides a searchable student directory with thesis title lookup, backed by a RESTful Flask API and a MySQL database.

---

## Features

- **Student Directory** — Browse all students (2007–2024) with name, NIM, enrollment year, and origin school.
- **Search & Filter** — Search by name or NIM, filter by enrollment year, and sort results.
- **Pagination** — Configurable page size (up to 100 records per page).
- **Student Detail** — View individual student profiles including linked thesis titles.
- **Thesis Browser** — Dedicated page for browsing thesis (Tugas Akhir) titles.
- **Dashboard Statistics** — At-a-glance counts: total students, students with a thesis, unique schools, and enrollment years.
- **Health Check** — `/health` endpoint for service monitoring.

---

## Tech Stack

| Layer    | Technology                              |
|----------|-----------------------------------------|
| Backend  | Python 3, Flask, Flask-CORS             |
| Database | MySQL (`mysql-connector-python`)        |
| Frontend | HTML5, Tailwind CSS, Font Awesome, Vanilla JS |
| Config   | `python-dotenv`                         |

---

## Project Structure

```
Student-Information-Center/
├── backend/
│   ├── app.py                      # Flask entry point, CORS, error handlers
│   ├── .env                        # Environment variables (DB credentials, Flask config)
│   ├── requirements.txt            # Python dependencies
│   ├── config/
│   │   └── db.py                   # MySQL connection
│   ├── controllers/
│   │   └── mahasiswa_controller.py # Business logic, input validation
│   ├── models/
│   │   ├── mahasiswa_model.py      # Student SQL queries
│   │   └── judul_ta_model.py       # Thesis title SQL queries
│   └── routes/
│       └── mahasiswa_routes.py     # Flask Blueprint URL routing
│
├── frontend/
│   ├── index.html                  # Student directory page
│   ├── detail.html                 # Individual student detail page
│   ├── ta.html                     # Thesis titles page
│   └── assets/
│       ├── app.js                  # Directory page logic
│       ├── detail.js               # Detail page logic
│       └── ta.js                   # Thesis page logic
│
└── data/
    ├── Data mhs 2007-2024.csv/.xlsx        # Full student dataset
    ├── data_judul_ta.csv/.xlsx             # Thesis titles dataset
    ├── data_mahasiswa_clean.csv            # Cleaned student records
    ├── data_sekolah_asal_clean.csv         # Cleaned origin school records
    ├── data_jurusan_sekolah_clean.csv      # Cleaned school department data
    ├── data_mahasiswa/                     # Per-year student CSV files
    └── data_sekolah/                       # Origin school CSV files
```

---

##  Database Schema

Database name: `db_mahasiswa`

**`mahasiswa`** — Student records
| Column       | Type         | Description            |
|--------------|--------------|------------------------|
| `nim`        | VARCHAR      | Student ID (primary key) |
| `nama`       | VARCHAR      | Full name              |
| `tahun_masuk`| YEAR / INT   | Enrollment year        |
| `id_sekolah` | VARCHAR      | Foreign key → `sekolah_asal.kode_sekolah` |

**`sekolah_asal`** — Origin high schools
| Column         | Type    | Description         |
|----------------|---------|---------------------|
| `kode_sekolah` | VARCHAR | School code (primary key) |
| `nama_sekolah` | VARCHAR | School name         |

**`judul_ta`** — Thesis titles (Tugas Akhir)
| Column  | Type    | Description           |
|---------|---------|-----------------------|
| `nim`   | VARCHAR | Foreign key → `mahasiswa.nim` |
| `judul` | TEXT    | Thesis title          |

---

## API Endpoints

Base URL: `http://localhost:5000`

| Method | Endpoint                      | Description                          |
|--------|-------------------------------|--------------------------------------|
| GET    | `/`                           | API info and available endpoints     |
| GET    | `/health`                     | Health check                         |
| GET    | `/api/mahasiswa/`             | All students (joined with school)    |
| GET    | `/api/mahasiswa/paginated`    | Paginated student list (see below)   |
| GET    | `/api/mahasiswa/tahun`        | List of unique enrollment years      |
| GET    | `/api/mahasiswa/statistics`   | Dashboard stats                      |
| GET    | `/api/mahasiswa/{nim}`        | Student detail + thesis titles by NIM|

### Paginated Query Parameters

| Parameter    | Default      | Description                                      |
|--------------|--------------|--------------------------------------------------|
| `page`       | `1`          | Page number                                      |
| `limit`      | `20`         | Records per page (max 100)                       |
| `search`     | *(empty)*    | Search by name or NIM                            |
| `tahun_masuk`| *(empty)*    | Filter by enrollment year                        |
| `sort_by`    | `tahun_masuk`| Sort column: `tahun_masuk`, `nama`, or `nim`     |
| `sort`       | `asc`        | Sort direction: `asc` or `desc`                  |

**Example:**
```
GET /api/mahasiswa/paginated?page=1&limit=20&search=budi&tahun_masuk=2020&sort_by=nama&sort=asc
```
---

## Data

The `data/` directory contains raw and cleaned CSV/Excel datasets sourced for this project:

- **Student records** — `Data mhs 2007-2024.csv` covers enrollment data from 2007 to 2024, split into yearly files under `data_mahasiswa/`.
- **Origin schools** — `data_sekolah_asal_clean.csv` and the files under `data_sekolah/` list the high schools students graduated from.
- **Thesis titles** — `data_judul_ta.csv` contains thesis titles linked to student NIMs.
- **Cleaned outputs** — `data_mahasiswa_clean.csv` and `data_jurusan_sekolah_clean.csv` are preprocessed versions ready for import.

---

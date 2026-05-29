<div align="center">

# ⬡ NanoTable

### Dynamic Data Management System

*Create fully custom-structured tables at runtime — your data, your structure, your rules.*

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)

</div>

---

## What is NanoTable?

Most data tools are rigid — one app per purpose. A habit tracker only tracks habits. An expense manager only manages expenses. **NanoTable changes this.**

NanoTable lets you define your own table structures from scratch. Create a table called *Expense Tracker* with columns Amount, Category, and Date. Create another called *Reading Log* with completely different columns. One system, infinite structures — like a personal Notion, built from the ground up.

> Built as a Database Systems project demonstrating meta-driven schema design, relational modelling, and full-stack web development.

---

## Features

### Data Management
- ✅ Create tables with fully custom column structures
- ✅ Four data types — `TEXT` `NUMBER` `DATE` `BOOLEAN`
- ✅ Insert, edit, duplicate, and delete rows
- ✅ Inline cell editing — click any cell to edit directly
- ✅ Sequential row numbering decoupled from database IDs

### Filtering & Search
- ✅ Server-side SQL filtering with 7 operators — `=` `≠` `>` `<` `≥` `≤` `contains`
- ✅ Filtering executed by PostgreSQL via subquery — not in application memory
- ✅ Client-side real-time row search
- ✅ Client-side column sorting — ascending and descending

### Analytics
- ✅ Live aggregation bar — sum, average, min, max, count per NUMBER column
- ✅ Analytics tab with auto-generated monthly bar chart
- ✅ Month-by-month breakdown table — total, avg, count, min, max
- ✅ Count mode for date-only tables (e.g. reading tracker, habit tracker)
- ✅ User-configurable column pickers for analytics

### Interface
- ✅ Dark mode single-page application
- ✅ Collapsible sidebar with search and pin-to-top
- ✅ Emoji icons per table, persisted across sessions
- ✅ Skeleton loading animations
- ✅ Toast notifications (success, error, info, warning)
- ✅ CSV export using browser Blob API
- ✅ Keyboard shortcuts — `Enter` to submit, `Esc` to close, `Ctrl+N` new table
- ✅ Session persistence across tab switches via sessionStorage

### Security
- ✅ bcrypt password hashing via Werkzeug
- ✅ Parameterised SQL queries — SQL injection prevention
- ✅ UNIQUE constraint on email at database level

---

## Database Design

NanoTable uses a **meta-driven schema** — instead of creating a new SQL table every time a user creates a table, all structures are stored as data inside 5 permanent relational tables.
Users
└── Tables (user_id FK)
├── Columns (table_id FK)
└── Rows (table_id FK)
└── CellValues (row_id FK, column_id FK)

| Table | Purpose |
|-------|---------|
| `users` | Stores registered user accounts |
| `tables` | Each user-created table (e.g. "Expense Tracker") |
| `columns` | Schema of each table — column names and data types |
| `rows` | Each row of data — links to a table |
| `cellvalues` | Actual cell data — one row per cell, typed value columns |

**Key design decisions:**
- `ON DELETE CASCADE` throughout — deleting a table removes all its columns, rows, and cells automatically
- `CHECK` constraint on `cellvalues` ensures exactly one value column is non-null per cell
- `UNIQUE(row_id, column_id)` enables UPSERT for cell editing
- Indexes on all foreign key columns for query performance
- Schema satisfies **Third Normal Form (3NF)**

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | HTML5, CSS3, JavaScript ES6+ |
| Backend | Python 3, Flask |
| Database | PostgreSQL 16 |
| DB Driver | psycopg2 |
| Auth | Werkzeug (PBKDF2-SHA256 hashing) |

---

## Project Structure
nanotable/
│
├── app.py                 # Flask backend — all 15 API endpoints
├── commands.sql           # PostgreSQL schema, user setup, indexes
├── requirements.txt       # Python dependencies
│
├── templates/
│   └── index.html         # Single-page application HTML structure
│
└── static/
├── style.css          # Dark mode UI — CSS custom properties,
│                        animations, responsive layout
└── script.js          # All frontend logic — API calls, state
management, analytics, export

---

## Setup & Installation

### Prerequisites
- Python 3.8 or higher
- PostgreSQL 16
- pip

### 1 — Clone the repository

```bash
git clone https://github.com/YOURUSERNAME/nanotable.git
cd nanotable
```

### 2 — Install Python dependencies

```bash
pip install -r requirements.txt
```

### 3 — Set up the database

Open **pgAdmin 4** and run the contents of `commands.sql` in the Query Tool.

Or via terminal:
```bash
psql -U postgres -f commands.sql
```

### 4 — Run the application

```bash
python app.py
```

### 5 — Open in your browser
http://localhost:5000

Register an account and start creating tables.

---

## API Reference

<details>
<summary>Click to expand — 15 endpoints</summary>

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/auth/register` | Register new user |
| `POST` | `/auth/login` | Authenticate and get user_id |
| `POST` | `/tables` | Create a new table |
| `GET` | `/tables?user_id=X` | Get all tables for a user |
| `PUT` | `/tables/{id}` | Rename a table |
| `DELETE` | `/tables/{id}` | Delete table and all its data |
| `POST` | `/columns` | Add a column to a table |
| `GET` | `/tables/{id}/columns` | Get all columns for a table |
| `PUT` | `/columns/reorder` | Reorder columns |
| `DELETE` | `/columns/{id}` | Delete column and its data |
| `POST` | `/rows/full` | Insert a complete row |
| `DELETE` | `/rows/{id}` | Delete a row |
| `POST` | `/cells` | Upsert a cell value |
| `GET` | `/tables/{id}/data` | Get full table data |
| `GET` | `/tables/{id}/data?column=X&op=>=&value=500` | Filtered data |

</details>

---


<div align="center">

*Built for the Database Systems course — demonstrating meta-driven schema design, relational modelling, REST API development, and full-stack web engineering.*

</div>
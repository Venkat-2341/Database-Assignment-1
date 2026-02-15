# Olympia Track — Sports Management System

> **CS 432 — Databases | Assignment 1 (Track 1) | Semester II (2025–2026)**  
> Indian Institute of Technology, Gandhinagar | Instructor: Dr. Yogesh K. Meena

---

## What is Olympia Track?

**Olympia Track** is a relational database-backed Sports Management System designed for schools, colleges, and sports clubs. It replaces scattered spreadsheets and manual registers with a single, structured database that handles everything from tournament scheduling to player health records.

The system is built around **13 normalized tables**, covering 9 core entities and 4 junction/tracking tables. It enforces referential integrity via foreign keys, business rules via `CHECK` constraints, and uniqueness where required (e.g., sport names, member emails).

---

## Project Structure

| File / Folder | Type | Description |
|---|---|---|
| `report.tex` | LaTeX | Full conceptual design report (UML + ER + Justifications) |
| `schema.sql` | SQL | `CREATE TABLE` statements for all 13 tables |
| `data.sql` | SQL | `INSERT` statements (15–20 rows per table) |
| `queries.sql` | SQL | Sample queries demonstrating system functionality |
| `ERDiag2.jpeg` | Image | ER diagram reference |
| `UML class (2).svg` | SVG | UML relationship diagram |

---

## Core Entities

| Entity | Primary Key | Role |
|---|---|---|
| `Member` | MemberID | Players, coaches, and admins — the central actor |
| `Sport` | SportID | Defines sport types (Team / Individual / Dual) |
| `Team` | TeamID | Groups of members with coach and captain |
| `Venue` | VenueID | Physical facilities with capacity and surface info |
| `Tournament` | TournamentID | High-level championship grouping events |
| `Event` | EventID | Individual matches/races within a tournament |
| `Equipment` | EquipmentID | Gear inventory tracked per sport |
| `PracticeSession` | SessionID | Scheduled team training at a venue |
| `PerformanceLog` | LogID | Timestamped player metrics (speed, goals, etc.) |
| `MedicalRecord` | RecordID | Injury and recovery tracking per member |

---

## ▶️ How to Run

**Prerequisites:** MySQL 8.0+ (or MariaDB 10.6+)

```sql
-- Step 1: Create database and load schema
CREATE DATABASE olympia_track;
USE olympia_track;
SOURCE schema.sql;

-- Step 2: Load sample data
SOURCE data.sql;

-- Step 3: Run sample queries
SOURCE queries.sql;
```

---

## Key Design Decisions

1. **Participation is Team-based, not Member-based.**  
   For individual sports (e.g., Tennis Singles), a solo player registers as a "Team of One." This lets the same `Participation` table handle both team and individual events without any schema changes.

2. **Captaincy is a FK, not a flag.**  
   Rather than an `IsCaptain` boolean in the roster, `Team.CaptainID` points directly to one member — enforcing exactly one captain per team at the database level.

3. **SQL keyword conflicts were renamed.**  
   The UML attribute `Condition` was split into `EquipmentCondition` and `MedicalCondition`. `Rank` was renamed `EventRank` to prevent reserved-word errors across SQL dialects.

4. **EquipmentIssue tracks both issue and return.**  
   A single row stores `IssueDate`, `ReturnDate` (nullable), and `Quantity` — giving a full audit trail per equipment loan without duplicate rows.

---

<p align="center"><i>CS 432 — Databases · Assignment 1 · IIT Gandhinagar · 2025–2026</i></p>

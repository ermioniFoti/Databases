# University Database Management System (PostgreSQL)

A comprehensive Relational Database Management System (RDBMS) designed to manage the academic and administrative operations of a University Department. This project was developed as **Phase A** of the laboratory assignment for the **Distributed Information Systems and Applications (PLH 303)** course at the **Technical University of Crete** (Spring 2021-2022).

## 📌 Project Overview
The system models the complex relationships between students, professors, courses, labs, and administrative rules. It is built on **PostgreSQL** and extends a baseline schema to support advanced functionality such as thesis management, lab workgroups, and automated grading logic.

### Core Domain Scope
The database manages:
* **Personnel:** Professors, Lab Teachers, and Students with unique IDs (AMKA) and ranks.
* **Structure:** Sectors and Laboratories, where labs are directed by senior professors.
* **Curriculum:** Courses (Compulsory/Elective), Prerequisites, and Semester scheduling.
* **Lab Operations:** Lab modules, student workgroups, and project grading.
* **Graduation:** Diploma theses, committees, and degree computation.


## 🚀 Features & Functionality

### 1. Schema Expansion
The project extends the initial ER model (specifically the "Green Polygon" area) to include:
* **Thesis Management:** Tables for `Diploma`, `Committee`, and `Thesis_Grade`.
* **Lab Management:** Tables for `LabModule`, `WorkGroup`, and student participation (`Joins`).

### 2. Stored Procedures (Data Management)
Custom PL/pgSQL functions were implemented to automate data population and management:
* **`assign_theses()`**: Automatically assigns random thesis titles (from a repository) to eligible 4th-year students who have not yet undertaken a thesis.
* **`create_workgroups(lab_module_id, count)`**: Randomly forms student workgroups for specific lab exercises, respecting maximum member limits.
* **`insert_grades(semester_id)`**: Batches insertion of random exam and lab grades for registered students, respecting existing valid grades.

### 3. Advanced Data Retrieval
Complex SQL queries and functions provide critical insights:
* **Staff Lookup:** Retrieve all Professors and Lab Teachers belonging to a specific Sector.
* **Student Transcripts:** Fetch full grade reports (Exam, Lab, or Final) for a student in the current semester.
* **Curriculum Analysis:** Identify elective courses that are *not* offered in the current semester.
* **Workload Calculation:** Calculate total teaching hours for Lab Staff, including overhead for supervising student workgroups.
* **Sector Analytics:** Identify which Sector produces the most diploma theses based on supervisor affiliation.

### 4. Business Logic (Triggers)
Automated constraints and logic ensures data integrity:
* **Capacity Checks:** Prevents exceeding the maximum number of members in Thesis Committees and Lab Workgroups.
* **Temporal Logic:** Automatically calculates `academic_year` and `academic_season` when a new Semester is inserted.
* **Automated Grading:**
    * Calculates `final_grade` based on weighted averages of Exam (X%) and Lab (Y%) scores.
    * Enforces "Minimum Passing Thresholds" for both Exam and Lab components.
    * Automatically updates the student's status to `PASS` or `FAIL`.
* **Registration Validation:**
    * Checks course prerequisites before accepting a registration.
    * Enforces ECTS limits (max 20 credits) or Course Count limits (max 6) per semester.
* **Semester Initialization:** When a new "Future" semester is created, the system automatically instantiates `CourseRun` entries for all courses typical to that season, assigning random staff and grading rules.

## 📝 Grading Rules Implementation
The system strictly enforces the department's grading regulation:
* **Non-Lab Courses:** 100% Exam based.
* **Lab Courses:**
    * If `Lab_Grade < Minimum_Limit`: Final Grade = 0 (Fail).
    * If `Exam_Grade < Minimum_Limit`: Final Grade = Exam_Grade (Fail).
    * Otherwise: `Final = (Exam * Exam_%) + (Lab * Lab_%)`.
* **Passing Grade:** A minimum of 5.0 is required to pass.


## 🛠️ Technology Stack
* **Database Engine:** PostgreSQL 13.6
* **Management Tool:** pgAdmin 4
* **Languages:** SQL, PL/pgSQL



---
*Developed for the School of Electrical and Computer Engineering (ECE), Technical University of Crete.*

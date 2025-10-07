# Unified Timeline Generation (MY CONTRIBUTION)

## Overview 📜
This component is responsible for ingesting multiple raw, heterogeneous data sources from the campus and creating a single, clean, chronological timeline of all known events for each entity (student or staff). It serves as the foundational data preprocessing step for the entire Campus Security Monitoring System, preparing the data for analysis, monitoring, and machine learning tasks.

---

## Process ⚙️
The Jupyter Notebook `Unified_Timeline.ipynb` executes a multi-step pipeline to achieve a clean, unified dataset.

### 1. Data Loading & Cleaning
- Loads all primary data sources into pandas DataFrames:
  - `student_or_staff_profiles.csv`
  - `campus_card_swipes.csv`
  - `wifi_association_logs.csv`
  - `lab_bookings.csv`
  - `library_checkouts.csv`
  - `free_text_notes.csv`
- The profiles DataFrame is cleaned to create a single, unique **`student_id/staff_id`** for each individual, which serves as the primary key for entity resolution.

### 2. Timestamp Conversion & Feature Engineering
- All string-based timestamp columns across all DataFrames are converted into proper **`datetime`** objects.

### 3. Entity Resolution & Event Standardization
- Each activity log is merged with the cleaned profiles table to map various identifiers to the common `student_id/staff_id`.
- All events are then standardized into a uniform format with the core columns.
  - **Point-in-time events** (`card_swipe`, `wifi_log`, `library_checkout`) are processed directly.
  - **Duration-based events** (`lab_booking`) are restructured into two distinct events: `lab_booking_start` and `lab_booking_end`.
  - **Unstructured Text Events** (`free_text_notes.csv`):Regular expressions (regex) are used to standardize location names based on identified patterns.

### 4. Timeline Aggregation & Finalization
- All the standardized event DataFrames are concatenated into a single master timeline.
- This timeline is then sorted chronologically for each person (`student_id/staff_id` then `timestamp`).
- The final index is reset to provide a clean, sequential log of all activities.

---

## Output 📊
The script generates a single file, **`unified_timeline.csv`**, which contains the complete, cleaned, and sorted activity log for all campus entities.
---

## How to Run ▶️
1. Ensure you have Python and the required libraries installed (`pandas`, `numpy`).
2. Place the `Unified_Timeline.ipynb` notebook in the same directory as the required input CSV files.
3. Run all cells in the Jupyter Notebook from top to bottom.
4. The output file, `unified_timeline.csv`, will be generated in the same directory.

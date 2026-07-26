#  IN PROGRESS

# CNC Shop Scheduler

A Python-based job scheduling system that simulates how an ERP might route CNC shop
work orders to work centers. Built as a portfolio project during a transition from
CNC machine operation into data analytics.

# What it does

1. JOB ENTRY - a script collects a new job order (job number, part number,
   material, due date, and one or more operations) via input prompts and logs it
   to `schedule.csv`. At this point in the script, no WC or Start Time is chosen yet.
2. WORK CENTER MATCHING - each operation is matched against
   `shop_config.json` to find work centers in the right department that can run
   the given material.
3. SCHEDULING — a greedy scheduler assigns each operation to a work center and
   computes a start time based on when that work center is next free and when the
   prior operation in the same job finishes. Results are appended to
   `schedule.csv`, which is the single source of truth for machine availability.

# Why it's built this way

**Flat work center list instead of nested by department.**
The original config nested work centers under `dept_1_work_centers` /
`dept_2_work_centers`, which forced department-specific branching in code. It's
now a single flat list where each work center carries its own `department` and a
`capabilities` list (e.g. `["steel", "aluminum"]`). Adding a work center, a
department, or a material no longer requires touching the matching logic — the
loop just checks `wc["department"] == target` and `material in wc["capabilities"]`.

**Datetime objects instead of integer ticks.**
An earlier version modeled time as a tick-based calendar. The current version
assumes 24/7/365 operation and uses plain `datetime`/`timedelta` objects instead.
This is a deliberate simplification: shop hours, breaks, and holidays are real
requirements but are being deferred until the core scheduling logic is proven out.
Ticks will likely come back once that layer is added.

**Raw setup/run time instead of a computed finish time.**
`schedule.csv` stores `Setup Time` and `Run Time` directly rather than a
precomputed `Finish` column. A stored finish time is derived data that can drift
from the values it was computed from if either changes. Finish time is instead
recomputed wherever it's needed (e.g. `start + timedelta(hours=setup+run)`),
which is slightly more computation but removes an entire class of consistency bugs.

## How to run

1. Run the job entry script and answer the prompts for a new job. This appends
   the job's operations to `alljobs.csv` and holds them in memory as `new_rows`.
2. The scheduler reads `schedule.csv` to reconstruct each work center's current
   availability, matches each operation against `shop_config.json`, and assigns a
   work center and start time to each operation.
3. The completed rows are appended to `schedule.csv`.

Each run re-reads `schedule.csv` before scheduling, so work center availability
stays consistent across separate runs without needing to keep state in memory
between sessions.

## Status

Job entry prompts and work center candidates is functdional, next step is 
selecting from the "candidates" list to pick a work center. Not yet built:
shop hours / breaks / holidays, due-date-aware prioritization, and handling for
jobs with no valid candidate work center.
#  IN PROGRESS

# CNC Shop Scheduler

A Python-based job scheduling system that simulates how an ERP might route CNC shop
work orders to work centers. Built as a portfolio project during a transition from
CNC machine operation into data analytics.

# What it does

1. Job Entry

   A script collects a new job order (job number, part number,
   material, due date, and one or more operations) via input prompts and logs it
   to 'schedule.csv'.

2. Work Center Matching
   
   A layout file ('shop_config.json') holds the capabilities and labeling 
   data for each work center in the shop (department and materials). It 
   is pulled into a dictionary in the scheduling logic script and parsed
   as needed.

3. Scheduling
   
   A greedy (picks the best available option at that moment without looking
   forward or back) scheduler assigns each operation to a work center and computes
   the best possible start time. Work centers are narrowed down by compatability 
   and availability. 

# Why it's built this way

1. Flattened Work Center List

The original config nested work centers under 'dept_1_work_centers', 'dept_2_work_centers', 
etc. (which was an easily interpreted visual structure in json format), but it forced 
department-specific branching in the coding logic. It's now a single flat 
list where each work center carries the dept and capabilities as attributes. This prevents 
the need for additional layers of code to unwrap the dictionary format of data from 
shop_config.json when it's pulled into the script.

2. Datetime Objects without Integer "Ticks"

At this point, the scheduler assumes 24/7/365 operation hours. A datetime calendar of
start times, finish times, and due dates was used to simplify that element of the
script until work center selection was operating properly. The next step would be 
converting any datetimes to "ticks past" an anchor point, and any values in units 
of hours would be converted to a specified timedelta. Holidays, breaks, and weekends
would be accounted for by blocking off ranges of "elapsed ticks" in the calendar as 
non-operational times.

3. Finish Times Calculated as Needed

Operationally, the "finish time" of an operation sequence would be necessitated by
the start time of the next operation sequence on that work center. To avoid clutter
and redundant information on the schedule, finish times (for predicting work center
availability and the movement of a job folder to the next work center in sequence)
are calculated on an 'as-needed' basis within the script. 

## How to run

1. Run the job entry script and answer the prompts for a new job.

2. The scheduler reads 'schedule.csv' and pulls the current scheduling
   picture of the shop into a dataframe in the script. This is where any
   information needed for calculating start times and choosing work centers
   comes from.

3. The scripting logic follows the priorities mentioned above to update
   'schedule.csv' with the new job and all associated operations. Upon completion
   of the script run, 'schedule.csv' will be current.

***Each run re-reads 'schedule.csv' before scheduling, so work center availability
stays consistent across separate runs without needing to keep state in memory
between sessions.***

## Status

Job placement is functional. Operation sequences will not overlap on a work center, and
operations within a given job will not overlap each other or lose sequence.

Not yet built:
shop hours / breaks / holidays, due-date-aware prioritization, and handling for
jobs with no valid candidate work center.
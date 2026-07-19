---CNC Shop Schedule---

A JSON and CSV based shop scheduling program.

---Core Idea---

This set of scripts will take files built to model the work centers in a mock-up CNC shop, and allow for files representing job submissions that populate the work center schedules.

---Goals---

1. Accomodate for work center capabilities

2. Account for setup and run times of part orders

3. Sequence jobs on work centers to meet due dates as necessary

4. Update a running job schedule and display a visualization

5. Flag bottlenecks or scheduling conflicts in machine availability that create timelines that cannot be met according to logged run times of submitted jobs

---Current Shop Being Modeled---

This layout reprents a shop with 3 CNC Mills, 3 fabrication stations, 2 assembly stations, and 2 stock saws.
The capabilities of each station are built into the values in the file.

#-----------------------------------------------------
#   Updated Approach 7/12
#-----------------------------------------------------

Goals remain the same, but changing script strategy to use one JSON file (shop config) and base jobs off of user inputs

#-----------------------------------------------------
#    Updated Strategy 7/18
#-----------------------------------------------------

Jobs will still be user inputs, but a JSON file of job information will be included to be a sample of job info that 
can be used to test the script. A third JSON file will be saved as an empty array, and each run of the scheduling script will 
update that file to serve as a master list of jobs entered. 

Overall Strategy ----

Starting Files:
    alljobs.json                (an empty array that gets updated to show all entered jobs with each script run)
    update_schedule.ipynb       (a script that generates user-input prompts for job information and schedules the jobs entered)
    cnc_and_weld_jobs.json      (a sample of jobs that can be used to test the scripts for scheduling)
    shop_config.json            (a digital representation of available work centers in the shop and what they can do)
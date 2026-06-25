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

#CNC Shop Schedule
A JSON and CSV based shop scheduling program.

#Core Idea
This set of scripts will take files built to model the work centers in a mock-up CNC shop, and allow for files representing job submissions that populate the work center schedules.

#Goals
-Accomodate for work center capabilities
-Account for setup and run times of part orders
-Sequence jobs on work centers to meet due dates as necessary
-Flag bottlenecks in machine availability that create timelines that cannot be met according to logged run times of submitted jobs
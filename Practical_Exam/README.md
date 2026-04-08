=> Step 1: Data Modeling & Cleaning

-Load Data: Import all your datasets (Excel) into Power BI.
-Transform Data (Power Query):
-Go to 'Transform Data' and check column names.
-Fix data types (e.g., Dates should be 'Date', Scores should be 'Whole Number').
-Replace null or missing values with '0' or 'Unknown'.
-Relationships: Go to the 'Model View' and link your tables (e.g., link Student ID from the Info table to the Scores table).

Step 2: Creating DAX Measures
-% Score = DIVIDE(SUM('Scores Dataset'[Score]),SUM('Scores Dataset'[Max Score]),0)
-Absent Days = CALCULATE(COUNT('Attendance Dataset'[Date]),'Attendance Dataset'[Status] = "Absent")
-Attendance % = DIVIDE([Present Days],[Total Attendance Days],0)
-Attendance Status = IF([Attendance %] >= 0.75,"Regular","Irregular")
-Average % Score = AVERAGEX(VALUES('Students Dataset'[Student ID]),[% Score])
-Average Score = AVERAGE('Scores Dataset'[Score])
-Behavior Count = COUNT('Behavior Dataset'[Behavior Type])
-Highest Score = MAX('Scores Dataset'[Score])
-Lowest Score = MIN('Scores Dataset'[Score])
-Performance Category = SWITCH(TRUE(),[Average % Score] >= 0.80, "High",[Average % Score] >= 0.40, "Medium", "Low")
-Present Days = CALCULATE(COUNT('Attendance Dataset'[Date]),'Attendance Dataset'[Status] = "Present")
-Result Status = IF([Average % Score] >= 0.40,"Pass","Fail")
-Total Attendance Days = COUNT('Attendance Dataset'[Date])
-Total Days = COUNT('Attendance Dataset'[Date])
-Total Students = COUNT('Students Dataset'[Student ID])

Step 3: Building Visualizations
-Create the following charts on your canvas:
-Bar Chart: Show Average Scores on the Y-axis and Subject/Class on the X-axis.
-Line Chart: Show the Performance Trend over different Terms.
-Donut Chart: Use this for Behavior Types distribution.
-Table: List student names and scores. Use Conditional Formatting:
-Green background if Score > 80%
-Red background if Score < 40%
-Card Visuals: Display big numbers for Total Students, Avg Attendance, and Avg Score.

Step 4: Adding Interactivity
-Slicers: Add filters for Class, Section, Subject, and Term.
-Drill through: Create a separate page for "Student Profile" so you can right-click a name to see their specific details.
-Tooltips: Design a small pop-up page that shows a mini-chart when you hover over a data point.
-Bookmarks: Create buttons to switch between "Academic View" and "Behavioral View."


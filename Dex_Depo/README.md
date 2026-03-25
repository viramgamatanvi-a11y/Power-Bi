1\. Imports All Dataset to Power Query.



2\. Remove Blank Rows:

&#x20;  Home->Remove Rows->Remove Blank Rows



3\. Remove Duplicates:

&#x20;  Home->Remove Rows->Remove Duplicates



4\. Close \& Apply.



=> Task to Perform :

&#x20;

=> 1. Calculated Columns : 

=> 1. Profit Column (Sales\_ Fact)

&#x20;  Table View -> Sales\_ Fact -> New Column -> Profit = Sales\_ Fact\[Sales Amount] - Sales\_ Fact\[Cost]



=> 2. Return Flag Column (Sales\_ Fact)

&#x20;  Sales\_ Fact -> New Column ->   

&#x20;  Return Flag = IF(ISBLANK(LOOKUPVALUE(Returns\_ Fact\[Return ID],Returns\_ Fact\[Sales ID], Sales\_ Fact\[Sales ID])),"Not Returned", "Returned")



=> 3. Full Name Column (Customer\_ Dim)

&#x20;  Customer\_ Dim -> New Column

&#x20;  Full Name = Customer\_ Dim\[FirstName] \& " " \& Customer\_ Dim\[Last Name]





=> 2. Measures

=> Table View -> New Measures :



\-Total Sales = SUM(Sales\_ Fact\[Sales Amount])



\-Total Cost = SUM(Sales\_ Fact\[Cost])



\-Total Profit = SUM(Sales\_ Fact\[Profit])



\-Return Rate = DIVIDE(COUNTROWS(Returns\_ Fact),COUNTROWS(Sales\_ Fact))



\-Avg Sale = DIVIDE(\[Total Sales],DISTINCTCOUNT(Sales\_ Fact\[Sales ID]))





=> 3. Quick Measures



=> 1. New Quick Measure -> Select → Year-over-year change -> Base value → Total Sales, Date → Date column



=> 2.New Quick Measure -> Select → Month-over-month change -> Base value → Total Sales, Date → Date column





=> 4. Measures Management 



=> Go to Home → Enter Data -> Name table: Measures



=> Move all measures: Change Home Table → Measures





=> 5. Filter Context \& Behavior : 



=> Sales (Ignore Filters) = CALCULATE(\[Total Sales], ALL(Sales\_ Fact))



=> Sales Returned Only = CALCULATE(\[Total Sales],FILTER(Sales\_ Fact, Sales\_ Fact\[Return Flag] = "Returned"))



=> Sales Selected Region = CALCULATE(\[Total Sales])





=> 6. DAX Operators and Functions :



1\. MAX (Measure)

&#x20;  Max Sale = MAX(Sales\_ Fact\[Sales Amount])



2\. DISTINCTCOUNT (Measure)

&#x20;  Total Transactions = DISTINCTCOUNT(Sales\_ Fact\[Customer ID])



3\. COUNTX (Iterator)

&#x20;  Count High Sales = COUNTX(FILTER(Sales\_ Fact, Sales\_ Fact\[Sales Amount] > 1000),Sales\_ Fact\[Customer ID])



4\. IF (Calculated Columns)

&#x20;  Sales Type = IF(Sales\_ Fact\[Sales Amount] > 1000, "High", "Low")



5\. AND (Calculated Columns)

&#x20;  High Profit Sales = IF(AND(Sales\_ Fact\[Sales Amount] > 1000, Sales\_ Fact\[Cost] < 500),"Good", "Normal")



6\. OR (Calculated Columns)

&#x20;  Check Condition = IF(OR(Sales\_ Fact\[Sales Amount] > 1000, Sales\_ Fact\[Cost] < 300),"Condition Met", "Not Met")



7\. SWITCH (Calculated Columns)

&#x20;  Sales Category = SWITCH(TRUE(),Sales\_ Fact\[Sales Amount] < 500, "Low", Sales\_ Fact\[Sales Amount] < 1000, "Medium", "High")  



8\. CONCATENATE (Calculated Columns)

&#x20;  Full Name = Customer\_ Dim\[FirstName] \& " " \& Customer\_ Dim\[Last Name]



9\. UPPER (Calculated Columns)

&#x20;  Upper Name = UPPER(Customer\_ Dim\[Full Name])



10\. LEFT (Calculated Columns)

&#x20;   First 3 Letters = LEFT(Customer\_ Dim\[FirstName], 3)



11\. YEAR (Calculated Columns)

&#x20;   YEARS = YEAR(Date\_ Dim\[Date])



12\. MONTH (Calculated Columns)

&#x20;   MONTHS = MONTH(Date\_ Dim\[Date])



13\. EOMONTH (Calculated Columns)

&#x20;   End of Month = EOMONTH(Date\_ Dim\[Date], 0)





=> 7. Joining and Relationships :



=> Customer Name = RELATED(Customer\_ Dim\[Full Name])



=> Customer Segment = RELATED(Customer\_ Dim\[Segment])





=> 8. Time Intelligence (Matrix-based Analysis):



1\. TOTALYTD

&#x20;  YTD Sales = TOTALYTD(\[Total Sales],Date\_ Dim\[Date])



2\. SAMEPERIODLASTYEAR

&#x20;  Last Year Sales = CALCULATE(\[Total Sales],SAMEPERIODLASTYEAR(Date\_ Dim\[Date]))



3\. DATESINPERIOD

&#x20;  Sales Last 3 Months = CALCULATE(\[Total Sales],DATESINPERIOD(Date\_ Dim\[Date],MAX(Date\_ Dim\[Date]),-3,MONTH))



4\. CALCULATE() and DATESBETWEEN()

&#x20;  Running Total = CALCULATE(\[Total Sales],DATESBETWEEN(Date\_ Dim\[Date],MIN(Date\_ Dim\[Date]),MAX(Date\_ Dim\[Date])))





=> 9. Additional Scenarios :

1\. Create Calculated Column

&#x20;  Sales Category's = SWITCH(TRUE(),Sales\_ Fact\[Sales Amount] < 500, "Low", Sales\_ Fact\[Sales Amount] < 1000, "Medium", "High")



2\. Create Measure SUMX()

&#x20;  Total Profit (SUMX) = SUMX(Sales\_ Fact, Sales\_ Fact\[Sales Amount] - Sales\_ Fact\[Cost])



3\. Create Measure AVERAGEX()

&#x20;  Avg Profit per Transaction = AVERAGEX(Sales\_ Fact, Sales\_ Fact\[Sales Amount] - Sales\_ Fact\[Cost])

&#x20;



= 10. Matrix 

=> Rows 

\- Region Name

\- Category

\- Segment

\- Month



=> Column

\- Year



=> Values

\- Total sales

\- Total Profit

\- Avg Profit per Transaction

\- YTD Sales

\- Last Year Sales

\- Sales Last 3 Months

\- Running Total

\- Return Rate






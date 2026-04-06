1.  DATA MODELING

-   Load all tables into Power BI.
-   Go to Model View.
-   Create relationships:
   -Sales_ Fact[Customer ID] → Customer_ Dim[Customer ID]
   -Sales_ Fact[Product ID] → Product_ Dim[Product ID]
   -Sales_ Fact[Date ID] → Date_ Dim[Date ID]
   -Sales_ Fact[Sale ID]  → Returns_ Fact[Sale ID]
   -Customer_ Dim[Region] → Region_ Dim[Region Name]   
-   Set cardinality = One to Many, filter = Single.
-   Arrange tables in star schema (Sales_ Fact in center).
-   Hide ID columns (Customer ID, Product ID, Date ID, Sale ID).
-   Keep clean naming (Customer_ Dim, Product_ Dim, etc.).

2.  DAX MEASURES :

- Total Units = SUM(Sales_ Fact[Units Sold])
- Total Sales All = CALCULATE([Total Sales],ALL(Sales_ Fact))
- Total Sales = SUM(Sales_ Fact[Total Amount])
- Target Value = [Total Sales] * 1.1
- Sales Value = SUMX(Sales_ Fact, Sales_ Fact[Units Sold] * RELATED(Product_ Dim[Price]))
- Sales KPI = SWITCH(TRUE(),[Total Sales] > 1000000, "Excellent",[Total Sales] > 500000, "Good", "Needs Improvement")
- Returned Sales = CALCULATE([Total Sales],FILTER(Sales_ Fact, Sales_ Fact[Sale ID] IN VALUES(Returns_ Fact[Sale    ID])))
- Return Count = COUNT(Returns_ Fact[Return ID])
- Order Count = COUNT(Sales_ Fact[Customer ID])
- Customer Count = COUNTX(VALUES(Sales_ Fact[Customer ID]),Sales_ Fact[Customer ID])
- Avg Sale Value = AVERAGEX(Sales_ Fact, Sales_ Fact[Total Amount])
- Avg Returns per Sale = DIVIDE(COUNT(Returns_ Fact[Return ID]),DISTINCTCOUNT(Sales_ Fact[Sale ID]))

3.  CALCULATED COLUMNS : 

- Profit Margin Classification :
  - Cost = Sales_ Fact[Total Amount] * 0.7
  - Profit = [Total Amount] - [Cost]
  - Profit Margin = DIVIDE([Profit], [Total Amount])

- Customer Full Name : 
  - Full Name = Customer_ Dim[FirstName] & " " & Customer_ Dim[Last Name]

- YEAR-MONTH Formatting : 
  - Year-Month = FORMAT(Date_ Dim[Date], "YYYY-MM")

4.  TIME INTELLIGENCE

- YTD (Year-To-Date)
  - Sales YTD
  - Sales YTD = TOTALYTD([Total Sales],Date_ Dim[Date])

  - Returns YTD
  - Returns YTD = TOTALYTD([Return Count],Date_ Dim[Date])

- YOY (Year Over Year)
  - Last Year Sales
  - Sales LY = CALCULATE([Total Sales],SAMEPERIODLASTYEAR(Date_ Dim[Date]))
 
  - YOY Growth %
  - Sales YOY % = DIVIDE([Total Sales] - [Sales LY],[Sales LY])
  
  - Returns YOY
  - Returns LY = CALCULATE([Return Count],SAMEPERIODLASTYEAR(Date_ Dim[Date]))

  - Returns YOY % = DIVIDE([Return Count] - [Returns LY],[Returns LY])

- MOM (Month Over Month)
  - Previous Month Sales
  - Sales PM = CALCULATE([Total Sales],DATEADD(Date_ Dim[Date], -1, MONTH))

  - MOM %
  - Sales MOM % = DIVIDE([Total Sales] - [Sales PM],[Sales PM])

  - Returns MOM
  - Returns PM = CALCULATE([Return Count],DATEADD(Date_ Dim[Date], -1, MONTH))
 
  - Returns MOM % = DIVIDE([Return Count] - [Returns PM],[Returns PM])

5.  DASHBOARD LAYOUT

-   Create 1 Main Page, 2 Detail Pages, 1 Drill through Page.
-   Add visuals: Cards (Total Sales, Orders, Return Rate) KPI Cards
    (Sales Trend) Line Chart (Sales Trend) Bar Chart (Sales by Category)
    Donut Chart (Sales by Segment)
-   Add Matrix: Rows = Region, Category Values = Sales, Return Rate, YOY
    %

6.  FILTERING & INTERACTION

-   Add slicers: Product Category Customer Segment Region Date
-   Enable Drill Down and Drill through.

7.  NAVIGATION & UX

-   Add buttons for page navigation.
-   Use bookmarks.
-   Add tooltips.
-   Apply conditional formatting in matrix.

8.  MOBILE LAYOUT

-   Optimize main page for mobile.
-   Keep KPI cards on top.

9.  SECURITY

-   Create roles by Region.
-   Apply Row Level Security using Region field.



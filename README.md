# Restaurant-Tips-Analysis

This project is aimed at analyzing a dataset containing information about tips earned by employees at a restaurant. The dataset contains data on the date, shift, name of employee, amount of tips earned, training day, and sick day. The goal of this project is to understand the trends of tips earned by employees and provide insights to the business owner. This project will be done by analyzing the data and providing visual representation of the data, by excluding the rows where the employees earned 0 in tips during training or sick days. The project will also include SQL commands that will be used to extract, manipulate and analyze the data from the dataset using PostgreSQL as the main database. The project will be implemented and visualized with Tableau.

This report will begin by explaining the process of cleaning and preparing the dataset for analysis and how it was imported to PostgreSQL. Then, it will cover the different analyses that are performed in order to answer the questions posed in the project. The report will also include SQL commands used in the project, results and conclusions, and suggestions for future work. Additionally, the report will also show how Tableau was used to visualise the data and make it more interactive and user-friendly.

The goal of this project is to answer the following questions:
- What is the average amount of tips earned per shift?
- What is the trend of tips over time?
- Is there a relationship between the day of the week and the amount of tips earned?
- Are there any specific times of day or week when the restaurant tends to earn more tips?
- Are there any specific employees that tend to earn more tips than others?

## Data Cleaning

Before analyzing the dataset, it was necessary to clean and prepare the data for analysis. The following steps were taken in Excel to clean the data:

Adding additional rows: Additional columns were added to the dataset such as "month", "day" and "month number" using Excel formulas such as "MONTH()" and "DAY()".

Saving the cleaned dataset: The cleaned dataset was saved as a new Excel file, and it was imported into Tableau and PostgreSQL for the analysis and visualization.

By cleaning the data, we were able to ensure that the dataset was consistent and accurate, and that it could be easily imported into other tools for analysis. Additionally, because there were no duplicate rows or missing values in the original dataset, those steps were not necessary. Any outliers present in the dataset were kept for the analysis.

## Preliminary Analysis

A preliminary analysis of the dataset was conducted using Excel to understand the trends in tips earned by employees. The following steps were taken:

Pivot tables: Pivot tables were created in Excel to look at the average amount of tips earned per shift and per employee. These pivot tables were useful to get a general overview of the data and identify any patterns or trends.

Charts: Charts were created to visualize the data and make it easier to understand. Line charts were created to show the trend of tips earned over time and bar charts were created to compare the average amount of tips earned per shift and per employee.

Filtering the data: A filter was applied to exclude the rows where the employees earned 0 in tips during training or sick days.

Data manipulation: The data was further manipulated to group it by day of the week, time and other variables.

Insights: From the pivot tables and charts, insights were gathered on the average amount of tips earned per shift, the trend of tips over time, and the relationship between day of the week and the amount of tips earned.

This preliminary analysis provided a good starting point for further analysis in Tableau and SQL. The insights gathered from this analysis can be used to inform the business owner on how to increase the tips earned.

## Data Analysis: SQL

SQL was used to extract, manipulate, and analyze the data from the dataset. The following steps were taken:

Importing the data: The cleaned dataset was imported into PostgreSQL using SQL commands such as CREATE TABLE and INSERT INTO.

Querying the data: SQL commands such as SELECT, FROM, WHERE, GROUP BY and HAVING were used to query the data and extract relevant information.

Data manipulation: SQL commands such as CASE, WHEN, THEN, and ELSE were used to manipulate the data and group it by different variables such as shift, day of the week, and employee name.

Aggregate functions: SQL aggregate functions such as SUM, AVG, COUNT, MIN, and MAX were used to calculate statistics such as the average amount of tips earned per shift and per employee.

Filtering the data: SQL commands such as WHERE were used to filter the data and exclude the rows where the employees earned 0 in tips during training or sick days.

### Sample Code
#### Query 1: 
#### Query 2: Avg tips
```
WITH cte AS(
	SELECT *
		, DATE_PART('dow', "Date")                AS day_num                        --Day in number form, for analysis
		, TO_CHAR("Date", 'Day')                  AS day_of_week                    --Day of the week
		, DATE_PART('month', "Date")              AS month_num                      --Month in number form, for analysis
		, TO_CHAR("Date", 'Month')                AS tip_month                      --Month of the year
	FROM tip_data 
	WHERE "Amount" > 0
	)

SELECT "Date", "Shift", "Name", "Amount", "day_of_week", "tip_month"
	, ROUND(AVG("Amount") OVER(
		PARTITION BY("Name")),2)                  AS avg_tip_per_employee           --Avg amount each employee makes in tips out of all the shifts they worked
	, ROUND(AVG("Amount") OVER(
		PARTITION BY("Shift")),2)                 AS avg_tip_per_shift              --Avg amount all employees make in tips on that specific shift
	, ROUND(AVG("Amount") OVER(
		PARTITION BY("day_of_week")),2)           AS avg_tip_per_day                --Avg amount all employees make in tips on that specfic day
	, ROUND(AVG("Amount") OVER(
		PARTITION BY("day_of_week", "Shift")),2)  AS avg_tip_per_day_and_shift      --Avg amount all employees make in tips on that specific day and shift
	, ROUND(AVG("Amount") OVER(
		PARTITION BY("tip_month")),2)             AS avg_tip_per_month              --Avg amount all employees make in tips on that specific month
	, ROUND(AVG("Amount") OVER(
		PARTITION BY("tip_month", "Shift")),2)    AS avg_tip_per_month_and_shift    --Avg amount all employees make in tips on that specific month and shift
	, SUM("Amount") OVER(
		PARTITION BY("Name"))                     AS total_earned_per_employee      --Total tips earned per each employee
	, SUM("Amount") OVER(
		PARTITION BY("Name", "day_of_week"))      AS total_earned_per_emp_day       --Total tips earned per each employee on that specific day
	, SUM("Amount") OVER(
		PARTITION BY("day_of_week"))              AS total_earned_day               --Total tips earned on that specific day	
	, SUM("Amount") OVER(
		PARTITION BY("day_of_week", "Shift"))     AS total_earned_day_and_shift     --Total tips earned on that specific day and shift
	, SUM("Amount") OVER(
		PARTITION BY("tip_month"))                AS total_earned_month             --Total tips earned on that specific month
	, SUM("Amount") OVER(
		PARTITION BY("tip_month", "Shift"))       AS total_earned_month_and_shift   --Total tips earned on that specific month and shift
FROM cte 
ORDER BY "Date", "Shift" DESC, "Name"
```
Insights: From the SQL queries, insights were gathered on the average amount of tips earned per shift, the trend of tips over time, and the relationship between day of the week and the amount of tips earned.

Saving the results: The results of the SQL queries were saved in a new table or exported to a CSV file to be further analyzed in Tableau or Excel.



By using SQL to analyze the data, we were able to extract and manipulate the data in a more efficient and precise manner. The insights gathered from the SQL queries provided a deeper understanding of the trends in tips earned by employees and can be used to inform the business owner on how to increase the tips earned.

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

### Sample Query
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
### Sample results
![Screenshot (41)](https://user-images.githubusercontent.com/91815051/213258198-dad86221-1642-4a20-9803-683119422f58.png)

Insights: From the SQL queries, insights were gathered on the average amount of tips earned per shift, the trend of tips over time, and the relationship between day of the week and the amount of tips earned.

Saving the results: The results of the SQL queries were saved in a new table or exported to a CSV file to be further analyzed in Tableau or Excel.

By using SQL to analyze the data, we were able to extract and manipulate the data in a more efficient and precise manner. The insights gathered from the SQL queries provided a deeper understanding of the trends in tips earned by employees and can be used to inform the business owner on how to increase the tips earned.

## Data Analysis: Tableau
Tableau was used to create interactive and visual representations of the data. The following steps were taken:

Connecting to the data source: The cleaned dataset was imported into Tableau.

Creating visualizations: Various visualizations were created in Tableau such as line charts, bar charts, and heat maps to represent the data.

Filtering the data: A filter was applied in Tableau to exclude the rows where the employees earned 0 in tips during training or sick days.

Formatting the visualizations: The visualizations were formatted and customized to make them more visually appealing and easy to understand.

Insights: From the visualizations, insights were gathered on the average amount of tips earned per shift, the trend of tips over time, and the relationship between day of the week and the amount of tips earned.

Creating dashboards: The visualizations were grouped into dashboards to allow for easy comparison and analysis.

### Dashboards
![Story 1](https://user-images.githubusercontent.com/91815051/213259678-d8458b3b-6133-406f-80ef-8dc31739e274.png)

![Story 2](https://user-images.githubusercontent.com/91815051/213259702-e10c87e1-aab7-478b-8967-25a3075aad0e.png)

![Story 3](https://user-images.githubusercontent.com/91815051/213259717-fc00d354-a3ec-4105-a5a0-94ee4ff4d4d9.png)

Sharing the results: The dashboards were shared with the business owner to allow for easy exploration and analysis of the data.

By using Tableau to analyze the data, we were able to create interactive and visually appealing representations of the data that helped to understand the trends in tips earned by employees and provided insights to the business owner on how to increase the tips earned.

## Results and Conclusion
The analysis of the dataset revealed several key insights on the trends of tips earned by employees at the restaurant.

- The average amount of tips earned per shift was found to be highest during the lunch shift and lowest dinner the morning shift.
- The trend of tips over time showed that the restaurant earned more tips in the early months of the year compared to the later months.
- The analysis also revealed that the restaurant tended to earn more tips on weekends than on weekdays.
- The heat map visualization showed that the restaurant tended to earn more tips during the early hours, particularly on Monday and Saturdays.
- The bar chart visualization showed that some employees tended to earn more tips than others.

Based on these findings, it can be concluded that the restaurant can potentially increase tips earned per employee by promoting lunch shifts, targeting weekends and lunch hours, as well as providing incentives to top performers employees. Additionally, the restaurant could also consider running promotions or events during the months where tips were found to be lower.

It's worth noting that these results should be taken with a grain of salt since the dataset used in this analysis is not the complete picture of the restaurant's performance and there could be other factors that influence the tips earned. Therefore, the business owner should consider these insights as a starting point for further analysis and testing.

Future work could include collecting more data and analyzing it over a longer period of time, as well as testing different strategies to increase tips earned and measuring their effectiveness.

## Future Work
While this analysis has provided a good starting point for understanding the trends of tips earned by employees at the restaurant, there are a few areas where future work could be done to gain a more comprehensive understanding of the data. Some suggestions include:

Collecting more data over a longer period of time: By collecting more data, it would be possible to gain a more detailed understanding of the patterns and trends in the data, and to identify any seasonal or annual fluctuations in tips earned.

Testing different strategies to increase tips earned: Based on the insights gained from this analysis, the restaurant could test different strategies for increasing tips earned, such as providing incentives to top performers employees, or running promotions during the months where tips were found to be lower.

Measuring the effectiveness of different strategies: By measuring the effectiveness of different strategies, it would be possible to determine which strategies are most effective at increasing tips earned, and to make data-driven decisions about how to increase tips in the future.

Incorporating external data: Incorporating external data such as weather, local events, and economic indicators, may provide a more comprehensive understanding of the factors that influence the tips earned by the restaurant.

Incorporating Machine learning: Incorporating Machine learning techniques such as, time-series forecasting, anomaly detection and prediction models, can be used to predict future trends of tips earned by employees.

By continuing to analyze and test different strategies, the restaurant can use data to make informed decisions about how to increase tips earned and improve the overall performance of the business.


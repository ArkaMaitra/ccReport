Credit Card Transaction and Customer Analysis
This Project aims to address all the insights and patterns of behaviors of credit card usage by using Power BI, Excel and SQL. We'll look at all the crucial metrics like customer age, sexual orientation, marital status, their jobs, education level etc. By the end we'll know how their patterns of using credit cards to better understand and predict their needs.
Table of Contents :
● Introduction
● Problem Statement
● Data Collection
● Importing The Data
● Data Alteration
● Dashboard Preparation
● Insights
● Conclusion
Introduction
We are showing all the data insights and graphs of all the transactions and customer details for 1 year.
All the data can be sorted and viewed through weekly for better convenience.
There are 2 different insights reports for transaction and customer analysis respectively.
Problem Statement
The company wants to better understand their business and wants data insights for all 4 quarters to streamline and focus on the aspects that needs their most attention.
Data Collection
The datasets used for this analysis is a structured data in CSV format and contains 10,109 rows and 18 columns.
Transaction Dataset : credit_card.csv
Overview of the Transaction datasetColumn Description :
● client_Num : It is the client's transaction number.
● card_Category : It is the type of card that the client used.
● Annual_Fees : It is the fees that the client paid for the card annually.
● Activation_30_Days : It is the binary indication of whether the client activated their card within 30 days or not.
● Customer_Acq_Cost : It is the amunt that went towards acquiring the client.
● Week_Start_Date : It is the date of the first day of each week of the year.
● Week_Num : It is the rolling count of the number of weeks.
● Qtr : It indicates the quarter of the year.
● current_year : It is the operation year.
● Credit_Limit : It is the amount limit of the client's card.
● Total_Revolving_Bal : It is the balance amount that carries over from one month to the next month.
● Total_Trans_Amt : It is the amount that has been transacted by the client.
● Total_Trans_Vol : It is the number of times that the client has trasacted from their card.
● Avg_Utilization_Ratio : It is the ratio of the amount of revolving credit and the total available credit to the client.
● Use_Chip : It is to indicate what type of method the client used for their card.
● Exp_Type : It is to find out the type of bills that the client used their card for.
● Interest_Earned : It is the amount of interest that the company earned through the client's card.
● Delinquent_Acc : It is a binary indication that flags an account if a payment if not paid for 30 days or more.
Customer Dataset : customer.csv
Overview of the Customer datasetColumn Description :
● Client_Num : It is the client's transaction number.
● Customer_Age : It is the client's age.
● Gender : It is the client's gender.
● Dependent_Count : It is the number of people that the client supports financially with their card.
● Education_Level : It is the highest level of education that the client has received.
● Marital_Status : It is the client's marital status.
● state_cd : It is the short code of the state that the client resides in.
● Zipcode : It is the zip code of the place that the client resides in.
● Car_Owner : It is the indication to find out if the client owns a car or not.
● House_Owner : It is the indication to find out if the client owns a house or not.
● Personal_loan : It is the indication to find out if the client has personal loan or not.
● contact : It is the type of contact that the client has provided.
● Customer_Job : It is the job that client has.
● Income : It is amount that the client earns.
● Cust_Satisfaction_Score : It is the indication to find out the satisfaction score that the client has provided for the service.
Importing The Data
Instead of importing the data straight to Power BI for analysis, I decided to chain the data through an SQL server first. This way, if any new data gets added to the dataset, then all the visualizations will get updated automatically with respect to the new data.
I used PostgreSQL as my preferred choice for server system.
Step 1 : Creating a New Database
Created a database using the Query tool in pgAdmin
Step 2 : Creating New Tables
Created two tables that have the same columns as the source csv files for both the transaction and customer details files.
Step 3 : Inserting The Values
Now, I inserted all the raw data from the source files to both of the newly created tables.
Data Alteration
Before diving into the analysis and visualization, I added some extra columns and measures to make some processes easier. I used the inbuild DAX tool in Power BI to make them.
New Columns
1. Revenue
Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[interest_earned] + 'public cc_detail'[total_trans_amt]
2. Revenue
Week_no = WEEKNUM('public cc_detail'[week_start_date])
3. AgeGroup
AgeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] > 60, "60+",
    "unknown"
    )
4. IncomeGroup
IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Mid",
    'public cust_detail'[income] > 70000, "High",
    "unknown"
)
New Measures Column Description :
1. WoW_revenue
WoW_revenue = DIVIDE('public cc_detail'[Current_week_revenue] - 'public cc_detail'[Previous_week_revenue], [Previous_week_revenue])
2. Current_week_revenue
Current_week_revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(ALL('public cc_detail'),
    'public cc_detail'[Week_no] = MAX('public cc_detail'[Week_no])))
3. Previous_week_revenue
Previous_week_revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(ALL('public cc_detail'),
    'public cc_detail'[Week_no] = MAX('public cc_detail'[Week_no])-1))
I added all the column for aggregating some column ranges for better visualization and added the measures to better understand the week on week revenue insights.
Dashboard Preparation
I devided the visualization in two parts as well. One for the detailed analysis of all the transaction and the other for all the customers of the company.
Transaction Analysis
KPIs : There are 4 main KPIs present. They are Revenue, Interest, Transacted Amount & Count of Transaction.
Filters : For filters I used Card Type, Quarter, Customer Age Groups & their genders.
Slicer : I used Week numbers as a slicer.

Transaction AnalysisCustomer Analysis
KPIs : There are 4 main KPIs present here as well. They are Revenue, Interest as before, and with them there is also Customer's Total Income & CSS or Customer Satisfaction Score.
Filters : Here, for filters I used Customer gender as a general divider between almost all the data. I also used Card Type & Quarter as before, but introduced Customer Incomer Groups.
Slicer : I used Week numbers as the slicer here as well.

Customer AnalysisInsights
● Overall revenue is 55M.
● Total interest is 8M.
● Total transaction amount is 46M.
● Male customers are contributing 31M in revenue, while female customers are pulling in 26M.
● In Low Income Groups, the usage of credit cards by male customers is very low, but in high income groups , the usage of credit cards by male customers is significantly higher.
● Blue & Silver credit card together are contributing to 93% of all transactions.
● Between all the states TX, NY & CA is contributing to 68%
● Card Activation rate is 57.47%
● The Delinquent rate is 6.07%
Conclusion
In conclusion, the analysis of all the customers and their transaction done in this project showcases valuable, clear and decisive insights. We have found out how, what and why revenue and overall customer retention seems affected and then can enforce steps that corrects the shift based on the visualizations. these key trends and patterns in customer behavior will help businesses make knowledgeable decisions.
Thanks for your time (I know it was probably a long read, sorry). Have a great rest of the day!

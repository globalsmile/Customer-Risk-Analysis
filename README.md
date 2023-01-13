# Pwc Switzerland Power BI virtual case experience - Customer Risk Analysis
<img align="right" alt="Customer Risk Analysis" width="1000" height = "500" src="https://user-images.githubusercontent.com/106287208/194933469-e31e05b4-9c23-48d7-96fe-ae0ecdd25897.png">

---


# Table of Contents

- [Problem Statement](https://github.com/globalsmile/Customer-Risk-Analysis#Problem-Statement)
- [Data Sourcing](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Sourcing)
- [Data Preparation](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Preparation)
- [Data Modeling](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Modeling)
- [Data Visualization](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Visualization)
- [Data Analysis](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Analysis)
- [Insights](https://github.com/globalsmile/Customer-Risk-Analysis#Insights)
- [Recommendation](https://github.com/globalsmile/Customer-Risk-Analysis#Recommendation)
- [Shareable link](https://github.com/globalsmile/Customer-Risk-Analysis#Shareable-Link)


---

# Problem Statement

The purpose of this analysis is to: 
- Define proper KPIs
- Create a dashboard for the retention manager that reflects all relevant Key Performance indicators(KPIs)
and metrics in the dataset.
- Based on your findings, recommend suggestions as to what needs to be changed

Here are some inputs:
- Customers who left within the last month
- Services each customer has signed up for: phone, multiple lines, internet, online security, online backup, device protection, tech 
support, and streaming TV and movies
- Customer account information: how long as a customer, contract, payment method, paperless billing, monthly charges, total charges 
and number of tickets opened in the categories administrative and technical
- Demographic info about customers – gender, age range, and if they have partners and dependents

---

# Data Sourcing

The Dataset used for this analysis was presented by [Pwc Switzerland](https://www.pwc.ch/en/careers-with-pwc/students/virtual-case-experience.html) and available at:

- [Churn Dataset](https://github.com/globalsmile/Customer-Risk-Analysis/blob/main/02%20Churn-Dataset.xlsx)



---

# Data Preparation

Data transformation was done in Power Query and the dataset was loaded into Microsoft Power BI Desktop for modeling.

The customer retention dataset is given by a table named:

- `churn` which has `23 columns and 7043 rows` of observation


The tabulation below shows the `churn` table with its column names and their description:
| Column Name | Description |
| ----------- | ----------- |
| customerID |   Represents the unique number of the customer in the dataset|
| gender | Describes the gender of the customer |
| SeniorCitizen | Describes if the customer is a senior citizen |
| Partner | Describes if the customer has a partner |
| Dependents | Describes if the customer has a dependent |
| tenure | Describes how long as a customer |
| PhoneService | Describes if the customer has registered a phone service |
| MultipleLines | Describes if the customer has registered multiple lines |
| InternetService | Describes if the customer has registered for internet service |
| OnlineSecurity | Describes if the customer has registered for online security  |
| OnlineBackup | Describes if the customer has registered for online backup |
| DeviceProtection | Describes if the customer has registered for device protection |
| TechSupport | Describes if the customer has registered for tech support |
| StreamingTV | Describes if the customer has registered  to stream tv|
| StreamingMovies | Describes if the customer has registered to stream movies |
| Contract | Describes if the length of the contract of the customer |
| PaperlessBilling | Describes if the customer has registered for paperless billing |
| PaymentMethod | Describes the payment method of the customer |
| MonthlyCharges | Represents the monthly charge incurred by the customer |
| TotalCharges | Represents the total charge incurred by the customer |
| numAdminTickets | Represents the number of admin tickets opened by the customer |
| numTechTickets | Represents the number of tech tickets opened by the customer |
| Churn | Describes if the customer is at risk of churn |

Data Cleaning for the dataset was done in power query as follows:

A new table named `churn - unpivot groups` was created by duplicating the `churn` table and unpivoting unnecessary columns.
In the new table, 2 additional conditional columns were added using M-formula:


- Loyalty (which describes how long as a customer): ` Table.AddColumn(#"Added Conditional Column", "Loyalty", each if [tenure] < 12 then "< 1 year" else if [tenure] < 24 then "< 2 years" else if [tenure] < 36 then "< 3 years" else if [tenure] < 48 then "< 4 years" else if [tenure] < 60 then "< 5 years" else "< 6 years")` 


- Risk category: ` Table.AddColumn(#"Changed Type", "Risk category", each if [tenure] < 24 then "Risk 1" else if [tenure] < 48 then "Risk 2" else "Risk 3Table.AddColumn(#"Changed Type", "Risk category", each if [tenure] < 24 then "Risk 1" else if [tenure] < 48 then "Risk 2" else "Risk 3"Table.AddColumn(#"Changed Type", "Risk category", each if [tenure] < 24 then "Risk 1" else if [tenure] < 48 then "Risk 2" else "Risk 3")`

Further cleansing was done as follows:

- Unnecessary columns were removed 
- Each of the columns in the table were validated to have the correct data type 

---

# Data Modeling

After the dataset was cleaned and transformed, it was ready to be modeled.

- A one-to-many (*:1) relationship was created between the `churn` and the `churn - unpivot groups` tables using the customerId column in each of the tables
- The relationship formed in the data model is shown below:

<img align="right" alt="Data Model" width="1000" height = "400" src="https://user-images.githubusercontent.com/106287208/194932956-140622b5-f861-4220-80cf-6a57c080b983.JPG">


---

# Data Visualization

Data visualization for the dataset was done in 3 folds using Microsoft Power BI Desktop:

-  The `Welcome` Page: Shows recommendations and a short insight on the report
-  The `Churn` Page: Shows the churn dashboad containing # of customers at risk, # of Tech Tickets, # of Admin Ticket, Yearly Charges, Monthly charges, Demographics, e.t.c 
-  The `Customer Risk Analysis` Page: Shows the # of customers, the churn rate, churn by type of internet service, type of contract, e.t.c

Figure 1 shows visualizations from `Welcome` page

| Figure 1 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/194933273-39523822-31dd-46b7-80e7-153273d5a599.png) |

Figure 2 shows visualizations from `Churn` page

| Figure 2 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/194933469-e31e05b4-9c23-48d7-96fe-ae0ecdd25897.png) |

Figure 3 shows visualizations from `Customer Risk Analysis` page

| Figure 3 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/194933633-d0089c62-02fd-4f39-9bf0-3ef5ca08be4b.png) |


---

# Data Analysis

Measures used in visualization are:

- churn rate % = `DIVIDE (CALCULATE(COUNT(churn[Churn]), churn[Churn] = "yes" ),CALCULATE ( COUNT (churn[Churn]), ALLSELECTED (churn[Churn] ) )
)`


- Count of Churn for Yes =` CALCULATE(COUNTA('churn'[Churn]), 'churn'[Churn] IN { "Yes" })`


- Count of DeviceProtection for Yes = 
`CALCULATE(
	COUNTA('churn'[DeviceProtection]),
	'churn'[DeviceProtection] IN { "Yes" }
)`


- Count of MultipleLines for Yes = 
`CALCULATE(
	COUNTA('churn'[MultipleLines]),
	'churn'[MultipleLines] IN { "Yes" }
)`


- Count of OnlineBackup for Yes = 
`CALCULATE(
	COUNTA('churn'[OnlineBackup]),
	'churn'[OnlineBackup] IN { "Yes" }
)`


- Count of OnlineSecurity for Yes = 
`CALCULATE(
	COUNTA('churn'[OnlineSecurity]),
	'churn'[OnlineSecurity] IN { "Yes" }
)`


- Count of PhoneService for Yes = 
`CALCULATE(
	COUNTA('churn'[PhoneService]),
	'churn'[PhoneService] IN { "Yes" }
)`


- Count of StreamingMovies for Yes = 
`CALCULATE(
	COUNTA('churn'[StreamingMovies]),
	'churn'[StreamingMovies] IN { "Yes" }
)`


- Count of StreamingTV for Yes = 
`CALCULATE(COUNTA('churn'[StreamingTV]), 'churn'[StreamingTV] IN { "Yes" })`


- Count of TechSupport for Yes = 
`CALCULATE(COUNTA('churn'[TechSupport]), 'churn'[TechSupport] IN { "Yes" })`


- Dependents in % = `DIVIDE(CALCULATE(COUNT(churn[Dependents]), churn[Dependents]="Yes", churn[Churn]= "Yes"), CALCULATE(COUNT(churn[Dependents]), churn[Churn]="Yes"),0)`


- Device protection in % = `DIVIDE(CALCULATE(COUNT(churn[DeviceProtection]), churn[DeviceProtection]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[DeviceProtection]),churn[Churn]="Yes"),0)`


- Multiple lines no in % =` DIVIDE(CALCULATE(COUNT(churn[MultipleLines]), churn[MultipleLines]="No", churn[Churn]="Yes"),CALCULATE(COUNT(churn[MultipleLines]),churn[Churn]="Yes", churn[MultipleLines] <> "No phone service"),0)`


- Multiple Lines yes in % =` DIVIDE(CALCULATE(COUNT(churn[MultipleLines]), churn[MultipleLines]="Yes", churn[Churn]= "Yes"), CALCULATE(COUNT(churn[MultipleLines]), churn[Churn]="Yes", churn[MultipleLines] <> "No phone service"),0)`


- Online Bacup in % = `DIVIDE(CALCULATE(COUNT(churn[OnlineBackup]), churn[OnlineBackup]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[OnlineBackup]),churn[Churn]="Yes"),0)`


- Online Sec. in % =` DIVIDE(CALCULATE(COUNT(churn[OnlineSecurity]), churn[OnlineSecurity]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[OnlineSecurity]),churn[Churn]="Yes"),0)`


- Partner in % =` DIVIDE(CALCULATE(COUNT(churn[Partner]), churn[Partner]="Yes", churn[Churn]= "Yes"), CALCULATE(COUNT(churn[Partner]), churn[Churn]="Yes"),0)`


- Phone Service in % = `DIVIDE(CALCULATE(COUNT(churn[PhoneService]), churn[PhoneService]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[PhoneService]),churn[Churn]="Yes"),0)`


- SeniorCitizen in % =` DIVIDE(CALCULATE(COUNT(churn[SeniorCitizen]), churn[SeniorCitizen]=1, churn[Churn]= "Yes"), CALCULATE(COUNT(churn[SeniorCitizen]), churn[Churn]="Yes"),0)`


- Streaming Movies in % = `DIVIDE(CALCULATE(COUNT(churn[StreamingMovies]), churn[StreamingMovies]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[StreamingMovies]),churn[Churn]="Yes"),0)`


- Streaming TV in % = `DIVIDE(CALCULATE(COUNT(churn[StreamingTV]), churn[StreamingTV]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[StreamingTV]),churn[Churn]="Yes"),0)`


- Tech Support in % = `DIVIDE(CALCULATE(COUNT(churn[TechSupport]), churn[TechSupport]="Yes", churn[Churn]="Yes"),CALCULATE(COUNT(churn[TechSupport]),churn[Churn]="Yes"),0)`


As shown from [Data Visualization](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Visualization), It can be deduced that:

- `7043` customers are at risk of churn 
- The churn rate is `26.54%` 
- The yearly charges is `$16.06M` 

---

# Insights

As shown by [Data Visualization](https://github.com/globalsmile/Customer-Risk-Analysis#Data-Visualization), It can be deduced that:

- `2955` tech tickets were opened
- `3632` admin tickets were opened
- The monthly charges is `$456.12K` 

---

# Recommendation

- Increase tech support   capacity for Fiber Optic customers and lower tech tickets per customer to 0.5

- Increase sale of 1 and 2 year contracts by 5% each

- Yearly increase of automatic payments by 5%


---


# Shareable Link

You can interact with the report here: 

[View Report](https://app.powerbi.com/view?r=eyJrIjoiMjg1NGRmN2UtYzVkNC00MmM5LWI4MWItNzhjYjhhOWU0ZWQwIiwidCI6IjQ5ODY4YWYzLWNjNWYtNDIxNC04YjdmLTQwZjM3NDY0OWEwOSJ9)

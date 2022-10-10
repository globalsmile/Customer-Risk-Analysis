# Pwc Switzerland Power BI virtual case experience - Customer Risk Analysis
<img align="right" alt="Twitter Sentiment Analysis" width="1000" height = "400" src="https://user-images.githubusercontent.com/106287208/187567112-ec593f73-7afe-4ef3-8c80-822591b89038.png">

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

INTRO :This project is the second of three projects from the PWC SWITZERLAND POWER BI VIRTUAL CASE EXPERIENCE INTERNSHIP PROGRAM on Forage.

The purpose of this analysis is to: 
- Define proper KPIs
- Create a dashboard for the retention manager that reflects all relevant Key Performance indicators(KPIs)
and metrics in the dataset.
- Based on your findings, recommend suggestions as to what needs to be changed

Here are some inputs
- Customers who left within the last month
- Services each customer has signed up for: phone, multiple lines, internet, online security, online backup, device protection, tech 
support, and streaming TV and movies
•- Customer account information: how long as a customer, contract, payment method, paperless billing, monthly charges, total charges 
and number of tickets opened in the categories administrative and technical
•- Demographic info about customers – gender, age range, and if they have partners and dependents

---

# Data Sourcing

- The dataset used for this analysis was scrapped from twitter using python and jupyter notebook
- The preview of the dataset and python code is shown below:


```python
!pip install snscrape
```

    Requirement already satisfied: snscrape in c:\users\user\anaconda3\lib\site-packages (0.4.3.20220106)
    Requirement already satisfied: lxml in c:\users\user\anaconda3\lib\site-packages (from snscrape) (4.8.0)
    Requirement already satisfied: beautifulsoup4 in c:\users\user\anaconda3\lib\site-packages (from snscrape) (4.11.1)
    Requirement already satisfied: requests[socks] in c:\users\user\anaconda3\lib\site-packages (from snscrape) (2.27.1)
    Requirement already satisfied: filelock in c:\users\user\anaconda3\lib\site-packages (from snscrape) (3.6.0)
    Requirement already satisfied: soupsieve>1.2 in c:\users\user\anaconda3\lib\site-packages (from beautifulsoup4->snscrape) (2.3.1)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (3.3)
    Requirement already satisfied: charset-normalizer~=2.0.0 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (2.0.4)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (1.26.9)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (2021.10.8)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in c:\users\user\anaconda3\lib\site-packages (from requests[socks]->snscrape) (1.7.1)
    


```python
import pandas as pd
import snscrape.modules.twitter as sntwitter
```


```python
query = "(#30DaysOfLearning OR #NG30DaysOfLearning) until:2022-06-26 since:2022-05-05"
tweets = []
limit = 30000


for tweet in sntwitter.TwitterHashtagScraper(query).get_items():
    
    if len(tweets) == limit:
        break
    else:
        tweets.append([tweet.date, tweet.url, tweet.user.username, tweet.sourceLabel, tweet.user.location, tweet.content, tweet.likeCount, tweet.retweetCount,  tweet.quoteCount, tweet.replyCount])
        
df = pd.DataFrame(tweets, columns=['Date', 'TweetURL','User', 'Source', 'Location', 'Tweet', 'Likes_Count','Retweet_Count', 'Quote_Count', 'Reply_Count'])

df.to_csv('30DLTweets.csv')
```


```python
df.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>TweetURL</th>
      <th>User</th>
      <th>Source</th>
      <th>Location</th>
      <th>Tweet</th>
      <th>Likes_Count</th>
      <th>Retweet_Count</th>
      <th>Quote_Count</th>
      <th>Reply_Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-06-25 22:51:18+00:00</td>
      <td>https://twitter.com/poetrineer/status/15408300...</td>
      <td>poetrineer</td>
      <td>Twitter for Android</td>
      <td>Oyo, Nigeria</td>
      <td>So as one of my commitment to document my lear...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-06-25 22:44:10+00:00</td>
      <td>https://twitter.com/poetrineer/status/15408282...</td>
      <td>poetrineer</td>
      <td>Twitter for Android</td>
      <td>Oyo, Nigeria</td>
      <td>Finally, here is my updated COVID-19 Data Anal...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-06-25 19:25:58+00:00</td>
      <td>https://twitter.com/MichealOjuri/status/154077...</td>
      <td>MichealOjuri</td>
      <td>Twitter Web App</td>
      <td>Oyo, Nigeria</td>
      <td>#30NGDaysOfLearning\n#30daysoflearning \n#micr...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-06-25 16:44:36+00:00</td>
      <td>https://twitter.com/oye__aashu/status/15407377...</td>
      <td>oye__aashu</td>
      <td>Twitter for Android</td>
      <td>Nainital, India</td>
      <td>Day 4/ #30daysoflearning learned all about arr...</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-06-25 12:49:02+00:00</td>
      <td>https://twitter.com/hsb_data/status/1540678455...</td>
      <td>hsb_data</td>
      <td>Twitter Web App</td>
      <td>New Jersey</td>
      <td>Learning about sub queries on @DataCamp (SQL) ...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Likes_Count</th>
      <th>Retweet_Count</th>
      <th>Quote_Count</th>
      <th>Reply_Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
      <td>683.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>15.780381</td>
      <td>3.812592</td>
      <td>0.185944</td>
      <td>1.166911</td>
    </tr>
    <tr>
      <th>std</th>
      <td>41.164555</td>
      <td>12.665561</td>
      <td>0.737938</td>
      <td>2.626554</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>8.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>549.000000</td>
      <td>248.000000</td>
      <td>9.000000</td>
      <td>29.000000</td>
    </tr>
  </tbody>
</table>
</div>




The dataset is also available at [30DLTweets](https://github.com/globalsmile/Twitter-Sentiment-Analysis/blob/main/30DLTweets.csv)

---

# Data Preparation

Data transformation was done in Power Query and the dataset was loaded into Microsoft Power BI Desktop for modeling.

The customer retention dataset is contained in a table named:

- `churn` which has `23 columns and 7043 rows` of observation


The tabulation below shows the `Customer Retention` table with its column names and their description:
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
- Loyalty(which describes how long as a customer): ` Table.AddColumn(#"Added Conditional Column", "Loyalty", each if [tenure] < 12 then "< 1 year" else if [tenure] < 24 then "< 2 years" else if [tenure] < 36 then "< 3 years" else if [tenure] < 48 then "< 4 years" else if [tenure] < 60 then "< 5 years" else "< 6 years")` 
- Risk category: ` Table.AddColumn(#"Changed Type", "Risk category", each if [tenure] < 24 then "Risk 1" else if [tenure] < 48 then "Risk 2" else "Risk 3Table.AddColumn(#"Changed Type", "Risk category", each if [tenure] < 24 then "Risk 1" else if [tenure] < 48 then "Risk 2" else "Risk 3"Table.AddColumn(#"Changed Type", "Risk category", each if [tenure] < 24 then "Risk 1" else if [tenure] < 48 then "Risk 2" else "Risk 3")`

Further cleaning was done as follows:

- Unnecessary columns were removed 
- Each of the columns in the table were validated to have the correct data type 

---

# Data Modeling

After the dataset was cleaned and transformed, it was ready to be modeled.

- A one-to-many (*:1) relationship was created between the `churn` and the `churn - unpivot` tables using the customerId column in each of the tables
- The relationship formed in the data model is shown below:

<img align="right" alt="Data Model" width="1000" height = "400" src="https://user-images.githubusercontent.com/106287208/187106255-bd25a422-fd74-4f5e-a529-7a9052915252.png">


---

# Data Visualization

Data visualization for the datasets was done in 3 folds using Microsoft Power BI Desktop:

-  The `Welcome` Page: Shows recommendations and a short insight on the report
-  The `Churn` Page: Shows the churn dashboad containing # of customers at risk, # of Tech Tickets, # of Admin Ticket, Yearly Charges, Monthly charges, Demographics, e.t.c 
-  The `Customer Risk Analysis` Page: Shows the # of customers, the churn rate, churn by type of internet service, type of contract, e.t.c

Figure 1 shows visualizations from `Welcome` page

| Figure 1 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/187567316-46bc6332-7507-4f11-b3c7-a18a52ed8e14.png) |

Figure 2 shows visualizations from `Churn` page

| Figure 2 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/187567316-46bc6332-7507-4f11-b3c7-a18a52ed8e14.png) |

Figure 3 shows visualizations from `Customer Risk Analysis` page

| Figure 3 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/187567316-46bc6332-7507-4f11-b3c7-a18a52ed8e14.png) |



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

As shown by [Data Visualization](https://github.com/globalsmile/Twitter-Sentiment-Analysis#Data-Visualization), It can be deduced that:

-`2955` tech tickets were opened
-`3632` admin tickets were opened
- The monthly charges is `$456.12K` 

---

# Recommendation

- Increase tech support   capacity for Fiber Optic customers and lower tech tickets per customer to 0.5

- Increase sale of 1 and 2 year contracts by 5% each

- Yearly increase of automatic payments by 5%


---


# Shareable Link

You can interact with the report here: 

[Report](https://app.powerbi.com/view?r=eyJrIjoiZjMzMjk1ZDAtYzBjYy00OTZjLTk1YzQtMzI1MjE0NWFkOGYxIiwidCI6IjQ5ODY4YWYzLWNjNWYtNDIxNC04YjdmLTQwZjM3NDY0OWEwOSJ9)

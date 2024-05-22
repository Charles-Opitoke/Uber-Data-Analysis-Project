
# Uber Analysis

This is a brief description of what this project is about

# Uber Dataset

### Jupyter Notebook Link : http://localhost:8888/notebooks/Music/Uber%20Data%20Analysis%20Project.ipynb

## Problem Statement

Uber Technologies, Inc. is an American multinational transportation network company based in San Francisco and has operations in approximately 72 countries and 10,500 cities. In the fourth quarter of 2021, Uber had 118 million monthly active users worldwide and generated an average of 19 million trips per day.

Ridesharing is a very volatile market and demand fluctuates wildly with time, place, weather, local events, etc. The key to being successful in this business is to be able to detect patterns in these fluctuations and cater to the demand at any given time.

This analysis is based on the task given by the company of extracting insights from data that will help the business better understand the demand profile and take appropriate actions to drive better outcomes for the business. Our goal is to identify good insights that are potentially actionable, i.e., the business can do something with it.

## Key Questions
- What are the different variables that influence pickups?
- Which factor affects the pickups the most? What could be plausible reasons for that?
- What are your recommendations to Uber management to capitalize on fluctuating demand?


### Steps followed 

- Step 1 : import the necessary libraries

       import pandas as pd
       import numpy as np
       import matplotlib.pyplot as plt
       import seaborn as sns
       import warnings
       warnings.filterwarnings('ignore')
       import datetime as dt


- Step 2 : loading the data

       df = pd.read_csv("uber.csv")
       df.head()

![2](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/712ad622-59c4-4f85-96f8-0a37f3b616dc)

       df.tail()

![3](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/e3922962-c554-429d-a25f-44607aa3d555)

- Step 3 : checking the rows and columns

       df.shape

![4](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/451f0d3d-1410-4426-a6bd-b964dec497d1)

- Step 4 : checking data types, missing values

       df.info()

![5](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/965a04a6-bc8e-4ff0-91f1-972e2a4227a2)

- Step 5 : checking for missing data

       df.isnull().sum()

![6](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/df43c546-2708-46bf-9746-f0e808b828de)

- Step 6 : statistical summary of numerical

       df.describe()

![7](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/747c3ec9-f2fc-4a92-ac16-d064f3cc941a)

- Step 7 :  statistical summary of non-numerical

       df.describe(exclude="number")

![8](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/e2fd6c56-b56c-4c28-b282-70f5b406dda7)

- Step 8 : checking unique values

       df["hday"].unique()

![9](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/b9398d76-9d7a-434c-9d64-99cacf83f447)

       df["borough"].unique()

![10](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/d4adab4c-5c9f-418a-b0b4-1ec24e02aa99)

- Step 9 : number of unique values for each location

           df["borough"].value_counts()

![11](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/48ad53ab-1e03-460d-bbb6-1ff88367e618)

- Step 10 : trip during holiday vs non holiday

           df["hday"].value_counts()

![12](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/0601b192-8244-4433-8e7a-b9ecdacc1313)

- Step 11 : converting pickup date to datetime using pandas

           df["pickup_dt"] = pd.to_datetime(df["pickup_dt"])
           df["pickup_dt"]
           df.info()

![13](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/f183a27e-bc6a-42c5-8ff8-7a087d8b5233)

- Step 12 :  conversion

           df.pickup_dt = pd.to_datetime(df.pickup_dt)

#### extracting month, day, time
           df["day_name"] = df.pickup_dt.dt.day_name()
           df["month_name"] = df.pickup_dt.dt.month_name()
           df["start_hour"] = df.pickup_dt.dt.hour
           df["day"] = df.pickup_dt.dt.day
           df["year"] = df.pickup_dt.dt.year


- Step 13 : removing pickup_date column

           df = df.drop("pickup_dt", axis=1)
           df.head()

![14](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/6dfcac74-ef9f-40cb-b7cc-09bbeb6b2a19)

- Step 14 : handling missing data in borough

           df["borough"].value_counts(normalize=True,dropna=False)

![15](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/23916fe6-6b5a-486a-b68a-9a9657b5ce65)
      
 - Step 15 : change missing values to unknown

         df["borough"] = df["borough"].fillna("unknown")
         df["borough"].value_counts()
 
 ![16](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/069a531f-d08d-41c2-9ee4-d5e36cdd42c6)
 
 - Step 16 : check missing values again

          df.isnull().sum()

![17](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/20e3f588-b298-476d-93b7-a2a9c86bc487)

 
### plotting total of trip on holidy versus non holiday
### this is a univaraite analysis - using one parameter to plot
          sns.countplot(df, x="hday")

![18](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/754a2c2b-9369-43e1-b975-61b65cde1406)

### multivariate analysis
          num_var = ['pickups', 'spd', 'vsb', 'temp', 'dewp', 'slp', 'pcp01',
       'pcp06', 'pcp24', 'sd'] 
          corr = df[num_var].corr()
          corr

![19](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/77d9203a-ecf0-4f4a-9111-59e93cb22284)

          sns.heatmap(corr, annot=True, fmt = ".1f")

![21](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/5ef9525f-182a-4418-bb9b-1b05547ee83b)

### pickup across the month
          months = df.month_name.unique().tolist()

![22](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/569698f5-14e3-4d01-b95e-6f9a21c28f43)

### months = df.month_name.unique().tolist()
### month_trend = pd

          plt.figure(figsize=(20,7))
          sns.lineplot(df, x="month_name", y="pickups", color="red", ci=0)
          plt.ylabel('Total pickups')
          plt.xlabel('Month')
          plt.show()   

![23](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/9822aec6-22bd-4f58-a9da-b9035a8c8c08)

### pickup across the day

          plt.figure(figsize=(20,7))
          sns.lineplot(df, x="day_name", y="pickups", color="red", ci=0)
          plt.ylabel('Total pickups')
          plt.xlabel('Day of Month')
          plt.show()

![24](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/eb16470a-c2a7-423e-9644-5f17c76e72a5)

### pickup hour of the day across different borough

          plt.figure(figsize=(20,7))
          sns.lineplot(df, x="start_hour", y="pickups", color="red", ci=0, hue="borough")
          plt.ylabel('Total pickups')
          plt.xlabel('Hour of the Day')
          plt.show()

![25](https://github.com/Charles-Opitoke/Bank-Loan-Analysis-/assets/164562500/02d42da7-c4f8-4239-aa27-ed7bce910e39)

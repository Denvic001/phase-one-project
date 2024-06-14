# phase-one-project
Here lies my first project
Project Overview
For this project, you will use data cleaning, imputation, analysis, and visualization to generate insights for a business stakeholder.
Business Problem
The company is expanding in to new industries to diversify its portfolio. Specifically, they are interested in purchasing and operating airplanes for commercial and private enterprises, but do not know anything about the potential risks of aircraft. I am charged with determining which aircraft are the lowest risk for the company to start this new business endeavor. I must then translate my findings into actionable insights that the head of the new aviation division can use to help decide which aircraft to purchase.
Recommendations
I will offer recommendations that the head of aviation division can get insights from and help him make better and well informed decision from the data i will share with him in this document. I will look into the accidents of planes and get data from their fatality rates and their accident frequency which will in turn; help me recommend the plane make with the least amount of injury fatalities,thus makes it safer for our passengers in time of an accident and also help save the company money when it comes to renumeration of families that have lost their loved ones in plane crashes.
### Project Overview
For this project, you will use data cleaning, imputation, analysis, and visualization to generate insights for a business stakeholder.

### Business Problem
The company is expanding in to new industries to diversify its portfolio. Specifically, they are interested in purchasing and operating airplanes for commercial and private enterprises, but do not know anything about the potential risks of aircraft. I am charged with determining which aircraft are the lowest risk for the company to start this new business endeavor. I must then translate my findings into actionable insights that the head of the new aviation division can use to help decide which aircraft to purchase.
### Recommendations
I will offer recommendations that the head of aviation division can get insights from and help him make better and well informed decision from the data i will share with him in this document. 
I will look into the accidents of planes and get data from their fatality rates and their accident frequency which will in turn; help me recommend the plane make with the least amount of injury fatalities,thus makes it safer for our passengers in time of an accident and also help save the company money when it comes to renumeration of families that have lost their loved ones in plane crashes.


### The Data
In the data folder is a datasetLinks to an external site. from the National Transportation Safety Board that includes aviation accident data from 1962 to 2023 about civil aviation accidents and selected incidents in the United States and international waters.

It is up to you to decide what data to use, how to deal with missing values, how to aggregate the data, and how to visualize it in an interactive dashboard.


### Key Points
Your analysis should yield three concrete business recommendations. The key idea behind dealing with missing values, aggregating and visualizaing data is to help your organization make data driven decisions. You will relate your findings to business intelligence by making recommendations for how the business should move forward with the new aviation opportunity.

Communicating about your work well is extremely important. Your ability to provide value to an organization - or to land a job there - is directly reliant on your ability to communicate with them about what you have done and why it is valuable. Create a storyline your audience (the head of the aviation division) can follow by walking them through the steps of your process, highlighting the most important points and skipping over the rest.

Use plenty of visualizations. Visualizations are invaluable for exploring your data and making your findings accessible to a non-technical audience. Spotlight visuals in your presentation, but only ones that relate directly to your recommendations. Simple visuals are usually best (e.g. bar charts and line graphs), and don't forget to format them well (e.g. labels, titles).
# import the necessary libraries

import pandas as pd


import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline


### Loading data
#importing csv files

states_df = pd.read_csv('USState_codes.csv')
aviationdata_df = pd.read_csv('AviationData.csv',encoding= 'windows-1252',low_memory=False)


# Summary  of the dataframe,number of non null entries in each column

aviationdata_df.info()

# Checking the first five rows in the dataframe

aviationdata_df.head()


# Checking the five last rows of the dataframe 

aviationdata_df.tail()

# Checking for descriptive stats of the dataframe for the numerical columns 
aviationdata_df.describe()


### Names of columns 
aviationdata_df.columns

# Number of rows and columns in the dataframe
aviationdata_df.shape

## #Checking the total number of missing values in the columns

aviationdata_df.isnull().sum()

### Cleaning Data
# Dropping columns with alot of irrelevant null values

aviationdata_df.drop(columns=['Location', 'Country','Registration.Number','Latitude', 'Longitude', 'Airport.Name', 'Airport.Code', 'Aircraft.Category', 'FAR.Description','Schedule','Air.carrier',], inplace=True)
print(aviationdata_df.columns)

### Analyzing Data

#Normalizing columns by merging columns sharing the same name but differnt casing

aviationdata_df['Make'] = aviationdata_df['Make'].str.upper()

 #Filling and replacing null values
# Fill numerical columns with mean 
aviationdata_df['Total.Fatal.Injuries'].fillna(aviationdata_df['Total.Fatal.Injuries'].mean(), inplace=True)
aviationdata_df['Total.Serious.Injuries'].fillna(aviationdata_df['Total.Serious.Injuries'].mean(), inplace=True)
aviationdata_df['Total.Minor.Injuries'].fillna(aviationdata_df['Total.Minor.Injuries'].mean(), inplace=True)
aviationdata_df['Total.Uninjured'].fillna(aviationdata_df['Total.Uninjured'].mean(), inplace=True)
aviationdata_df['Number.of.Engines'].fillna(aviationdata_df['Number.of.Engines'].mean(), inplace=True)

# Fill categorical columns with mode
aviationdata_df['Make'].fillna(aviationdata_df['Make'].mode()[0], inplace=True)
aviationdata_df['Model'].fillna(aviationdata_df['Model'].mode()[0], inplace=True)
aviationdata_df['Investigation.Type'].fillna(aviationdata_df['Investigation.Type'].mode()[0], inplace=True)
aviationdata_df['Injury.Severity'].fillna(aviationdata_df['Injury.Severity'].mode()[0], inplace=True)
aviationdata_df['Aircraft.damage'].fillna(aviationdata_df['Aircraft.damage'].mode()[0], inplace=True)
aviationdata_df['Amateur.Built'].fillna(aviationdata_df['Amateur.Built'].mode()[0], inplace=True)
aviationdata_df['Engine.Type'].fillna(aviationdata_df['Engine.Type'].mode()[0], inplace=True)
aviationdata_df['Purpose.of.flight'].fillna(aviationdata_df['Purpose.of.flight'].mode()[0], inplace=True)
aviationdata_df['Weather.Condition'].fillna(aviationdata_df['Weather.Condition'].mode()[0], inplace=True)
aviationdata_df['Broad.phase.of.flight'].fillna(aviationdata_df['Broad.phase.of.flight'].mode()[0], inplace=True)
aviationdata_df['Report.Status'].fillna(aviationdata_df['Report.Status'].mode()[0], inplace=True)

### Drop columns with high missing values or not relevant

aviationdata_df.drop(columns=['Publication.Date'], inplace=True)


# Check remaining missing values
print(aviationdata_df.isna().sum())

### Plotting bar graph showing flight make and their accident frequencies

# Selecting the top 20 models with the most accidents
accident_frequency_by_make = aviationdata_df['Make'].value_counts().head(20)
top_10_models = accident_frequency_by_make.head(20)

# Plotting
plt.figure(figsize=(14, 7))
top_10_models.plot(kind='bar', color='black')
plt.title('Aircraft Makes with Most Accidents')
plt.xlabel('Aircraft Make')
plt.ylabel('Number of Accidents')
plt.xticks(rotation=45)
plt.show()

# Fatality Rates by Aircraft Model. Here i will take into comparison the two plane models with the highest number accidents and compare it with the model with the lowest number of accidents.


# Code for fatality rates for Cessna
cessna_data = aviationdata_df[aviationdata_df['Make'] == 'CESSNA']
accidents_by_model = cessna_data['Model'].value_counts().head(10)
fatalities_by_model = cessna_data.groupby('Model')['Total.Fatal.Injuries'].sum()
fatality_rate = (fatalities_by_model / accidents_by_model).dropna().sort_values(ascending=False)
plt.figure(figsize=(14, 7))
fatality_rate.plot(kind='bar', color='red')
plt.title('Total Fatal Injuries by Cessna Aircraft Model')
plt.xlabel('Cessna Model')
plt.ylabel('Fatality Rate')
plt.xticks(rotation=45)

plt.show()


### Code for Luscombe model fatality rates
luscombe_data = aviationdata_df[aviationdata_df['Make'] == 'LUSCOMBE']
accidents_by_model = luscombe_data['Model'].value_counts().head(10)
fatalities_by_model = luscombe_data.groupby('Model')['Total.Fatal.Injuries'].sum()
fatality_rate = (fatalities_by_model / accidents_by_model).dropna().sort_values(ascending=False)
plt.figure(figsize=(12, 6))
fatality_rate.plot(kind='bar', color='indigo')
plt.title('Fatality Rates by Luscombe Aircraft Model')
plt.xlabel('Luscombe Model')
plt.ylabel('Fatality Rate')
plt.xticks(rotation=45)

plt.show()


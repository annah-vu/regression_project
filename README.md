# Estimating Home Values from Zillow
![](https://github.com/annah-vu/regression_project/blob/main/assets/1ac.jpg)
### Table of Contents
- Project Overview
- Project Description
- Project Goals
- Project Deliverables
<br>
<br>

 - Project Summary
 - Hypothesis
 - Findings and Next Steps
<br>
<br>

 - Planning
 - Data Acquisition
 - Data Preparation
 - Data Exploration
 - Modeling and Evaluation
 - Delivery
 <br>
 <br>

 - Data Dictionary
 - How to Recreate


 ## Project Overview
 In this project, I will be working with a Zillow dataset to create a model that will predict a property's value. Specifically for this scenario...
 This machine learning model will predict single-unit property values that are sold during the peak season of real-estate (May-Aug) of 2017. 
 <br>
 <br>

 ## Goals
  - Deliver a Jupyter notebook going through the steps of the data science pipeline
  - Create a regression model that performs better than the baseline
  - Present to audience about my findings
 <br>
 <br>

## Deliverables
 - Finalized Jupyter notebook complete with comments
 - A README.md with executive summary, contents, data dictionary, conclusion and next steps, and how to recreate this project.
 - Presentation slide deck
 <br>
 <br>

## Project Summary
   TBA

## Hypothesis
    TBA

## Findings and Next Steps
 TBA

# The Pipeline: 

## Planning :stopwatch:
Goal: Plan out the project
I will be seeing how square footage, bathroom count, and bedroom count relate to property value. I believe there will be a 
positive correlation among these variables. 

I also want to look into other features, like (TBA) and see if that will also correlate to property value. 
A lot of these features could play hand in hand and help my model make better predictions.

Hypotheses:
TBA

<br>

## Acquire :bulb:
Goal: Have Zillow dataframe ready to prepare in acquire.py
In this stage, I used a connection URL to access the CodeUp database. Using a SQL query, I brought in the Zillow dataset with only properties set for single use, and were sold in between May-August 2017. I turned it into a pandas dataframe and created a .csv in order to use it for the rest of the pipeline. 
<br>

## Prep :soap:
Goal: Have Zillow dataset that is split into train, validate, test, and ready to be analyzed. Assure data types are appropriate and that missing values/duplicates/outliers are addressed. Put this in a prep.py. 
In this stage, I handled outliers by dropping any rows with values that were 3 standard deviations above or below the mean.
I assured that all columns had a numeric data type, and renamed them for ease of use.
Duplicates were dropped (in parcelid)
Nulls were also dropped, due to the strong correlation between square feet in respect to property value. I did not want to risk making the model dependent on assumed values. 
I split the data into train, validate, test, X_train, y_train, X_validate, y_validate, X_test, and y_test.
Last, I scaled it on a min-max scaler (I made sure to drop outliers first!) and also returned X_train, X_validate, and X_test scaled. 
<br>

## Explore :mag:
Goal: Visualize the data. Explore relationships.  Find answers. Use the visuals and statistics tests to help answer your questions. 
I plotted distributions, made sure nothing was out of the ordinary after cleaning the dataset. 

Plotted a pairplot to see combinations of variables.

I ran a few t-tests with the features in respect to tax_value. Also a few to see if the independent variables were related to each other. 

I found that square footage, bedroom count, and bathroom count were all statistically significant. They are not independent to property value. Bedroom count and bathroom count were also dependent on each other. 
<br>

## Modeling :chart_with_upwards_trend:
Goal: develop a regression model that performs better than the baseline.




## Data Dictionary 

| Column Name                  | Renamed   | Info                                            |
|------------------------------|-----------|-------------------------------------------------|
| parcelid                     | N/A       | ID of the property (unique)                     |
| bathroomcnt                  | baths     | number of bathrooms                             |
| bedroomcnt                   | beds      | number of bedrooms                              |
| calculatedfinishedsquarefeet | sqft      | number of square feet                           |
| fips                         | N/A       | FIPS code (for county)                          |
| propertylandusetypeid        | N/A       | Type of property                                |
| yearbuilt                    | N/A       | The year the property was built                 |
| taxvaluedollarcnt            | tax_value | Property's tax value in dollars                 |
| transactiondate              | N/A       | Day the property was purchased                  |
| age                          | N/A       | 2017-yearbuilt (to see the age of the property) |
    
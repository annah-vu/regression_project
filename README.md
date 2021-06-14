# Estimating Home Values from Zillow

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
   I built a regression model to predict single unit property values in 3 California counties using a Zillow data frame. 

## Hypothesis

1.) The larger the square footage, the higher the property value

2.) The more bedrooms a house has, the higher its property value will be

3.) The more bathrooms a house has, the higher its property value will be

4.) The older a house is, the less it will be worth.

5.) Value is dependent on property location

## Findings and Next Steps
   - Square footage was the best feature for predicting home value, followed up by bathrooms and bedrooms.
   - Age may have had a little factor, so it was accepted to use to better our model, but there may be other features we could look into next time.
   - Location still may have a factor in value, but we would need data that is more normally distributed. Most of the properties were in Los Angeles County. 

Next steps would be:
 - gather more information on location
 - try to clean up/fill in missing values for other location-based columns such as ZIP code, longitude/latitude
 - clean up other columns on home features and see if our model would perform with them (lower RMSE, higher r^2) 

<br>
<br>

# The Pipeline: 

## Planning :stopwatch:
Goal: Plan out the project
I will be seeing how square footage, bathroom count, and bedroom count relate to property value. I believe there will be a 
positive correlation among these variables. 

I also want to look into other features, like age and FIPS code, and see if that will also correlate to property value. 
A lot of these features could play hand in hand and help my model make better predictions.

Hypotheses: Square footage, number of bedrooms, number of bathrooms have a positive relationship with value. Age has a negative relationship with value. FIPS codes have an affect on value, perhaps the means are different across each county. 


<br>

## Acquire :bulb:
Goal: Have Zillow dataframe ready to prepare in acquire.py
In this stage, I used a connection URL to access the CodeUp database. Using a SQL query, I brought in the Zillow dataset with only properties set for single use, and were sold in between May-August 2017. I turned it into a pandas dataframe and created a .csv in order to use it for the rest of the pipeline. 
| acquire.py Functions | Purpose                                                        |
|----------------------|----------------------------------------------------------------|
| get_connection()     | Creates a connection link so we can access our data            |
| new_zillow_data()    | Uses a SQL query to return Zillow into a data frame            |
| get_zillow_data()    | Returns Zillow as a data frame, and creates a local Zillow.csv |
<br>


For the next stage: Drop or fill in nulls, remove outliers, rename columns, make new relevant columns.

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
| prep.py                                                  | Purpose                                                           |
|----------------------------------------------------------|-------------------------------------------------------------------|
| remove_outlier(df)                                       | removes outliers for certain columns                              |
| clean_zillow(df)                                         | obtain certain columns, makes it ready for use                    |
| train_validate_test(df, target)                          | X sets and y sets                                                 |
| get_object_cols(df)                                      | returns columns with object data types                            |
| get_numeric_X_cols(df, object_cols)                      | returns columns with numeric data types                           |
| min_max_scale(X_train, X_validate, X_test, numeric_cols) | uses MinMax scaler on X sets                                      |
| clean_zillow_taxes(df) *for tax_rates                    | cleans Zillow data for use in calculating tax_rates of properties |
| remove_outlier_tax(df) *for tax rates                    | removes outliers for use in calculating tax_rates of properties   |
<br>

For the next step: run statistical testing and visualize data to find relationships between variables.
<br>


## Explore :mag:
Goal: Visualize the data. Explore relationships.  Find answers. Use the visuals and statistics tests to help answer your questions. 
I plotted distributions, made sure nothing was out of the ordinary after cleaning the dataset. 

Plotted a pairplot to see combinations of variables.

I ran a few t-tests with the features in respect to tax_value. Also a few to see if the independent variables were related to each other. 

I found that square footage, bedroom count, and bathroom count were all statistically significant. They are not independent to property value. Bedroom count and bathroom count were also dependent on each other. 
<br>

| explore.py Functions                               | Purpose                                                                  |
|----------------------------------------------------|--------------------------------------------------------------------------|
| plot_variable_pairs(train, cols, hue=None)         | displays pairplot with regression line                                   |
| plot_pairplot(train, cols, hue=None)               | displays pairplot with scatter plots and histograms                      |
| correlation_exploration(train, x_string, y_string) | shows visual correlation between two columns                             |
| get_zillow_heatmap(train)                          | returns a heat map and r values of how each feature relates to tax_value |

<br>
For the next step: Select features to use to build a regression model that predicts property value
<br>

## Modeling and Evaluation :chart_with_upwards_trend:
Goal: develop a regression model that performs better than the baseline.

The models worked best with sqft, baths, beds, and age. Polynomial Regression performed the best, so I did a test on it.

| Model                            | RMSE Training | RMSE Validate | R^2   |
|----------------------------------|---------------|---------------|-------|
| Baseline                         | 357,185.61    | 359,454.06    | -3.07 |
| LinearRegression                 | 280,731.20    | 279,672.68    | 0.395 |
| LassoLars                        | 280,731.60    | 279,675.52    | 0.395 |
| TweedieRegressor                 | 280,731.20    | 279,672.68    | 0.395 |
| PolynomialRegression (3 degrees) | 276,310.65    | 274,076.31    | 0.42  |
<br>

Test:
 - RMSE of 272,168.27
 - R^2 of 0.403

Beats the baseline! 
<br>

## Delivery
I will be giving a presentation over my findings!



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
| taxamount                    | tax_amount| amount of tax on property                       |
| tax_rate                     | N/A       | tax_rate on property                            |


<br>
<br>

## How to Recreate Project

 - You'll need your own username/pass/host credentials. 
 - Have a copy of my acquire, prep, explore .py files. You can adjust the features to use, how to handle outliers, etc. 
 - My final notebook has all of the steps outlined, and it is really easy to adjust parameters. 

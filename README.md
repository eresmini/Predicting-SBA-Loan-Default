# Predicting-SBA-Loan-Default

## Summary
This project utilizes random forest and gradient boosting to predict if a Small Business Administration loan will default.

## Research Question
*Using historical data, can we predict if a Small Business Administration (SBA) loan will default?*

The SBA is an independent agency of the U.S. government that provides support to entrepreneurs and small businesses. They provide government-backed guarantee on a proportion of a loan made through partnering banks, credit unions, and other lenders. When an SBA loan defaults, the portion *not* covered by SBA falls back on the bank. The rate of default has fluxuated a lot over time, but a more recent statistic is 1 in 6 loans defaulting (https://www.nerdwallet.com/article/small-business/study-1-in-6-sba-small-business-administration-loans-fail).

## Data
Source: https://www.kaggle.com/datasets/mirbektoktogaraev/should-this-loan-be-approved-or-denied  
While the dataset is from Kaggle, this does appear to be real SBA data. The agency releases data, generally on a yearly basis, so this particular dataset is likely a compilation of pubicly available data. The data range from 1973-2011.  

For this project, the target variable is the loan outcome, either *default* or *paid in full.*

#### Data Consideration
While this dataset is quite comprehensive, we don't know what type of loans any of the entries are. SBA has several types of loans for different business needs and goals.Additionally, we don’t know the interest rate on any of these loans.

## Data Cleaning
Data cleaning consisted of:
- Recoding and reformatting values
  - some variables had confusing classification, so I changed any binary-type variables to 0/1 coding for simplicity
  - currency and date/time variables had to be reformatted for easier use
  - removing incorrect and special characters
- Removed rows which did not have a loan outcome (target) value
- Removed rows with a loan disbursement date after 2010
  - this was done to avoid skewing the data, as loans tend to close before they mature more due to defaulting than to paying in full early
- Filtered out businesses with more than 1500 employees
  - in general, a business with more than 1500 employees is no longer a small business, in the United States
  - removed these rows, assuming an error
- Added new predictor variables
  - most importantly: the proportion of the loan backed by SBA, and the number of days between loan approval and loan disbursement

## Not All Small Businesses Are Alike
Looking at the numerical data, we see a wide distribution of values and a lot of variation. Small businesses can widely differ from each other, for example the number of employees (0-1500). Likewise with the loans themselves, some SBA loans are only for a few thousand dollars while others are for a few million. This would also greatly affect the length of the loan term.
![image](https://user-images.githubusercontent.com/70169642/226688829-6101faaf-223a-46e8-873c-3781ee1ab1a5.png)

## EDA
### Where's the Risk?
![image](https://user-images.githubusercontent.com/70169642/226689915-ba80eb87-7fb9-44b9-bf10-c37314a01847.png)






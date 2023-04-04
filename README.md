# Predicting-SBA-Loan-Default

### Presented on March 10, 2023

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
![image](https://user-images.githubusercontent.com/70169642/229910054-f832b41f-f48d-41b1-8485-0d54815f754a.png)

## Modeling
### Considerations
As stated in the previous section, this dataset contains a lot of variation in numerical data, and many outliers. Additionally, we see a significant class imbalance within our target variable (see plot below). Especially when class imbalance is an issue, it is common for a model to simply predict the 'major' class for all cases, and while technically getting a high accuracy in testing, the model would return very poor precision and recall scores. Ultimately, I chose to use ensemble methods for this project; ensemble methods combine base models in order to produce one optimal predictive model, and thus tend to handle issues such as class imbalance and outliers better than other types of classification models.

### Random Forest
The random forest performed well, giving high accuracy but also promising precision and recall scores.
![image](https://user-images.githubusercontent.com/70169642/229910191-8be8f491-964d-4654-bf77-dcad34dad34d.png)

Metrics: 'accuracy': 0.9272883262473547, 'precision': 0.8418504043457955, 'recall': 0.7305050763586725

The precision score shows that out of all predictions of defaulted loans, about 84% were labelled correctly. The recall was not as reliable, with only 73% of all default loans being predicted accurately.

Looking at feature importance (plotted below), the random forest model gave the most importance to the term length of the loan, followed by the number of days between approval and disbursement. This weighting is logical: short-term loans are considered safer, while long-term loans are considered riskier and tend to have higher interest rates to compensate for the risk. For days until disbursement, the difference between waiting a couple weeks versus a couple months can make a critical difference in a business's financial stability and overall success.
![image](https://user-images.githubusercontent.com/70169642/229910397-96bed7de-50cf-4b4f-a9db-faf4f0dd3b3d.png)


### Gradient Boosting
The gradient boosting model performed very similarly to the random forest model. Again, we see high overall accuracy and moderate precision and recall scores: 'accuracy': 0.9189948714037864, 'precision': 0.8185884691848907, 'recall': 0.7025851036600973

The precision score for gradient boosting is marginally lower than random forest, with 81% of predictions of defaulted loans being accurate, and the precision score is also lower than random forest at 70%.
  
![image](https://user-images.githubusercontent.com/70169642/229910585-41a1c3aa-ed93-470f-845c-ca8ae89deca9.png)
  
Looking at feature importance, the gradient boosting put the most weight overwhelmingly on the term length of the loan. While, as stated previously, this feature can play an important roll in assessing the risk of default on a loan, I do prefer random forest once more for putting more weight of importance on more features.
![image](https://user-images.githubusercontent.com/70169642/229910494-3173b6ad-219e-4d60-8fb4-a5a757ee4b7b.png)

### Conclusion
While both models returned similar results, random forest did perform the best out of the two, and also gave more consideration to more features when making predictions.




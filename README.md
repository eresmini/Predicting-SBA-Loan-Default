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
![image](https://user-images.githubusercontent.com/70169642/226688829-6101faaf-223a-46e8-873c-3781ee1ab1a5.png)

## Modeling
### Considerations
As stated in the previous section, this dataset contains a lot of variation in numerical data, and many outliers. Additionally, we see a significant class imbalance within our target variable (see plot below). Especially when class imbalance is an issue, it is common for a model to simply predict the 'major' class for all cases, and while technically getting a high accuracy in testing, the model would return very poor precision and recall scores. Ultimately, I chose to use ensemble methods for this project; ensemble methods combine base models in order to produce one optimal predictive model, and thus tend to handle issues such as class imbalance and outliers better than other types of classification models.

### Random Forest
The random forest performed well, giving high accuracy but also promising precision and recall scores.
![image](https://user-images.githubusercontent.com/70169642/226979672-8a0aea74-e938-40e7-aa15-c98203b566ea.png)

Metrics: 'accuracy': 0.9239352273610022, 'precision': 0.8346023688663282, 'recall': 0.710024391219161

The precision score shows that out of all predictions of defaulted loans, about 83% were labelled correctly. The recall was not as reliable, with only 71% of all default loans being predicted accurately.

Looking at feature importance (plotted below), the random forest model gave the most importance to the term length of the loan, followed by the number of days between approval and disbursement. This weighting is logical: short-term loans are considered safer, while long-term loans are considered riskier and tend to have higher interest rates to compensate for the risk. For days until disbursement, the difference between waiting a couple weeks versus a couple months can make a critical difference in a business's financial stability and overall success.
![image](https://user-images.githubusercontent.com/70169642/226980208-6546edd3-50b5-4c5d-9c7d-2995a92f5d2a.png)


### Gradient Boosting
The gradient boosting model performed very similarly to the random forest model. Again, we see high overall accuracy and moderate precision and recall scores: 'accuracy': 0.9193166811437611, 'precision': 0.8094587206123565, 'recall': 0.7103842616658003

The precision score for gradient boosting is marginally lower than random forest, with 81% of predictions of defaulted loans being accurate, and the precision score is the same as random forest at 71%.

![image](https://user-images.githubusercontent.com/70169642/226981144-50d7bb6b-d798-40a9-9ed6-05ba6056f75b.png)

Looking at feature importance, the gradient boosting put the most weight overwhelmingly on the term length of the loan. While, as stated previously, this feature can play an important roll in assessing the risk of default on a loan, I do prefer random forest once more for putting more weight of importance on more features.
![image](https://user-images.githubusercontent.com/70169642/226982811-f0fc4865-b7f0-4360-9536-e00fc417ee2f.png)

### Conclusion
While both models returned similar results, random forest did perform the best out of the two, and also gave more consideration to more features when making predictions.




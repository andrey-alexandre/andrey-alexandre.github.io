---
layout: post
title:  "Churn Analysis"
date:   2022-04-09
excerpt: "Churn analysis project using survival analysis methods to discover WHEN will the costumer leave the company."
tag:
- "Survival Analysis"
- "Churn"
comments: true
project: true
---
# Churn Analysis
## What is Churn analysis?
Churn analysis is the evaluation of a company’s customer loss rate. This analysis is focused on evaluating how many costumers are being lost and bringing to light the reasons why these costumers are leaving the company.

With this metrics, we are able to reajust company policies and products to better satisfy the client, rising clients fidelity and profits as well.

## What is costumer churn?
Customer churn, also known as customer attrition, is when a customer essentially stops being a customer - ie, they choose to stop using your products or services. The costumer churn rate is the percentage of customers that stopped using your company's product or service during a certain time frame. Every company experiences churn - the key is to understand why your customers are churning and decrease the churn rate.

## How to model churn?
The most common way to analyse churn is using classification models. Your costumers either are a churn or are still with you, so it's easy to configure it as a binary classification problem. Given the nature of the problem, it's best to use an interpretable model, like logistic regression or a tree-based model, or a post hoc explanation tool like LIME or SHAP if the model isn't interpretable.
Although this is the main modelling pratice seen on most article, there are limitations to this approach, you can only predict either the client wil leave the company or not. For example, you can rank the clients by it's likelihood to leave the company, but you cannot differ which costumer will be the first to leave the company. If you need this time to churn, you would need to create a secondary model to predict it.

And creating this secondary model, you are only able to analyze the observations that left the company, given that if they didn't leave you still dont know how long it'll take for them to churn. So there will be , hopefully, many costumers that will be excluded from this analysis.

To overcome this problem we could use survival models to comprehend when the costumer will leave the company. In this approach we assume that every costumer will leave the company, it's just a matter of time for it to happen. While it doesn't happen, we see the costumer as a censored observation, since the event (leave the company) didn't happen until the moment of analysis.

With this model we are able to utilize both censored and uncensored observations to predict the time to churn, thus getting a more accurate prediction, as well a better response capability for the company.

# Churn Analysis on Telco
## Introduction
The data that we are going to use is the 'Telco Costumer Churn', that lists the costumers from Telco, a telecommunication company. For that we have a csv file that contains 7032 rows and 21 columns, one of them being the Churn column, that identifies the costumers that left the company within the last month.

Aside from the Churn column, we have a list of services that the costumer signed up for (phone, multiple lines, internet, etc.), account informations ( tenure, contract, payment method, paperless billing, monthly charges, and total charges) and demographic informations (gender, age range, and if they have partners and dependents).

All the data is available at Kaggle [in this link](https://www.kaggle.com/datasets/blastchar/telco-customer-churn?datasetId=13996), as well as more information on metadata. And all the codes used in this project are available [in this github repository](https://github.com/andrey-alexandre/Churn-Analysis).

## Exploratory Data analysis
For the data analysis, all costumers that contracted Telco for less than a month were removed from the analysis, given their likely bad contribution to the analysis. After this, we look at the tenure behaviour.

As we can see from the table below there is a little right skewness on the data, since the median is lower than the mean, and, although it isn't a proof, that indicates non-normality on the data, a characteristic that we already expected, since we are going to try and model it with survival models. The oldest client of the company is there for 6 years at total, while the average would be around 2 years and a half.

| Min. | 1st Qu. | Median | Mean  | 3rd Qu. | Max.  |
|------|---------|--------|-------|---------|-------|
| 1.00 | 9.00    | 29.00  | 32.42 | 55.00   | 72.00 |


When we look at the churn variable, we see that 27% of the costumers left Telco last month, so in the case of a classification model, it would be indicated to treat the unbalanced data.

| 0    | 1    |
|------|------|
| 5163 (73.42%) | 1869 (26.58%) |

We can also look at how the other variables interact with churn. Firstly, we look at the clients payment method. We can see that most of the clients choose the automatic and mailed check have similar churn rates, but when eletronic checks is chosen, the churn rate rises to almost half the clients.

![PaymentMethod](/assets/img/2022-04-09-ChurnAnalysis-project/PaymentMethodChurn.png)

When we look if the client has a partner, we can see that clients that don't have a partner tend to leave the company more.

![Partner](/assets/img/2022-04-09-ChurnAnalysis-project/PartnerChurn.png)

The monthly charges also appear to have impact on churn, given that the clients that left the company had, in median, a higher bill than the clients that stayed at the company.

![MonthlyCharges](/assets/img/2022-04-09-ChurnAnalysis-project/MonthlyChargesChurn.png)

We also plotted the behaviour of the survival rates for each of the independent variables to analyse if they had any impact on churn. In the following plot we can notice that when the client has partners or dependents, they have higher chances to stay in the company, as well as if they don't have any internet services and use Paper Billing. With these, we could interpret that older clients, that already have a family and are not that fond of technology are more faithful clients.

![SurvivalRate](/assets/img/2022-04-09-ChurnAnalysis-project/SurvivalRate.png)

We can see, as well, that clients that have longer contracts tend to stay longer with the company, even considering their contract length. And clients that have automatic payment tends to stay longer too.

# Modelling

Now we will start to model our data. For this purpose, as said earlier, we'll use survival models. Usually the only models shown for this approach are the Kaplan Meier model and the Cox Proportional Hazard model, but many other models are available, like tree-based models and deep learning models.

Here we assess four models: Bag Tree, Boost Tree, Logistic Survival Regression and Cox PH models. For feature selection, we used the statistical significance from the Cox PH model, which lead us to nine features: Partner, Dependents, PhoneService, InternetService, Contract, PaperlessBilling, PaymentMethod, MonthlyCharges, TotalCharges.

To evaluate the models we use a common metric for this field, the C-Index. With it we can better evaluate the concordance of the predictions.

The outputs were as follows, the Bag Tree model had a 0.904 C-Index, the Boost Tree had 0.918, the Logistic Regression had 0.926 and the PH Model had 0.922. Thus the model that had the best performance was the Survival Logistic Regression.

From the model coefficients we are able to reassure some of the findings in the EDA step, like not having a partner lower you time with the company, as well as not having any dependent, using paperless billing, and using payment methods other than automatic.

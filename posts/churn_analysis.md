---
layout: post
title:  "Churn Analysis"
date:   '2022-04-09'
project: true
tag:
comments: true
---
# What is Churn analysis?
Churn analysis is the evaluation of a company’s customer loss rate. This analysis is focused on evaluating how many costumers are being lost and bringing to light the reasons why these costumers are leaving the company.

With this metrics, we are able to reajust company policies and products to better satisfy the client, rising clients fidelity and profits as well.

# What is costumer churn?
Customer churn, also known as customer attrition, is when a customer essentially stops being a customer - ie, they choose to stop using your products or services. The customer churn rate is the percentage of customers that stopped using your company's product or service during a certain time frame. Every company experiences churn - the key is to understand why your customers are churning and decrease the churn rate.

# How to model churn?
The most common way to analyse churn is using classification models. Your costumers either are a churn or are still with you, so it's easy to configure it as a binary classification problem. Given the nature of the problem, it's best to use an interpretable model, like logistic regression or a tree-based model, or a post hoc explanation tool like LIME or SHAP if the model isn't interpretable.

Although this is the main modelling pratice seen on most article, there is a limitation to this approach, you can only predict either the client will leave the company or not. For example, you can rank the clients by it's likelihood to leave the company, but you cannot differ which costumer will be the first to leave the company. If you need this time to churn, you would need to create a secondary model to predict it.

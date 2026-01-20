# Designing_ML_System_CH
This repo contains points discussed in the book Designing Machine Learning Systems by Chip Huyen


## Chapter 1: Overview of ML Systems
1. This book discusses when to use Machine Learning, some use cases of ML for a individual consumer and big and small enterprises.
2. Some of the most famous industry usecases are - fraud detection, price optimization, demand forecasting, churn prediction etc.
3. This chapter discusses when it is appropriate to use Machine Learning

### Components of ML systems
#### Stakeholders
4. Stakeholders involced in bringing an ML system to production are - ML Engineers, Sales Team (Stakeholders), Product Team, ML platform team, Manager
5. This book talks about decoupling objectives i.e. when the stakeholders want to achieve two different business objectives e.g. - "Recommending the restaurants that users are most likely to click on” and “recommending the restaurants that will bring in the most money for the app” are two different objectives.
#### Computational Priorities
6. Latency is often a non-negotiable for a company and can make or break a decision to put an ML model into production. While complex ML techniques like ensembling can give your ML system a small performance improvement, they tend to make a system too complex to be useful in production, e.g., slower to make predictions or harder to interpret the results. For many tasks, a small improvement in performance can result in a huge boost in revenue or cost savings. For example, a 0.2% improvement in the click-through rate for a product recommender system can result in millions of dollars increase in revenue for an ecommerce site.
7. Academic and Research settings prioritize fast training whereas industry setting prioritize fast inference. 
8. Academic and Research settings prioritize high throughput whereas industry setting prioritize high latency.
9. Most modern distributed systems batch queries to process them together, in this case higher latency might also mean higher throughput. However, batching requires your system to wait for enough queries to arrive in a batch before processing them, increases latency.
10. It is importanto to use percentiles to measure latency, some teams use p90 as a measure of latency i.e. what is time within which 90% of requests are completed
#### Data
11. Labels, Sparsity, Imbalance, Incorrectness are few things to issues with real-world data.
12. Working with users data also requires privacy and regulatory concerns.
#### Fairness
#### Interpretability

## Chapter 2: Introduction to ML System Design


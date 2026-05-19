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
1. Business and ML Objective - Most companies will only care about ML metrics if it helps them improve business metrics
2. Netflix measures the performance of their recommender system using take-rate: the number of quality plays divided by the number of recommendations a user sees.
3. According to a 2020 survey by Algorithmia, among companies that are more sophisticated in their ML adoption (having had models in production for over five years), almost 75% can deploy a model in under 30 days. Among those just getting started with their ML pipeline, 60% take over 30 days to deploy a model.
4. Most ML systems should have these four characteristics: reliability, scalability, maintainability, and adaptability.
5. Scalability - Model can grow in complexity, traffic volume, or ML Model Count
6. Maintainability - Code should be documented. Code, data, and artifacts should be versioned. Models should be sufficiently reproducible
7. Adaptability - System should allow updates to data distribution and business requirements without interruption
8. Building an ML system is an iterative and never ending process. Step of building ML system are as follows -
  *  Step 1 - Project Scoping: Goals, Objectives, and Constraints, Resource Estimation and Allocation, Stakeholders involved etc.
  *  Step 2 - Data Engineering: Handling data from data sources and formats
  *  Step 3 - Extract Features and Develop Models
  *  Step 4 - Deployment: Make model accessible to users
  *  Step 5 - MOnitoring and Continual Learning: for performance decay and changing business requirements
  *  Step 6 - Business Analysis: Evaluate Model performance against business goals and analyze to generate business insights
9. Framing ML Problems - There are two aspects to framing an ML problem
 * Output of the Model - Regression vs Classification (Binary vs Multiclass vs Multilabel)
 * Objective Function
10. Multiclass problems are generally harded than Binary. If a problem has too many labels, we say that classification task has high cardinality.
11. For multilabel classification, we can approach the classification in two ways -
 * The first is to treat it as you would a multiclass classification i.e., [0,1,0,0] vs [0,1,0,1]
 * Second is to turn into set of binary classification problems
12. ML model need an objective function (loss function) to guide the learning process
13. Common loss functions are RMSE or MAE (mean absolute error) for regression, logistic loss (also log loss) for binary classification, and cross entropy for multiclass classification.
14. We learn about decoupling objectives through an example of ranking posts for user feed. In the example we have objective of maximizing user engagement (minimize engagement loss) and maximizing the quality of content (minimize content loss) which could be at odds with each other. <br>
   <i>loss = ɑ quality_loss + β engagement_loss</i>
15. When there are multiple objectives, it’s a good idea to decouple them i.e. train two different models and combine their output. This also makes them easier to develop and maintain


## Chapter 3: Data Engineering Fundamentals
1. In this chapter we discuss databases for two major types of processing: analytical and transactional. We also discuss how data is passed across processes and the different types of data which is passed: historical data in data storage engines, and streaming data in real-time transports.
2.  CSV (comma-separated values) is row-major, which means consecutive elements in a row are stored next to each other in memory. Parquet is column-major, which means consecutive elements in a column are stored next to each other.
3. Data Model describe how data is represented. There are two common data models - NoSQL Model and Relational Model.
4. **Relational Model** - Data is organized into relation, each relation is a set of tuple. You can shuffle the order of the rows or the order of the columns in a relation and it’s still the same relation. Data in relational table should also be normalized. One major downside of normalization is that your data is now spread across multiple relations. The data model behind SQL has deviated from the original relational model. For example, SQL tables can contain row duplicates, whereas true relations can’t contain duplicates. 
5. SQL is that it’s a declarative language, as opposed to Python, which is an imperative language. In the imperative paradigm, you specify the steps needed for an action and the computer executes these steps to return the outputs. In the declarative paradigm, you specify the outputs you want, and the computer figures out the steps needed to get you the queried outputs.
6. **Declarative ML system** - With a declarative ML system, users only need to declare the features’ schema and the task, and the system will figure out the best model to perform that task with the given features.
7. Two major types of nonrelational models are the document model and the graph model. The document model targets use cases where data comes in self-contained documents and relationships between one document and another are rare. The graph model goes in the opposite direction, targeting use cases where relationships between data items are common and important.
8. **Document Model:** A collection of documents could be considered analogous to a table in a relational database, and a document analogous to a row. Document databases just shift the responsibility of assuming structures from the application that writes the data to the application that reads the data.
9. **Graph Model:** A graph consists of nodes and edges, where the edges represent the relationships between the nodes. A database that uses graph structures to store its data is called a graph database.
10. THis chapter then discusses the key differences between Structured vs Unstructured data.
11. Data has been traditionally stored in two kinds of databases - OLTP (Online Transactional Processing), OLAP (Online Analytical Processing).
12. **ETL:**  ETL refers to the general purpose processing and aggregating of data into the shape and the format that you want.
13. Some cmompanies do ELT to avoid storing data in structured format to process the data as we need, however, as the data grows it become inefficient to search through the data.
14. Modes of Dataflow:
  * Data passing through databases
  * Data passing through services (REST APIs/RPC) - Request driven
  * Data passing through a real-time transport like Apache Kafka and Amazon Kinesis
15. Data passing through services -
  * To pass data from process B to process A, process A first sends a request to process B that specifies the data A needs, and B returns the requested data through the same network. Because processes communi‐ cate through requests, we say that this is request-driven.
  * Structuring an application as separate services gives you a microservice architecture.
  * Implementations of a REST architecture are said to be RESTful. Even though many people think of REST as HTTP, REST doesn’t exactly mean HTTP because HTTP is just an implementation of REST
16. Data Passing Through Real-Time Transport -
  * Request-driven architecture works well for systems that rely more on logic than on data. Event-driven architecture works better for systems that are data-heavy.
  * Request-driven data passing is synchronous: the target service has to listen to the request for the request to go through. We introduce a "broker" which can facilitate the transfer, technically a database can be a broker but processing in a DB is slow.
  * The two most common types of real-time transports are pubsub, which is short for publish-subscribe, and message queue.
  * In the pubsub model, any service can publish to different topics in a real-time transport, and any service that subscribes to a topic can read all the events in that topic. e.g. - Apache Kafka and Amazon Kinesis
  * In a message queue model, an event often has intended consumers (an event with intended consumers is called a message), and the message queue is responsible for getting the message to the right consumers. e.g. - Apache RocketMQ and RabbitMQ
17. Batch Processing Versus Stream Processing



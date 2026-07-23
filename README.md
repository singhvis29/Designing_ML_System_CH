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
  * When data is processed in batch jobs, we refer to it as batch processing. Distributed systems like MapReduce and Spark to process batch data efficiently.
  * Stream processing refers to doing computation on streaming data with real-time transports like Apache Kafka and Amazon Kinesis.
  * Streaming technologies like Apache Flink are proven to be highly scalable and fully distributed, which means they can do computation in parallel.
  * Batch features—features extracted through batch processing—are also known as static features. e.g. - driver rating
  * Streaming features— features extracted through stream processing—are also known as dynamic features. e.g. - how many drivers are available right now
  * Kafka stream processing is limited in its ability to deal with various data sources. To extract these features requires efficient stream processing engines. Stream processing is more difficult because the data amount is unbounded and the data comes in at variable rates and speeds.

## Chapter 4: Training Data

### Sampling:
1. two families of sampling: nonprobability sampling and random sampling.
2. Nonprobability Sampling: The samples selected by nonprobability criteria are not representative of the real- world data and therefore are riddled with selection biases. e.g.- Convenience sampling, Snowball sampling, Judgment sampling, Quota sampling
3. Simple Random Sampling: all samples in the population equal probabilities of being selected. advantage of this method is that it’s easy to implement. The drawback is that rare categories of data might not appear in your selection.
4. Stratified Sampling:  you can first divide your population into the groups that you care about and sample from each group separately. Each group is called a stratum.
5. Weighted Sampling: This method allows you to leverage domain expertise. For example, if you know that a certain subpopulation of data, such as more recent data, is more valuable to your model and want it to have a higher chance of being selected, you can give it a higher weight. Weighted sampling is used to select samples to train your model with, whereas sample weights are used to assign “weights” or “importance” to training samples. Samples with higher weights affect the loss function more.
6. Reservoir Sampling: Reservoir Data is used when we streaming data. Each incoming nth element has n/k probability of being in the reservoir, where k is the size of reservoir.
7. Importance Sampling: This kind of sampling allows us to sample from a distribution when we only have access to another distribution. Imagine you have to sample x from a distribution P(x), but P(x) is really expensive, slow, or infeasible to sample from. However, you have a distribution Q(x) that is a lot easier to sample from. So you sample x from Q(x) instead and weigh this sample by P(x)/Q(x) is called the proposal distribution or the importance distribution. E.g. - Reinforcement Learning

### Labeling:
8. Hand Labeling: The technique of manually labelling the data. It can be tough to acquire hand labels as they are expensive, they can pose a threat to data privacy, they are slow, and there is an issue of different levels of accuracy. This leads to the problem of label ambiguity or label multiplicity. 
9. Data Lineage: Indiscriminately using data from multiple sources, generated with different annota‐ tors, without examining their quality can cause your model to fail mysteriously. It’s good practice to keep track of the origin of each of your data samples as well as its labels, a technique known as data lineage.
10. Natural Labels: Tasks with natural labels are tasks where the model’s predictions can be automatically evaluated or partially evaluated by the system. 
11. Many tasks can be framed as recommendation tasks. For example, you can frame the task of predicting ads’ click-through rates as recommending the most relevant ads to users based on their activity histories and profiles. Natural labels that are inferred from user behaviors like clicks and ratings are also known as behavioral labels.
#### Handling Lack of Labels:
12. Weak Supervision: Leverages (often noisy) heuristics to generate labels. No ground truth required, but a small number of labels are recommended to guide the development of heuristics. Labeling Method can include: Keyword heuristic, Regular Expression, Database Lookup, and Output of Other Models.
13. Semi-Supervision: semi-supervision leverages structural assumptions to generate new labels based on a small set of initial labels. Unlike weak supervision, semi-supervision requires an initial set of labels. The similarity can only be discovered by more complex methods. For example, you might need to use a clustering method or a k-nearest neighbors algorithm to discover samples that belong to the same cluster or use the high probability predictions of the model to add to training data.
14. Transfer Learning: Transfer learning refers to the family of methods where a model developed for a task is reused as the starting point for a model on a second task. In many cases, you might need to fine-tune the base model. Transfer learning also lowers the entry barriers in ML/AI.
15. Active Learning: Active learning is a method for improving the efficiency of data labels. The hope here is that ML models can achieve greater accuracy with fewer training labels if they can choose which data samples to learn from. The most straightforward metric is uncertainty measurement—label the examples that your model is the least certain about.
 * query-by-committee is a popular heuristic based on disagreement among multiple candidate models. You need a committee of several candidate models, which are usually the same model trained with different sets of hyperparameters or the same model trained on different slices of data. Each model can make one vote for which samples to label next, and it might vote based on how uncertain it is about the prediction. You then label the samples that the committee disagrees on the most.
 * Labels can come from different data regimes -
    - model generates samples in the region of the input space that it’s most uncertain about
    - stationary distribution where you’ve already collected a lot of unlabeled data and your model chooses samples from this pool to label.
    - real-world distribution where you have a stream of data coming in, as in production, and your model chooses samples from this stream of data to label.
16. Class Imbalance: where there is a substantial difference in the number of samples in each class of the training data
17. It is important to choose the right metrics to handle class imbalance. If it's a classification problem it is recommended to use F1, precision, and recall, these are asymmetric metrics, which means that their values change depending on which class is considered the positive class.
18. **Resampling -**
 i. Includes Oversampling (minority class) and Under Sampling (majority class). SMOTE and Tomek Links are popular techniques for Oversampling and Undersampling respectively
 ii. Undersampling runs the risk of losing important data from removing data. Oversam‐ pling runs the risk of overfitting on training data, sophisticated sampling techniques have been developed to mitigate these risks - Two-Phase Learning and Dynamic Sampling
19. Algorithm-level methods - Altering the loss function so that if there are two instances, x1 and x2, and the loss resulting from making the wrong prediction on x1 is higher than x2, the model will prioritize making the correct prediction on x1 over making the correct prediction on x2. By giving the training instances we care about higher weight, we can make the model focus more on learning these instances.
   * Cost-sensitive Learning: using a cost matrix to specify Cij: the cost if class i is classified as class j. If i = j, it’s a correct classification, and the cost is usually 0. If not, it’s a misclassification. If classifying POSITIVE examples as NEGATIVE is twice as costly as the other way around, you can make C10 twice as high as C01.
   * Class-balanced loss: punish the model for making wrong predictions on minority classes. In its vanilla form, we can make the weight of each class inversely proportional to the number of samples in that class, so that the rarer classes have higher weights.
   * Focal Loss: djust the loss so that if a sample has a lower probability of being right, it’ll have a higher weight
20. Data Augmentation: Data augmentation is a family of techniques that are used to increase the amount of training data.
 * Simple Label-Preserving Transformations:  randomly modify an image while preserving its label. You can modify the image by cropping, flipping, rotating, inverting (horizontally or vertically), erasing part of the image, and more.
 * Perturbation: In the case of computer vision, this means that adding a small amount of noise to an image can cause a neural network to misclassify it. Using deceptive data to trick a neural network into making wrong predictions is called adversarial attacks. Adversarial augmentation is less common in NLP.
 * Data Synthesis: synthesize new data is to combine exist‐ ing examples with discrete labels to generate continuous labels. The label of x' is a combination of the labels of x1 and x2: γ×0+ 1−γ ×1. This method is called mixup.

     





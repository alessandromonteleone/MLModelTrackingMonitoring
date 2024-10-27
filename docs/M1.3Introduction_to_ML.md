# Introduction to Machine Learning and Model Deploying

## What is a machine learning model?

Machine Learning models are computer algorithms that use data to make estimates (reasoned guesses) or decisions. Machine Learning models differ from traditional algorithms in the way they are designed. When a traditional computer software needs to be improved, users modify it. In contrast, a Machine Learning algorithm uses data to improve a specific task.

![ML_vs_traditional](images/ML/ML_vs_traditional.jpg)

---

## Type of Machine Learning

Machine learning can be divided into three main categories, depending on how systems gather information and learn from it:

### 1. Supervised Learning
In supervised learning, the system receives a set of labeled data, that is, with input (problems) and output (solutions) already defined. The goal is for the model to learn the relationship between input and output so that it can apply it to new, unseen data. In short, the model generalizes a rule that connects input and output, and uses it to solve similar problems in the future.

![ML_S_Learning](images/ML/Supervised_learning.png)

### 2. Unsupervised Learning
In unsupervised learning, the system receives a set of unlabeled data, i.e., only inputs without any indication of the desired outputs. The model's task is to identify hidden patterns or logical structures in the data, without any external intervention. The goal is to discover correlations or clusters within the dataset without prior knowledge.

![ML_US_Learning](images/ML/Unsupervised_learning.png)

### 3. Reinforcement Learning.
In reinforcement learning, the system learns by interacting with a dynamic environment and improves its behavior through a reward and punishment mechanism. The model receives positive or negative feedback depending on the actions taken, gradually adapting to maximize rewards over time. This type of learning is particularly useful for scenarios where continuous action and adaptation are critical, such as in games or robotics.

### 4. Deep Learning.
Deep learning is a subcategory of machine learning, often associated with reinforcement learning. Unlike the latter, deep learning does not rely on trial and error. It uses deep neural networks to analyze large amounts of data and independently create complex models. These models apply knowledge gained from old datasets to new data, identifying features relevant to specific tasks, such as speech or visual recognition.

---

## Machine Learning Deployment Workflow

The process of developing an ML-based solution is based on four stages: Data management, Model learning, Model verification, and Model deployment.

### 1. Data Management

Data is a key element in achieving good performance in machine learning solutions; in fact, the overall effectiveness of the solution depends as much on the training and testing data as on the algorithm itself. Therefore, it is critical to emphasize this phase, which focuses on preparing the data needed to build a machine learning model. Typically, data preparation accounts for about 80 percent of the time spent on a machine learning project and involves several steps: Data collection, Data preprocessing, Data augmentation, and Data analysis.

- **Data Collection** is the foundational step in building any machine learning model. It involves identifying, gathering, and organizing data from various sources. One of the biggest challenges in this phase is data discovery: finding out what data exists and ensuring it is accessible and relevant to the task at hand. In many cases, especially in large organizations, data can be scattered across different systems, services, or departments, making it difficult to locate.

  Furthermore, data can come in different formats—structured, such as databases with clear schemas, or unstructured, like text documents, images, or logs. Structured data is easier to work with as it follows defined formats, but unstructured data, while more flexible, often requires additional preprocessing to be useful for machine learning models.

  Another key aspect of data collection is ensuring that the data sources are reliable and of high quality. The credibility of the source impacts the accuracy and trustworthiness of the model’s predictions. Inconsistent, incomplete, or outdated data can lead to biased or faulty models, which is why it's important to carefully evaluate the provenance of the data.

- **Data Preprocessing** is a crucial phase in the life cycle of a machine learning project. It includes a number of data cleaning and transformation activities, such as handling missing values, correcting errors in the data, simplifying the data structure, and converting raw data into formats more suitable for the model. At this stage, the dataset is prepared to ensure it is homogeneous, error-free, and ready for analysis or training.

  A particularly important aspect of preprocessing is normalization or scaling of the data, ensuring that features have equal weight in the model and preventing variables with wider ranges from disproportionately affecting results. Additionally, handling outliers or anomalies is necessary to ensure that anomalous data do not distort model outcomes.

  One of the key challenges in preprocessing is the integration of data from multiple heterogeneous sources, which may use different schemas and conventions, complicating the creation of a cohesive dataset. For example, databases may present the same data type with different encodings or inconsistent field names, necessitating reconciliation and standardization.

  Data quality is another frequent problem. Often, data collected from various sources contain errors or duplicates, requiring actions such as removing duplicates, imputing missing values, and harmonizing formats. In situations where data are dispersed across multiple silos or log files, it becomes critical to ensure that all relevant information is properly retrieved and organized.

- **Data Augmentation** focuses on increasing the quantity and quality of available data, particularly to address the lack of labeled data essential for supervised machine learning. Manual labeling becomes complex and expensive when dealing with large volumes of data, such as in network traffic analysis, where millions of packets must be labeled. In contexts like medical image analysis, the shortage of available experts to label large datasets can further limit this process.

- **Data Analysis** involves examining the data for potential biases or unexpected variations in their distribution. A crucial aspect of this phase is data profiling, which includes activities such as checking data quality, identifying missing values and inconsistencies, and validating hypotheses about the data. Effective visualization and profiling tools are essential for detecting problems in data structure, but they remain scarce. This scarcity makes it difficult for data scientists to address data quality challenges, which can undermine the overall success of a machine learning project.

- **Division of Data**: A crucial step in data preparation is the division of the dataset into three main parts: **training**, **validation**, and **test**. 

  - **Training Set**: This portion is used to train the model. It contains the data on which the model learns the relationships between the independent (input) and dependent (output) variables. The size of this set must be large enough for the model to learn effectively.

  - **Validation Set**: Used to optimize the model's hyper-parameters and perform tuning without interfering with training. Validation helps monitor the model's performance during training and prevent overfitting, allowing parameters to be adjusted and the overall accuracy of the model to be improved.

  - **Test Set**: Finally, the test set is a portion of the data never used during training or validation. It is used exclusively to evaluate the final performance of the model, providing an unbiased measure of its ability to generalize to unseen data.

  This subdivision is essential to ensure that the model can not only learn from the data, but also generalize correctly to new data, avoiding the risk of overfitting to the training data.

## 2. Model Learning

**Model Learning** is the phase of the deployment workflow in which machine learning (ML) models are developed and trained to solve specific problems. This process involves choosing the appropriate model, training the model, and optimizing hyper-parameters, each of which plays a crucial role in determining the effectiveness of the final model.

### Model Selection

Model **selection** is a critical step that depends on the specific goal of the application. There are several types of models that can be considered:

- **Regression models**: used to predict continuous values, such as house prices.
- **Classification models**: used to assign categories to discrete data, as in the case of classifying emails as spam or non-spam.
- **Clustering models**: used to group similar data without predefined labels, as in the case of user behavior analysis.
- **Dimensional reduction**: a technique that simplifies data representation while retaining the most meaningful information; useful for improving model performance and facilitating visualization.

![ML_type](images/ML/Models.png)


When choosing a model, it is important to consider complexity and interpretability. Simple models may offer advantages in computational resources and ease of interpretation, while more complex models may improve performance but require more resources.

### Training

Model **training** is the process by which the selected model is fed a **training dataset** to learn patterns and relationships. During this phase, the model optimizes its internal parameters to minimize the error in predictions based on the training data. However, training requires significant computational resources, and the quality of the training dataset is crucial; poor data can compromise the performance of the model. In addition, the training process must be mindful of privacy issues, especially when using sensitive data.


### Hyper-parameter Selection

The **selection of hyper-parameters** is another important step in model learning. Hyper-parameters are parameters that are not learned during training but must be set before the learning process begins, such as the depth of a decision tree or the number of neurons in a neural network.

To identify the best hyper-parameters, a **validation dataset** is often used. This dataset is separate from the training and test datasets and serves to evaluate the model's performance during the hyper-parameter tuning process without introducing bias. By using a validation set, practitioners can effectively assess how different hyper-parameter configurations influence model performance and avoid overfitting to the training data.

Techniques such as **grid search** are used to systematically explore different combinations of hyper-parameters, testing each configuration on the validation dataset. Other techniques, such as **random search** and **Bayesian optimization**, can also be employed to quickly find optimal settings, reducing the computational time required compared to grid search.

## 3. Model verification

Model verification is a crucial step in the software development cycle, particularly for machine learning (ML) models. It ensures the quality and reliability of the model, ensuring that it can generalize correctly to unseen data and satisfy all functional requirements.

The verification process has three main stages: requirements coding, formal verification, and test-based verification.

- In the **requirements coding** phase, it is essential to define clear, industry-specific metrics that reflect business objectives and user expectations. It is important to consider and choose the right metrics, as it is not enough to measure the accuracy of the model; it is also critical to consider other relevant metrics to ensure that the model produces useful and relevant results.

- **Formal verification** focuses on ensuring that the model meets established requirements. This may include setting quality standards that the model must achieve to be considered compliant with industry regulations and best practices.

- Finally, in the **test-based verification** phase, a test dataset is used to evaluate how the model performs with previously unseen data. This phase is crucial to ensure that the model not only performs well during training, but can also generalize effectively in real-world situations. It is important to note that validation of the data must occur continuously to identify and correct any errors that could affect the performance of the model over time.

![ML_lifecycle](images/ML/Lifecycle_ML.png)

### 4. Model Deployment

Model deployment is a crucial phase for machine learning systems in production, which are complex and require ongoing maintenance. This presents challenges that are both shared with traditional software services and unique to machine learning. It is essential to apply **DevOps** principles to these systems, despite the specific difficulties associated with productionizing ML models.

The deployment process is divided into three main steps: **integration**, **monitoring**, and **updating**.

1. **Integration**: This step involves building the necessary infrastructure to run the model and implementing it in a consumable form. Code and data reuse is essential for saving time and resources. It is important for researchers and engineers to collaborate throughout the development process, as their responsibilities often overlap.

2. **Monitoring**: Monitoring is critical for maintaining machine learning systems. The ML community is still in the early stages of understanding which key metrics to monitor. It is necessary to track evolving input data, prediction bias, and overall model performance. Managing feedback loops is another challenge, as models in production can influence their behavior over time.

3. **Updating**: Once the initial deployment is complete, it is often necessary to update the model to reflect the most recent trends in data. Techniques such as scheduled retraining and continual learning are useful but must address practical considerations. A critical issue is **concept drift**, which refers to changes in data distribution that can adversely affect model performance. Detecting concept drift is essential for maintaining model effectiveness over time. Additionally, implementing **continuous delivery** practices is complex for ML models due to changes affecting code, model, and data. It is crucial to ensure that model updates do not compromise user trust in the system.

Further exploration of **MLOps** will be covered in the subsequent module.




# Operationalizing Machine Learning

The Operationlalizing Machine Learning project is part of the Machine Learning Engineer with Microsoft Azure Nanodegree Program on Udacity.

In the project we demonstrate to build a Machine Learning model, deploy the model into production and consume the model via an API. 
This is been achieved in two different ways:
1. Manually within the the Azure Machine Learning Studio
2. In Python by using the SDK

## Architectural Diagram

The main steps in the project are illustrated in the following Diagram:

![Architecture](images/01-architecture.png)

* *Authentication*: Prepare infrastructure enabling connection to Azure Machine Learning Studio
* *AutoML Model*: Upload dataset, create compute cluster and configure an experiment with AutoML 
* *Deploy the best model*: Deploy the best model provided by AutoML to allow interaction via API service
* *Enable logging*: Enable Application Insights
* *Consume model endpoints*: Interact with the model via API
* *Create and publish a pipeline*: Automate the workflow (Create Model - Deploy Model - Consume Model) with the SDK
* *Documentation*: Document the model deployed

## Key Steps

### Authentication

The lab provided by Udacity does not allow to create a security principal. Therefore this step is not performed.

### AutoML Model

Upload and register the Bank Marketing dataset (Source file: [Bank Marketing](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)) 

![Dataset](images/02-dataset.png)

Setting up a compute cluster based on *Standard_DS12_v2* virtual machines and a minimum number of 1 node.

![Compute Cluster](images/03-compute-cluster.png)

Definition of the AutoML run. First, we need to select the previously uploaded Bank Marketing dataset.

![Configure AutoML](images/04-create-automl.png)

Next we need to specify a name for the experiment, define a target column in the dataset (Target column: Y) and select the compute cluster.

![Configure AutoML](images/05-create-automl2.png)

The machine learning task type for the experiment is a classification problem without enabling *Deep Learning*.

![Configure AutoML](images/07-create-automl3.png)

Additionally the *Exit Criterion* is reduced to 1 hour and the *maximum number of concurrent iterations* is limited to 5.

![Configure AutoML](images/06-create-automl4.png)

AutoML trains several models on the registered dataset and determines their metrics such as AUC or accuracy.

![AutoML run completed](images/08-automl-completed.png)

The best model (with regard to accuracy) was a **Voting Ensemble** model with an accuracy of **0.91866**.

AutoML provides comprehensive metrics on the model for further analysis.

![Best model metrics](images/10-best-model-metrics.png)


### Deploy the best model

Once the best model is identified, we want to use it to predict whether the client subscribed a term deposit based on various input parameters. Next step is therefore to deploy the model in Azure which allows interaction with the model via standardized HTTP APIs. We want to deploy the model on *Azure Container Instance (ACI)* with *Authentication* enabled.

![Deploy best model](images/11-best-model-deploy.png)


![Best model deployed](images/12-best-model-deployed.png)

The main outcome of the deployment process is a successful established endpoint. Apart from infrastructure details and a link to the endpoint, Azure also provides information on how to consume and interact with the service.

![Endpoint details](images/13-endpoint-best-model.png)


![Endpoint more details](images/14-endpoint-best-model2.png)


### Enable logging

Azure provides a tool called *Application Insights* to detect anomalies and visualize performance of applications. Since we did not enable this tool before the deployment, we do this subsequently via the Python SDK. (Source Code: [logs.py](https://github.com/HaslRepos/nd00333_AZMLND_C2/blob/master/logs.py)). The Script requires interactive login.

![Enable logging](images/15-enable-logging.png)

![Enable logging](images/16-enable-logging2.png)

Running the Python Script enables *Application Insights*.

![Application Insight enabled](images/17-endpoint-best-model-application-insight-enabled.png)


### Swagger Documentation

Azure provides documentation (swagger.json file) of the web services deployed in ML Studio, explaining the different request types (such as POST and GET). 
Swagger is an Interface Description Language for describing RESTful APIs expressed using JSON. (Wiki)

![Swagger](images/18-swagger.png)


![Swagger](images/19-swagger2.png)


### Consume model endpoints

Now we want to interact with our model to predict whether a client subscribed a term deposit. Again we utilize the Python SDK (Source Code: [endpoint.py](https://github.com/HaslRepos/nd00333_AZMLND_C2/blob/master/endpoint.py)). Azure ML Studio provides the necessary *URI* and *Key* to access the service.

The requests to the service get the following payload attached:

'''python
data = {"data":
        [
          {
            "age": 17,
            "campaign": 1,
            "cons.conf.idx": -46.2,
            "cons.price.idx": 92.893,
            "contact": "cellular",
            "day_of_week": "mon",
            "default": "no",
            "duration": 971,
            "education": "university.degree",
            "emp.var.rate": -1.8,
            "euribor3m": 1.299,
            "housing": "yes",
            "job": "blue-collar",
            "loan": "yes",
            "marital": "married",
            "month": "may",
            "nr.employed": 5099.1,
            "pdays": 999,
            "poutcome": "failure",
            "previous": 1
          },
          {
            "age": 87,
            "campaign": 1,
            "cons.conf.idx": -46.2,
            "cons.price.idx": 92.893,
            "contact": "cellular",
            "day_of_week": "mon",
            "default": "no",
            "duration": 471,
            "education": "university.degree",
            "emp.var.rate": -1.8,
            "euribor3m": 1.299,
            "housing": "yes",
            "job": "blue-collar",
            "loan": "yes",
            "marital": "married",
            "month": "may",
            "nr.employed": 5099.1,
            "pdays": 999,
            "poutcome": "failure",
            "previous": 1
          },
      ]
    }
'''

![Consume Endpoint](images/20-consume-endpoint.png)


### Create and publish a pipeline

Azure Pipelines offers Continuous Integration (CI) and (Continuous Delivery (CD) to consistently create, test and deploy code. We make use of that functionality to automate all of the manual tasks we performed so far.

We use a Jupyter Notebook (Source Code: ) to create and run the Pipeline.

![Create pipeline](images/21-create-pipeline.png)


During runtime we get a lot of logging and status information in the SDK.

![Pipeline](images/23-pipeline.png)


![Create pipeline](images/22-create-pipeline2.png)


Alternatively we can also access the console in Azure ML Studio to retrieve this information:

![Pipeline](images/23-pipeline3.png)


![Pipeline](images/24-pipeline2.png)



![Pipeline endpoint](images/25-pipeline-endpoint.png)



## Screen Recording

[Screencast](https://drive.google.com/file/d/1Q4mb2UiF-ariXywD1DgUTmd5qpm69wsS/view?usp=sharing)

https://drive.google.com/file/d/1Q4mb2UiF-ariXywD1DgUTmd5qpm69wsS/view?usp=sharing

## Standout Suggestions

In the project we mainly concentrated on operationalizing ML in Azure. We did not go into much details optimizing the Models. The dataset used to train the model for example is highly imbalanced which might have a negative effect on the trained model leading to biased predictions. To be evaluated in further experiments.

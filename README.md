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



![Configure AutoML](images/07-create-automl3.png)



![Configure AutoML](images/06-create-automl4.png)


### Deploy the best model


### Enable logging


### Consume model endpoints


### Create and publish a pipeline


### Documentation


## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.

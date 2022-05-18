# Challenge 1 – Import repo into Azure DevOps

[< Previous Challenge](./Challenge-00.md) - **[Home](../README.md)** - [Next Challenge >](./Challenge-02.md)

## Introduction

The objective of challenge is to forecast the daily transactions using time series [ARIMA](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average) modeling. It includes processing the transactions data from AdventureWorks database, analyzing it, creating and registering an ARIMA model, and finally deploying the model to an ACI instance. This entire lifecycle is done using [Azure ML Python SDK](https://docs.microsoft.com/en-us/python/api/overview/azure/ml/?view=azure-ml-py).

Time series is a series of data points collected or indexed in time order at regular time points. It is a sequence taken at successive equally spaced time points. For example, weather data collected every hour or stock data collected every day or sensor data collected every minute. As a result, we see a trend and seasonality in time-series datasets. It is this temporal nature that makes them different from other conventional datasets and warrants a different type of modeling. We will cover that in this challenge as forecasting is one of the most common and prevalent tasks in Machine Learning.

## Description

- Create and setup a new project in Azure DevOps
  - Copy yourself a of the [repository](https://github.com/karpova01/DemandForecasting).
  - [Create new service connections](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml) in Project Settings for your Azure ML service and Azure Subscription using Azure Resource Manager service principal. This will enable you to connect to external and remote services to execute tasks in a pipeline.
- Configure your Azure ML Workspace for the project.
  - **HINT:** Add workspace details in `config.json` in a file in the repository you copied. 
- Import the repository into an Azure DevOps project
  - **HINT:** Use the `repos` section on the DevOps portal

## Description of notebooks

- `WorkSpace.py` to setup connection with your Azure ML service workspace.
- `AcquireData.py` to get daily transactions data from AdventureWorks.
(Load the data, form a DataFrame from them with the following columns:'TransactionID', 'ProductID', 'ReferenceOrderID', 'ReferenceOrderLineID', 'TransactionDate', 'TransactionType', 'Quantity', 'ActualCost', 'ModifiedDate'.Create a dataset in Azure ML from the uploaded data.
- `TrainOnLocal.py` to train the model.An experiment is created in the Azure Machine Learning workspace. The model is trained using the transaction_arima.py file. This file adds data from the dataset and imports the ARIMA model from the statsmodels library.
- `EvaluateModel.py` to evaluate the model.Compare different versions of models. Comparison criterion: [R2](http://www.machinelearning.ru/wiki/index.php?title=%D0%9A%D0%BE%D1%8D%D1%84%D1%84%D0%B8%D1%86%D0%B8%D0%B5%D0%BD%D1%82_%D0%B4%D0%B5%D1%82%D0%B5%D1%80%D0%BC%D0%B8%D0%BD%D0%B0%D1%86%D0%B8%D0%B8)
- `RegisterModel.py` to register the model with Model registry.
- `CreateScoringImage.py` for scoring/forecasting using the trained model.
- `deployOnAci.py` to deploy the scoring image on ACI.
- `WebserviceTest.py` to the ACI deployment/endpoint. (Gets the hosted web server and writes the model there with all the functions.)

## Success Criteria

- Understand the contents of the python files under `service/code/`.
- Forecasting project imported into Azure DevOps.

## Learning resources

- [MLOps Home page to discover more](<https://azure.microsoft.com/en-us/services/machine-learning/mlops/>)
- [MLOps documentation: Model management, deployment, and monitoring with Azure Machine Learning](<https://docs.microsoft.com/en-us/azure/machine-learning/concept-model-management-and-deployment>)
- [A blog on MLOps - How to accelerate DevOps with ML Lifecycle Management](<https://azure.microsoft.com/en-us/blog/how-to-accelerate-devops-with-machine-learning-lifecycle-management/>)

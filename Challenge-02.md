# Challenge 2 – Create a Build Pipeline in Azure DevOps

[< Previous Challenge](./Challenge-01.md) - **[Home](../README.md)** - [Next Challenge >](./Challenge-03.md)

## Introduction

The Build pipeline demonstrates the automation of various stages/tasks involved in building an ML model and creating a container image on top of that model. The stages generally constitute creating a project environment, preparing the data, training the model, evaluating the model, registering/versioning the model, and scoring the model generally by containerizing the model.

There are several ways to create a `Build` pipeline. The two most common and popular ways are:
- Using a YAML file that represents the entire pipeline.
- Using the classic GUI pipeline and adding tasks sequentially.

We can setup Continuous Integration (CI) trigger for every `Build` pipeline. The CI pipeline gets triggered every time code is checked in. It publishes an updated Azure Machine Learning pipeline after building the code.

## Description

- Make sure you have setup a new project,imported the code, created service connections, and configured Azure ML Workspace for the project.
- Create a `Build` pipeline.
  - **TIP:** use the classic editor.
  - Select the repo that was imported in the previous challenge.
  - Start with an empty job.
  - Set Agent Pool to `Azure Pipelines`.
  - Set Agent Specification to `ubuntu-18.04`.
- Add `Build` pipeline tasks (add task to Agent job)
  - Add a task to install the required version of Python `Python 3.6`.
  - Add a task to setup environment by using `install_environment.sh` file in `environment_setup/` folder. This will install all the python modules required for the project.
  - **TIP:** Use Bash task to run a Bash script on macOS, Linux, or Windows.
  - Add a task to get Azure ML Workspace connection using `WorkSpace.py` in `service/code/` folder. This will establish connection to Azure ML workspace by using your workspace details in `configuration/config.json` file.
  - **TIP:** Use the Azure CLI task to run the Python scripts since they need to interact with the Azure Machine Learning resource (for this task and beyond, where you will need to run the Python script). Azure CLI task for Azure ML Workspace connection with  `python service/code/WorkSpace.py` in the Inline Script. Script type: ` Shell `         
  - Add a task to acquire time series transactions data using `AcquireData.py` in `service/code/` folder. This will download and extract the data required to train a forecasting model in the next steps.
  - Add a task to train ARIMA forecasting model using `TrainOnLocal.py` in `service/code/` folder. This will build a model to forecast demand of items from AdventureWorks database.
  - Add a task to evaluate the model performance using `EvaluateModel.py` in `service/code/` folder. This will evaluate how well the model is doing by using evaluation metrics such as R-squared and RMSE(Root mean squared error).
  - Add a task to register the model in Azure ML Model Registry for model versioning using `RegisterModel.py` in `service/code/` folder.    
  - Add a task to score the model, to forecast future transactions using `CreateScoringImage.py` in `service/code/` folder. This will create a scoring file.       
  - Now you are at a point of creating an artifact for your `Release` pipeline. An artifact is the deployable component of your model or application. `Build Artifact` is one of the many artifact types. The following two tasks are required to create `Build artifact` in your `Build` pipeline. 
    - Use Copy Files task to copy files from `$(Build.SourcesDirectory)` to `$(Build.ArtifactStagingDirectory)`.
    - Use Publish Artifact task with `$(Build.ArtifactStagingDirectory)` as path to publish. 
- Save and run the `Build` pipeline.
- Review `Build` Outputs - confirm that the model and Azure Container Image have been registered in the [Azure ML workspace](https://ml.azure.com/) in respective registries.

## Success criteria

- An end-to-end `Build` pipeline in Azure DevOps.
- Forecasting model registered with the Azure ML Model Registry.

## Tips

- Use the [predefined variables](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=classic) in Azure DevOps to make your tasks simpler & more robust.

## Learning resources

- [Key concepts for new Azure Pipelines users](<https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops>)
- [MLOps documentation: Model management, deployment, and monitoring with Azure Machine Learning](<https://docs.microsoft.com/en-us/azure/machine-learning/concept-model-management-and-deployment>)
- [MLOps Reference Architecture](<https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/ai/mlops-python>)

## Deploy and Consumed Bank Marketing ML Data using Microsoft Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree. In this project, we use Azure ML to configure a cloud-based machine learning production model, deploy it, and consume Bank Marketing Dataset. This dataset contains data about prospective customer such as age,job,marital,education, and whether they subscribe to a term deposit with the bank. we seek to predict based on prospective customer data which kind of customer characteristic that they will subscribe to a term deposit with the bankThe model then create, publish, and consume a pipeline.build 

## Architectural Diagram 
![](architecture%20diagram.png)

## Key Steps
There are 7 key steps:

1.Authentication
First we run az login command, and the login on Azure ML account, then type azure-cli-ml to make sure extension is added or already installed, and then run az ad sp create-for-rbac --sdk-auth --name {name} to create role based account, and then use client Id and run az ad sp show --id xxxxxxxx-3af0-4065-8e14-xxxxxxxxxxxx and finally use Object Id and run az ml worskspace share -w Demo -g demo --user xxxxxxxx-cbdb-4cfd-089f-xxxxxxxxxxxx --role owner  to make service principal assign us as a role owner

2.Automated ML Experiment
First we make sure that dataset is available in Experiment
![](step%202%20-%20dataset%20is%20available.png)
Create a new Automated ML run, upload the dataset and select it. Create and configure new compute cluster, and once the new compute cluster is successfully created, use this cluster to run the autoML experiment. Make sure you the name and target column are filled. Go to the Automated ML section and wait until the recent experiment with a completed status. 
![](step%202-%20experiment%20finished.png)
Click on it. And find the best model
![](step%202%20-%20best%20model.png)
Click on best model
![](step%202-%20best%20model%20(detail).png)
3.Deploy the best model
Above model tab, a triangle button (or Play button) will show with the "Deploy" word. Click on it. Deployment takes a few seconds. After a successful deployment, a green checkmark will appear on the "Run" tab and the "Deploy status" will show as succeed.

4.Enable logging
Download the config.json file from the top left menu in the Azure portal. Put this file in the same directory of other files. Find the previously deployed model to verify its name. It is needed in the SDK to select it for enabling logging. Run vim logs.py and change the name of deploy model and authentication to True. Run python logs.py.
![](step%204%20-%20log.py%20running.png)
Go back to ML Azure studio, and click authentication link and we can see the log
![](step%204%20-%20application%20insight.png)
5.Swagger Documentation
Ensure that Docker is installed on the computer. Azure provides a Swagger JSON file for deployed models. Head to the Endpoints section, and find deployed model there. Click on the name of the model, and details will open that contains a Swagger URI section. Download the file locally to the computer and put it in the same directory with serve.py and swagger.sh. Run serve.py on port 8000. This script needs to be right next to the downloaded swagger.json file. NOTE: this will not work if swagger.json is not on the same directory. Run cat swagger.sh which will download the latest Swagger container, and it will run it on port 80 and run bash swagger.sh. If you don't have permissions for port 80 on your computer, update the script to a higher number (in this project 9000). Open the browser and go to http://localhost:8000 where serve.py should list the contents of the directory. swagger.json must show. Go to http://localhost/ which should have Swagger running from the container (in this project 9000). On the top bar, where petsore.swagger.io shows, change it to http://localhost:8000/swagger.json, then hit the Explore button. It should now display the contents of the API for the model.
![](step%205%20-%20swagger%20run%20in%20local%20host.png)
Look around at the different HTTP requests that are supported for your model, including the example.
![](step%205%20-%20response%20for%20the%20model.png)
6.Consume model endpoints
n Azure ML Studio, head over to the "Endpoints" section and find a previously deployed model. In the "Consume" tab, of the endpoint, a "Basic consumption info" will show the endpoint URL and the authentication types. Take note of the URL and the "Primary Key" authentication type. Using the provided endpoint.py replace the scoring_uri and key to match the REST endpoint and primary key respectively. The script issues a POST request to the deployed model and gets a JSON response that gets printed to the terminal.
![](step%206%20-%20endpoint.png)
Change benchmark.sh to include the scoring_uri and key to match the REST endpoint and primary key respectively and run bash benchmark.sh
![](step%206%20-%20benchmark.png)
7.Create and publish a pipeline
Open up the Jupyter Notebook and upload aml-pipelines-with-automated-machine-learning-step.ipynb, make sure you replace all of the URIs, Keys, and experiment names to match your own. Run the Jupyter Notebook all the way up until pipeline section of Azure ML studio, showing that the pipeline has been created
![](step%207-%20pipeline%20has%20been%20created.png)
Run Bankmarketing dataset with the AutoML module
![](step%207%20bank%20marketing%20dataset%20with%20ml%20modul.png)
Run until “Published Pipeline overview”, showing a REST endpoint and a status of ACTIVE
![](step%207%20pipeline%20active%20and%20rest%20endpoint.png)
Configure a pipeline with the Python SDK until showing the “Use RunDetails Widget” with the step runs
![](step%207%20run%20detailed%20widget.png)
Use a REST endpoint to interact with a Pipeline. ML studio showing the pipeline endpoint as Active.
![](step%207%20ml%20studio%20pipeline%20is%20active.png)
ML studio showing the scheduled run
![](step%207%20ml%20pipeline%20is%20schedule.png)

## Screen Recording
The link to a screen recording of the project in action: https://youtu.be/ZEqKavpAtuo

## Standout Suggestions
It would be better the Bank Marketing Dataset is cleanse first, so the model would be more accurate. Running AutoML should also not to be reduce to 1 hour from the default of 3 hours.

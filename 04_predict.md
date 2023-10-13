# 04_predict

In this lab, we will explore and test the end\-to\-end machine learning project we created in [03_visualize Lab 1](03_visualize.md#lab-1-deploy-machine-leaning-applied-machine-learning-prototype-amp) using Cloudera Machine Learning (CML).

The primary goal of this project that we deployed is to build a gradient boosted \(XGBoost\) classification model to predict the likelihood of a flight being canceled based on years of historical records. To achieve that goal, this project demonstrates the end\-to\-end Machine Learning journey for model training and inference using Spark on CML. Additionally, this project deploys a hosted model and front\-end application to allow users to interact with the trained model.

## Pre-requisite

1. Please ensure that you have completed the [03_visualize Lab 1](03_visualize.md#lab-1-deploy-machine-leaning-applied-machine-learning-prototype-amp) to deploy the Applied Machine Learning Prototype (AMP) for `Canceled Flight Predicion`.


## Lab 1 - Explore Machine Learning Project

1. Open Cloudera Machine Learning (CML)

   * If you just completed the [03_visualize](03_visualize.md#03_visualize) phase, in the left nav click on Home.

   * If not, you can always go back to CDP Home Page by clicking the bento menu icon in the top left corner, click on Home, select the `Machine Learning` tile, and click on the Workspace available in your Machine Learning page (found under `Workspace`).  
![Screen_Shot_2023_04_24_at_11_33_56_PM.png](images/Screen_Shot_2023_04_24_at_11_33_56_PM.png)
![Screen_Shot_2023_04_24_at_11_42_33_PM.png](images/Screen_Shot_2023_04_24_at_11_42_33_PM.png)
![Screen_Shot_2023_04_24_at_11_37_42_PM.png](images/Screen_Shot_2023_04_24_at_11_37_42_PM.png)

--
--
-- Looking to explain everything that was loaded when the AMP was deployed in a few steps here - files, data, and code (model and App are in the next 2 labs already)
2.


## Lab 2 - Explore and test the deployed model

1. Go to the `Projects` page and click on the project we created now.
2. One of the steps that AMP executed was productionalize the model and make accessible via a REST API.
3. Click on the Models page and then the model that was deployed.
4. We will now test the real time scoring of the model that was just deployed.
5. The `Test Model` section contains sample input populated automatically. You can also pass your own sample Inputs in the format given. Then click Test
6. The model gets called with the feature we just sent through and a prediction result is given back. Value of  1 predicts that the flight will be delayed, 0 means the flight will not be delayed

![Screen_Shot_2023_04_25_at_12_03_54_AM.png](images/Screen_Shot_2023_04_25_at_12_03_54_AM.png)

## Lab 3 - Explore and test the Application

The AMP deployed a visual dashboard to expose the results from the Machine Learning pipeline for the business users. In this lab, we will access the Analytical Application

1. While on the project we created in the previous labs, click on `Applications`
2. There will be an app called `Application to serve flight prediction front end app`. Click the link.
3. This will take you to a visual dashboard, where you can pass various inputs to the model and get a prediction back. You can use your own datapoints as inputs to this app.

![Screen_Shot_2023_04_25_at_12_08_43_AM.png](images/Screen_Shot_2023_04_25_at_12_08_43_AM.png)

# 04_predict

In this lab, we will explore and test the end\-to\-end machine learning project we created in [03_visualize Lab 1](03_visualize.md#lab-1-deploy-machine-leaning-applied-machine-learning-prototype-amp) using Cloudera Machine Learning (CML).

The primary goal of this project that we deployed is to build a gradient boosted \(XGBoost\) classification model to predict the likelihood of a flight being canceled based on years of historical records. To achieve that goal, this project demonstrates the end\-to\-end Machine Learning journey for model training and inference using Spark on CML. Additionally, this project deploys a hosted model and front\-end application to allow users to interact with the trained model.

## Pre-requisite

1. Please ensure that you have completed the [03_visualize Lab 1](03_visualize.md#lab-1-deploy-machine-leaning-applied-machine-learning-prototype-amp) to deploy the Applied Machine Learning Prototype (AMP) for `Canceled Flight Predicion`.


## Lab 1 - Explore Machine Learning AMP Project

1. Open Cloudera Machine Learning (CML) `Canceled Flight Prediction` project Overview page

   * If you just completed the [03_visualize](03_visualize.md#03_visualize) phase, in the left nav click on Overview.

![CML_Overview_left_nav.png](images/CML_Overview_left_nav.png)

   * If not, you can always go back to CDP Home Page by clicking the bento menu icon in the top left corner, click on Home, select the `Machine Learning` tile, click on the Workspace available in your Machine Learning page (found under `Workspace`), find and select the `Canceled Flight Prediction` project tile.  
![Screen_Shot_2023_04_24_at_11_33_56_PM.png](images/Screen_Shot_2023_04_24_at_11_33_56_PM.png)
![Screen_Shot_2023_04_24_at_11_42_33_PM.png](images/Screen_Shot_2023_04_24_at_11_42_33_PM.png)
![Screen_Shot_2023_04_24_at_11_37_42_PM.png](images/Screen_Shot_2023_04_24_at_11_37_42_PM.png)

2. On the Overview page you can preview the materials that were created as part of deploying the AMP.

   * AMPs are examples of common problems in the machine learning field demonstrate how to fully use the power of Cloudera Machine Learning (CML). AMPs can be an excellent way to get started as they show you how to solve problems similar to your own business use cases.
![AMP_overview_page.png](images/AMP_overview_page.png)

   * On the initial view into the `Canceled Flight Prediction` AMP Overview page you will see the status bar which is a recap of what you executed in [03_visualize Lab 2](03_visualize.md#lab-2-configure-and-deploy-canceled-flight-prediction-amp)
![AMP_status_banner.png](images/AMP_status_banner.png)

   * Scroll down on the right side of the screen, as you do you will see the following that was deployed as part of this AMP

      * `Models` - 
         - has been deployed and it is currently running.  You can see the resources that are allocated to the running model

         - can be deployed in a matter of clicks, removing any roadblocks to production. They are served as REST endpoints in a high availability manner, with automated lineage building and metric tracking for MLOps purposes.
![AMP_models.png](images/AMP_models.png)

      * `Jobs` - 
      - can be used to orchestrate an entire end-to-end automated pipeline, including monitoring for model drift and automatically kicking off model re-training and re-deployment as needed.
![AMP_jobs.png](images/AMP_jobs.png)

      * `Files` - 
![AMP_files.png](images/AMP_files.png)

      * `Applications` - 
      - deliver interactive experiences for business users in a matter of clicks. Frameworks such as Flask and Shiny can be used in development of these Applications, while Cloudera Data Visualization is also available as a point-and-click interface for building these experiences


      * `README.md` for Canceled Flight Prediction project that describes what this model is trying to accomplish and how to use it.
![AMP_readme.png](images/AMP_readme.png)



## Lab 2 - Explore and test the deployed model

1. One of the steps that AMP executed was productionalize the model and make accessible via a REST API.
2. Click on the Models page and then the model that was deployed.
3. We will now test the real time scoring of the model that was just deployed.
4. The `Test Model` section contains sample input populated automatically. You can also pass your own sample Inputs in the format given. Then click Test
5. The model gets called with the feature we just sent through and a prediction result is given back. Value of  1 predicts that the flight will be delayed, 0 means the flight will not be delayed

![Screen_Shot_2023_04_25_at_12_03_54_AM.png](images/Screen_Shot_2023_04_25_at_12_03_54_AM.png)

## Lab 3 - Explore and test the Application

The AMP deployed a visual dashboard to expose the results from the Machine Learning pipeline for the business users. In this lab, we will access the Analytical Application

1. While on the project we created in the previous labs, click on `Applications`
2. There will be an app called `Application to serve flight prediction front end app`. Click the link.
3. This will take you to a visual dashboard, where you can pass various inputs to the model and get a prediction back. You can use your own datapoints as inputs to this app.

![Screen_Shot_2023_04_25_at_12_08_43_AM.png](images/Screen_Shot_2023_04_25_at_12_08_43_AM.png)

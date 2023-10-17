# 04_predict

As part of the `Predict` phase, we will explore and test the end\-to\-end machine learning project we created in [03_visualize - Lab 1](03_visualize.md#lab-1-deploy-machine-leaning-applied-machine-learning-prototype-amp) using Cloudera Machine Learning (CML).

The primary goal of this project that we deployed is to build a gradient-boosted \(XGBoost\) classification model to predict the likelihood of a flight being canceled based on years of historical records. To achieve that goal, this project demonstrates the end\-to\-end Machine Learning journey for model training and inference using Spark on CML. Additionally, this project deploys a hosted model and front\-end application to allow users to interact with the trained model.

## Prerequisites

1. Please ensure that you have completed [00_prereqs](00_prereqs.md) to deploy the Applied Machine Learning Prototype (AMP) for `Canceled Flight Prediction`.
2. Please ensure that you have completed [01_ingest](01_ingest.md#01_ingest) to ingest the data needed for Visualizations.

## Lab 1 - Explore Machine Learning AMP Project

1. Open Cloudera Machine Learning (CML) `Canceled Flight Prediction` project Overview page

    - If you just completed the [03_visualize](03_visualize.md#03_visualize) phase, in the left nav click on Overview.

    ![CML_Overview_left_nav.png](images/CML_Overview_left_nav.png)

    - If not, you can always go back to the CDP Home Page by clicking the bento menu icon in the top left corner, clicking on Home, selecting the `Machine Learning` tile, clicking on the Workspace available on your Machine Learning page (found under the `Workspace` column), find and select the `Canceled Flight Prediction` project tile.
   
    ![Screen_Shot_2023_04_24_at_11_33_56_PM.png](images/Screen_Shot_2023_04_24_at_11_33_56_PM.png)

    ![Screen_Shot_2023_04_24_at_11_42_33_PM.png](images/Screen_Shot_2023_04_24_at_11_42_33_PM.png)

    ![Screen_Shot_2023_04_24_at_11_37_42_PM.png](images/Screen_Shot_2023_04_24_at_11_37_42_PM.png)

2. On the Overview page, you can preview the materials that were created as part of deploying the AMP.

    - AMPs are examples of common problems in the machine learning field and demonstrate how to fully use the power of Cloudera Machine Learning (CML). AMPs can be an excellent way to get started as they show you how to solve problems similar to your own business use cases.

    ![AMP_overview_page.png](images/AMP_overview_page.png)

    - On the initial view into the `Canceled Flight Prediction` AMP Overview page, you will see the status bar which is a recap of what you executed in [03_visualize Lab 2](03_visualize.md#lab-2-configure-and-deploy-canceled-flight-prediction-amp)

    ![AMP_status_banner.png](images/AMP_status_banner.png)

    As you scroll down the right side of the screen, notice the following AMP component deployments:

    - `Models`

         - A model has been deployed and it is currently running
         
         - View the resources that are allocated to the running model

         - Models can be deployed in a matter of clicks, removing any roadblocks to production. 
         
         - Served as REST endpoints in a high availability manner, with automated lineage building and metric tracking for MLOps purposes.
         
    ![AMP_models.png](images/AMP_models.png)

    - `Jobs`

        - Can be used to orchestrate an entire end-to-end automated pipeline, including monitoring for model drift and automatically kicking off model re-training and re-deployment as needed.

    ![AMP_jobs.png](images/AMP_jobs.png)

    - `Files`

    - Browse the file folder structure to view how project assets are organized

    ![AMP_files.png](images/AMP_files.png)

    - `Applications`
      
      - Deliver interactive experiences for business users in a matter of clicks. 
      - Frameworks such as Flask and Shiny can be used in the development of these Applications, while Cloudera Data Visualization is also available as a point-and-click interface for building these experiences

    - `README.md` for Canceled Flight Prediction project that describes what this model is trying to accomplish and how to use it.

    ![AMP_readme.png](images/AMP_readme.png)

## Lab 2 - Explore and Test the Deployed Model

1. One of the steps that AMP executed was to productionalize the model and make it accessible via a REST API.

2. Click on the Models page and then the model that was deployed.

3. We will now test the real-time scoring of the model that was just deployed.

4. The `Test Model` section contains sample input populated automatically. You can also pass your own sample Inputs in the format given. Then click Test

5. The model gets called with the feature we just sent through and a prediction result is given back. a value of 1 predicts that the flight will be delayed, while 0 means the flight will not be delayed.

    ![Screen_Shot_2023_04_25_at_12_03_54_AM.png](images/Screen_Shot_2023_04_25_at_12_03_54_AM.png)

## Lab 3 - Explore and Test the Application

The AMP deployed a visual dashboard to expose the results from the Machine Learning pipeline for the business users. In this lab, we will access the Analytical Application

1. While on the project we created in the previous labs, click on `Applications`

2. There will be an app called `Application to serve flight prediction front-end app`. Click the link.

3. This will take you to a visual dashboard, where you can pass various inputs to the 
model and get a prediction back. You can use your own data points as inputs to this app.

    ![Screen_Shot_2023_04_25_at_12_08_43_AM.png](images/Screen_Shot_2023_04_25_at_12_08_43_AM.png)

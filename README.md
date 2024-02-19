# HealthGuard ML-Powered Diabetes Prediction Application

## Overview
This project focuses on developing a machine learning model to predict the likelihood of diabetes in women above the age of 21. The dataset is sourced from Kaggle, containing various health indicators and diagnostic measurements. The goal is to create a simple and accurate prediction model without extensive development time.

## Architecture
1. **User Interface:** A web application developed using Streamlit, providing prediction and past prediction functionalities.
2. **Model Service (API):** Developed using FastAPI to serve the machine learning model, make predictions, and store predictions in the database.
3. **ML Model:** Decision tree used for prediction.
4. **Database:** PostgreSQL database used to store past predictions and features used for prediction.
5. **Prediction Job:** Scheduled job implemented with Airflow to periodically make predictions on ingested data.
6. **Data Validation:** Utilizing Great Expectations for data validation (blood pressure, glucose, age).
7. **Monitoring Dashboard:** Grafana dashboard to monitor data quality problems and model prediction issues, with two dashboards: 
    i) XY-plot for Age vs Pregnancies \n
    ii) Bar gauge.

## Workflow
### User-end:
i) Users access the web application to make on-demand predictions by filling in single-sample features or uploading a CSV file for multiple predictions.\n
ii) The web application calls the model service API to retrieve predictions, displaying them along with corresponding features.

### Prediction-end:
i) New data files are ingested and validated for quality using tools like Great Expectations. 
ii) Clean data files are stored, while files with data quality issues are flagged and saved in the database. 
iii) The prediction job, running every 5 minutes, checks for new clean data files. 
iv) The prediction job reads clean data files and makes API calls to the model service for predictions. 
v) Predictions are stored in the database for access to the latest and historical data.

## Installation
1. Install dependencies: `pip install -r requirements.txt`
2. Application setup: `streamlit run app.py`
3. Airflow: 
   - Install: `pip install apache-airflow`
   - Initialize Airflow Database: `airflow db init`
   - Start Web Server and Scheduler: `airflow webserver --port 8080` and `airflow scheduler`
4. Great Expectations: `pip install great_expectations`
5. Grafana: Download from the official website and follow installation instructions.

## Usage
### Prediction Webpage:
Use the streamlit webpage  to make predictions.
i) Fill in single-sample features using the provided form.
ii) To make multiple predictions, upload a CSV file without labels and click "Predict" for on-demand predictions.

### Past Predictions Webpage:
View past predictions on this webpage.
i) Select start and end dates.
ii) Choose the prediction source from the drop-down list (webapp, scheduled predictions, or all).
iii) View past predictions with corresponding features.

## Visualization
Utilize Grafana for Data Quality Monitoring, Model Performance Monitoring, Data Drift Detection, Dashboard Customization, Alerting, and Notifications.

## API Endpoints
The model service (API) exposes the following endpoints:
i) POST /predict: Make model predictions by providing necessary input features.
ii) GET /past-predictions: Retrieve past predictions along with used features.

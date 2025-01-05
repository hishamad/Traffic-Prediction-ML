# Electricity-Price-Forecasting-ML
This project is part of the course _Scalable Machine Learning and Deep Learning_ at KTH. 

## Problem Description 
The prediction problem targeted in this project is electricity price forecasting. The electricity price prediction is usually based on data such as historical electricity prices, power generation data and weather data. The goal of the project is to build a serverless real-time machine learning system that is able to predict the daily average electricity price in the Stockholm region.

## Data Description
There are four main data sources used in this project: 
- **Elpris**, which provides historical and real-time hourly electricity price data in Sweden. The data is divided into four geographical regions in Sweden: SE1, SE2, SE3 and SE4, which is the standard division used by all entities working with electricity in Sweden. As Stockholm is located in SE3, the data used in this project is only from this region. The prices are provided in both Euro and SEK but only prices in SEK are used here. As the hourly electricity prices vary too much between day and night, making it too complex for a machine learning model to learn, the data is combined into a daily average instead to make the prediction problem simpler.
- **Electricity Maps**,
- **Open-meteo**, which provides historical and real-time weather data. The data used includes only relevant features for this prediction problem such as temperature, wind speed, precipitation, wind direction and sunshine duration.
- **ENTSO-e**
## Solution Description 
The system architecture used in this project consists of four main pipelines that can be found in the notebooks folder:
- **Backfill pipeline**: in this pipeline, historical data is collected from the previously mentioned data sources. The collected data is then preprocessed and cleaned to make it ready to be saved. The final data is then saved into a feature store provided by Hopsworks. Data from each source is saved into a separate feature group resulting in three different groups(electricity prices, power generation and weather data).
- **Daily feature pipeline**: in this pipeline, real-time daily data is fetched from the different data sources and inserted into the different feature groups. This data will be later used in the inference pipeline to predict the daily electricity price based on it. This pipeline is scheduled to run daily using GitHub actions.
- **Training pipeline**: in this pipeline, the historical data stored in different feature groups on Hopsworks is fetched. Two different machine learning models are trained in this pipeline. The first model is an LSTM-based deep neural network as we are working with time series data. The second model is a gradient-boosted regrssion decision tree provided by the XGBoost library(XGBRegressor). For LSTM, the data is transformed using the MinMaxScaler from the Scikit-learn library. For XGBoost, the data is used as such transformation is not needed for it. The data is then split into training, validation and test data. For LSTM, the data is then transformed into sequences of length 3 to make it compatible with the model. The training data is used to train the model while the validation data is used for hyperparamter tuning. The test data is used for final evaluation. As this is a regression problem, the objective function used is the Mean Squared Error (MSE) is used for LSTM. The models are evalutated using both the MSE loss and the R^2 metric which measures how well the model fits the data. The best model is then chosen based on these two metrics.
- **Inference pipeline**:

## Results 
about GUI and model monitoring ... 

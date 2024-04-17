# TestCase

## Introduction
The process has been divided into four distinct phases in notebooks:
0. EDA (Exploratory Data Analysis) to explore Time Series Patterns and special features of the series.
1. Baseline to set a benchmark to beat.
2. Arima models.
3. Tree based models.
4. Final pipeline.

## Conclusions
0. The time series is not stationary (contains trend and is heterocedastic). It seems to have a breaking point in 2018. There is **no stable seasonal pattern repeated accross all years**: only 2017, 2018 and 2019 shows the same shape and distribution. Additionally, the year 2020 differs from the past years. Therefore, as is usually done with this year in time series modeling, it has been processed. As the series has data, the following year has to be predicted and it is not known beforehand whether the series is like this or is biased by the COVID-19 effect, it has been decided not to eliminate that year of data. Instead, **the average for 2019 and 2020 has been taken as 2020**. However, this year will not be taken into account to assess the quality of the models.
1. Multiple baselines have been fitted to data to find the best performer one. It turns out to be the simplest: **repeat the data observed last year** (according to MSE and MAE evaluated on three folds). Evaluation strategy has been set as **Expanding Window** with three folds (2018, 2019, 2020), but due to the nature of the data, only **the fold corresponding to 2019 will be the most important to choose a model** (see notebooks).
2. The modelling phase has started with ARIMA models, which could work well for univariate series. These models explain the future of the series from its own past. Once the patterns present in the series were identified, an AutoARIMA is trained which does not provide the expected results. Subsequently, an **ARIMA** is trained whose orders are chosen according to what was previously explored. The ARIMA outperforms the baseline results for 2019 and 2020.
3. XGBoost model is studied too, since tree based models like XGBoost have beaten traditional time series algorithms in time series problems (although the data complexity and size were greater than in this problem). **Time series features** have been added using TSFresh library. The base XGBoost model does not provide the desired outcomes, so a time-intensive hyperparameter search is conducted. This is done using hyperopt library, dividing the search phase in two consecutive stages: the first one to find the best learning hyperparameters and the second one to find the best regularization parameters. The metrics of the xgboost are worse than the baseline and arima, probably due to the complexity of the model and lack of complexity of the series. 
4. Finally, the process is encapsulated in a **pipeline** to run the whole process -data retrieval, data processing, model fitting and forecasting- using a single command. 

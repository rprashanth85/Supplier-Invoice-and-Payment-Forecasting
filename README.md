### Supplier Invoice and Payment Forecasting

**Raghu Prashanth**

#### Executive summary
This project focuses on visualizing and forecasting supplier invoices and payments to support financial planning and supplier management. By leveraging synthetic data simulating invoice and payment records with seasonality and trend-based indicators, the project employs time series models like SARIMA and classification techniques to identify high-risk invoices. The goal is to provide interactive visualizations and reliable forecasts, enabling businesses to anticipate invoice volumes, manage payments efficiently, and mitigate risks.

#### Rationale
Supplier invoice and payment management directly impacts financial stability and operational continuity. Late or missed payments strain supplier relationships and disrupt supply chains, while unmanaged invoice spikes can hinder liquidity planning. This project helps businesses anticipate payment needs, reduce risks associated with high-priority or delayed invoices, and improve supplier relationships, ensuring smooth operations and financial predictability.

#### Research Question
How can businesses effectively visualize and forecast supplier invoices and payments to enhance financial planning and reduce supplier-related risks?

#### Data Sources
The project uses synthetic data created using Python’s Faker and Pandas libraries, simulating:
	•	Supplier Invoices: Daily invoice records with varying volumes, urgency, supplier ratings, and seasonal adjustments.
	•	Supplier Payments: Historical payment records, including partial payments and payment delays.

This data reflects real-world scenarios such as seasonal spikes, and fluctuations.

#### Methodology
Data Creation
	Synthetic data created for Supplier Invoice for 10 different suppliers. And also supplier invoice payments are created for those payments. Amount, Volume, Payment Status, Payee Types, Items, etc added features related to the invoices and payments. 

Data Analysis
	As the data is synthetic there is no null values. Also normalized across. That is clear from the plots below. 
 
 <img width="1017" alt="Invoice Amount Over Time by Supplier" src="https://github.com/user-attachments/assets/5bc514c3-7ed2-4e4c-8c98-0689c9a14e7e">

<img width="1015" alt="Invoice Amount Distribution by Supplier" src="https://github.com/user-attachments/assets/41ba6114-c60b-455f-8775-5fe151696a6b">

<img width="1018" alt="Count of Payment and Status by Payee" src="https://github.com/user-attachments/assets/81cd8d8f-7d14-46c7-bd33-15c3ef731a4a">

 <img width="1015" alt="Total Invoice vs Payments Over Time" src="https://github.com/user-attachments/assets/7b72abb1-59a6-4b7a-9d93-7adae12d9595">


Created auto correlation plot to access seasonality and trend


<img width="1007" alt="Autocorrelation of Daily Invoice Amounts" src="https://github.com/user-attachments/assets/783e2035-b88f-4d0e-9b3b-ddada59abcd6">


Inferences
Non-Stationarity:
    The slow decay of autocorrelation suggests that the time series is non-stationary.
    Stationary series typically show autocorrelations that decay quickly to zero or oscillate around zero for higher lags.
Trend in the Data:
    The persistent positive autocorrelation indicates that the time series has a trend. This aligns with the earlier plot showing an upward trajectory in invoice amounts.
Seasonality:
    There are no clear periodic spikes in autocorrelation at fixed lags, so strong seasonality is not immediately apparent. However, other methods like Fourier transformations or decomposition might be needed for detailed seasonal analysis.

As a next step started analysing payment trends, residuals and stationarity, etc. Clearly there are seasonal and trend patterns available in the data. Applied ACF and PACF on Payment Amount to make the data stationary. I also did the seasonal decomposition using additive model but the standard deviation and MAE results were better for multiplicative model decomposition.  

![Multiplicative Decomposition](https://github.com/user-attachments/assets/411e3b9e-1519-4ea1-a621-ca4207ee725d)

<img width="790" alt="Comparison of Residuals " src="https://github.com/user-attachments/assets/f3d28a44-d001-4812-830c-aac7299d9a15">

<img width="291" alt="ACF and PACF" src="https://github.com/user-attachments/assets/444d9116-fb16-494f-8271-70a89afdadee">


Data Modeling

Payment Modeling

	Performed couple of basic modeling with ARIMA and Prophet. For this basic analysis I just used Amount and Date for a daily period.
 	Performed another of basic modeling with ARIMA for the same features with 12 month period.

Supplier Specific Invoice Forecasting

	Based on Supplier Invoice Amount distribution noticed that Supplier 9 with hight trends and Supplier 6 with less trend or seasonal comparatively. 
 	Created 2 different data frame for each supplier.
  	For supplier 9, basic modeling using applied XGBoost and Prophet and optimzed Prophet with additional features like moving average and relative indexes. 
   	For Supplier 6, tried modeling using SARIMAX and also tried Grid Search CV type (as SARIMAX is not supported using Grid Search we will have to build one)


#### Results

**Supplier 9 - Forecasting - No Grid Search CV applied**

**Basic Modeling** 

XGBoost - RSME: 8741.365078169816

Prophet - RMSE: 1193.040209585796

<img width="586" alt="Basic Prophet" src="https://github.com/user-attachments/assets/7e824e4b-e67c-4f63-997e-c4b82e0396ed">


**Optimized Modeling**

Prophet - RMSE: 272.4380126809856

<img width="606" alt="Optimized Prophet" src="https://github.com/user-attachments/assets/b68d8e39-9794-4d6b-a454-9503e0915e2d">



**Supplier 6 - Forecasting - SARIMAX Grid Search CV **kind** applied**

This forecasting requires more optimzation. Only used Invoice Amount as of now for modeling. Based on that below are the best parameters and RSME

<img width="584" alt="SARIMAX Wrapper" src="https://github.com/user-attachments/assets/3c99a884-b901-45d2-a70c-03525c886a21">


Best Parameters: {'diff': True, 'order': (1, 1, 1), 'seasonal_diff': True, 'seasonal_order': (1, 1, 1, 7)}
Test RMSE: 308.0244282758301

<img width="570" alt="SARIMAX Test vs Forecast" src="https://github.com/user-attachments/assets/5ca21ed7-3f3a-42e6-9d17-5011ccc1e63d">



#### Next steps
1) Optimizing supplier specific forecasting using SARIMAX by introducing moving averages and other factors.
2) Explore Supplier Payment forecasting using SARIMAX and optimizing Prophet model.
3) Improve model accuracy using RSME or MAE and avoid over fitting.
4) Use Validation set and explore the optimzed forecasting models for Supplier Invoices and Payments.

#### Outline of project

[Initial Report and EDA.ipynb](https://github.com/rprashanth85/Supplier-Invoice-and-Payment-Forecasting)


##### Contact and Further Information
https://www.linkedin.com/in/raghu-prashanth-056b1814/

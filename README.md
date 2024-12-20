### Supplier Invoice and Payment Forecasting

**Raghu Prashanth**

#### Executive summary
This project focuses on visualizing and forecasting supplier invoices and payments to support financial planning and supplier management. By leveraging synthetic data simulating invoice and payment records with seasonality and trend-based indicators, the project employs time series models like Prophet, SARIMAX and classification techniques to identify high-risk invoices. The goal is to provide interactive visualizations and reliable forecasts, enabling businesses to anticipate invoice volumes, manage payments efficiently, and mitigate risks.

#### Rationale
Supplier invoice and payment management directly impacts financial stability and operational continuity. Late or missed payments strain supplier relationships and disrupt supply chains, while unmanaged invoice spikes can hinder liquidity planning. This project helps businesses anticipate payment needs, reduce risks associated with high-priority or delayed invoices, and improve supplier relationships, ensuring smooth operations and financial predictability.

#### Research Question
How can businesses effectively visualize and forecast supplier invoices to enhance financial planning and reduce supplier-related risks?

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


#### Data Modeling

Reading the paper by Agarwal, P. (2024). Enhancing Financial Forecasting in ERP Systems using XGBoost: A [Robust Sales Prediction Model] 
(https://www.atlantis-press.com/proceedings/icetsbp-24/126004216?utm_source=chatgpt.com)

This study presents a robust sales prediction model utilizing XGBoost to enhance financial forecasting within ERP systems. The model achieved a training accuracy of 99.90% and a test accuracy of 97.20%, demonstrating XGBoost’s effectiveness in improving ERP financial forecasts.

Following the study I started exploring XGBoost ensemble other models. In each step below I have updated the model based on findings.

This study is hugely focused on Supplier Invoices Forecasting and touchbase on Supplier Payment Forecasting and Supplier Invoice Aging.

1. XGBoost 

Initially, XGBoost was used for supplier invoice forecasting, but its root mean squared error of 8741.36 indicated poor performance. The model struggled with sequential patterns and seasonality, highlighting the limitations of using XGBoost alone for time-series forecasting.

2. Prophet

Using Prophet without seasonality yielded a significantly lower root mean squared error of 1193.04. Incorporating quarterly seasonality further reduced to 272.44, indicating Prophet’s strength in capturing recurring patterns. However, residual errors suggested unmodeled complexities.

<img width="606" alt="Optimized Prophet" src="https://github.com/user-attachments/assets/14c7739c-08a7-4b33-a018-63176ba0445f" />


3. SARIMAX + LSTM

This approach combined SARIMAX for baseline forecasting with LSTM to model residuals. While the root mean squared error improved to 51.48, residual analysis revealed misaligned forecasts, heavy tails, and unmodeled non-linear patterns, making the model insufficient for extreme variations.

4. Prophet + LSTM

Replacing SARIMAX with Prophet for baseline predictions resulted in an improved root mean squared error of 49.67, but residual errors and P-ACF analysis still indicated significant spikes and seasonal gaps, hinting at potential unmodeled seasonality.

![Prophet + LSTM](https://github.com/user-attachments/assets/a798939c-8688-4d6c-9573-ecba08d12c66)


![Residuals](https://github.com/user-attachments/assets/44a08f65-f60d-48f6-8fe2-4f7350d8fec6)


5. Prophet + ARIMA + LSTM

Adding ARIMA (seasonal parameters: 2,0,0) to Prophet + LSTM reduced short-term errors. Test root mean squared error was 89.86, and for the next 20 days, root mean squared error dropped to 7.75. However, overfitting became apparent during training.

![Prophet + ARIMA + LSTM](https://github.com/user-attachments/assets/4437de6f-95b5-414c-bd0d-94457a9147ff)


6. Fine-Tuning and Exponential Smoothing

To mitigate overfitting, exponential smoothing and additional LSTM layers were introduced. While the test root mean squared error decreased to 6.92, the root mean squared error for the next 20 days rose to 154.73, signaling a need for further adjustments in handling future residuals.

![Prophet + ARIMA + LSTM + Exponential Smoothing](https://github.com/user-attachments/assets/d271586a-d511-498d-be07-2f6ee2e68c06)


7. Fine-Tuning and Adding Weights

The final model incorporated XGBoost and Ridge Regressors, removing LSTM to reduce complexity. Weight adjustments optimized the hybrid approach. Results:
	•	Meta-Model root mean squared error for Train Set: 4.71
	•	Meta-Model root mean squared error for Test Set: 5.81
	•	Meta-Model root mean squared error for Next 20 Days: 12.27
With no overfitting or bias and closely matching forecast variance (1900.22) with actual variance (1906.80), this approach provided robust generalization.

![Prophet+ARIMA+XGBoost+Ridge](https://github.com/user-attachments/assets/2baf3de8-a166-400c-a9f2-1c726b6ac291)


#### Note on Residuals and Accuracy

Throughout the modeling process, RMSE (Root Mean Squared Error) was used as the key metric to measure the accuracy of all models. After experimenting with hybrid models like Prophet + LSTM, I realized the critical importance of residuals in improving predictions. This understanding led to the integration of Ridge, ARIMA and XGBoost to model residuals effectively, significantly enhancing forecast accuracy.


#### Results

Model Performance Summary

1.	Baseline Models: XGBoost alone performed poorly, while Prophet with seasonality showed promise.
2.	Hybrid Models: Combinations of SARIMAX, LSTM, ARIMA, and Prophet progressively improved accuracy, reducing residual errors but introduced overfitting and misalignment in some cases.
3.	Fine-Tuned Meta-Model: The final hybrid approach, using XGBoost, Ridge Regression, ARIMA and Prophet, delivered the best performance with excellent generalization and reduced overfitting. RMSE for test and next 20 days was 5.81 and 12.27, respectively. 

	There by concludidng that the supplier invoice forecasting model evolved significantly through iterative improvements, with the final hybrid approach achieving robust results. However, the next 20-day forecast still requires refinement.

#### Next steps
1) Improve Forecasting little bit more
2) Appluy similar methodology followed for Supplier invoice Forecasting for Supplier Payment Forecasting and Supplier Invoice Aging
3) Involve real world objects like cost center, funds, projects kind of tags into the modeling. 


#### Outline of project

[Initial Report and EDA.ipynb](https://github.com/rprashanth85/Supplier-Invoice-and-Payment-Forecasting/blob/main/Initial%20Report%20and%20EDA.ipynb)

[Forecasting and Fine Tuning.ipynb](https://github.com/rprashanth85/Supplier-Invoice-and-Payment-Forecasting/blob/main/Forecasting%20and%20Fine%20Tuning.ipynb)


##### Contact and Further Information
https://www.linkedin.com/in/raghu-prashanth-056b1814/

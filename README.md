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




Data Modeling

#### Results
What did your research find?

#### Next steps
1) Optimizing supplier specific forecasting using SARIMAX by introducing moving averages and other factors.
2) Explore Supplier Payment forecasting using SARIMAX and optimizing Prophet model.
3) Improve model accuracy using RSME or MAE and avoid over fitting.
4) Use Validation set and explore the optimzed forecasting models for Supplier Invoices and Payments.

#### Outline of project

[Initial Report and EDA.ipynb](https://github.com/rprashanth85/Supplier-Invoice-and-Payment-Forecasting)


##### Contact and Further Information
https://www.linkedin.com/in/raghu-prashanth-056b1814/

# Super-Store-Sales-Forecast
## 📊 Dataset Overview
The dataset contains historical retail sales data from a Superstore, primarily used to analyze and forecast sales over time. The focus of this project is on understanding **temporal sales patterns** rather than detailed customer or product-level behavior.

The key variable used for analysis is the **sales value**, along with the **order date**, which is converted into a time-based format. Sales data is aggregated at a **monthly level** to construct a univariate time series suitable for forecasting.

Each record contributes to the overall sales trend for a given time period, enabling:
- Trend analysis
- Seasonality detection
- Time series modeling and future sales prediction

This dataset is well-suited for applying **time series forecasting techniques such as ARIMA**, helping simulate real-world demand forecasting scenarios in retail businesses.


## 🛒 Order & Customer Information

- `order_id` – Unique identifier for each order

- `order_date` – Date when the order was placed

- `ship_date` – Date when the order was shipped

- `ship_mode` – Shipping method used

- `customer_id` – Unique customer identifier

- `customer_name` – Name of the customer

- `segment` – Customer segment (Consumer, Corporate, Home Office)

## 🌍 Geographical Details

- `country` – Country of sale

- `city` – City where the order was delivered

- `state` – State of delivery

- `postal_code` – Postal code of the delivery location

- `region` – Sales region (East, West, Central, South)

## 📦 Product Information

- `product_id` – Unique product identifier

- `category` – Product category

- `sub_category` – Product sub-category

- `product_name` – Name of the product

## 💰 Sales Metrics

- `sales` – Sales value generated from the transaction

## 📐 Dataset Size

- **Rows:** ~9,800 transactions

- **Columns:** 18 features  

# Exploratory Data Analysis (EDA)



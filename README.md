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

## 📥 Importing and Understanding the Data
- Imported required Python libraries (Pandas, NumPy, Matplotlib, Seaborn) and loaded the Super Store sales CSV file into a Pandas DataFrame.
- Inspected the dataset to understand its structure, key columns (sales and order date), data types, and values in few columns.

## ⚙️ Preprocessing
- Dropped the unnecessary columns and converted Order Date and Ship Date columns to datetime format to enable time-based analysis.

## 📊 Takeaways from Visualizations:
- West region contributes the highest share of total sales (~31%), indicating it is the strongest performing region, while the South region contributes the least (~17%), highlighting potential opportunities for growth or targeted business strategies there.
- California and New York dominate total sales, significantly outperforming other states, indicating that sales are heavily concentrated in a few high-performing regions.
- Standard Class shipping generates the highest sales, indicating it is the most preferred and cost-effective option for customers, while Same Day shipping contributes the least, suggesting lower demand for premium delivery services.
- Despite having 1,800+ products, just five items (Canon imageCLASS 2200 Advanced Copier, Fellowes PB500, Cisco TelePresence EX90, HON 5400 Series Chairs, and GBC DocuBind TL300) accounted for ~7% of total sales.
- Order volume remains relatively steady from March to August, followed by a sharp surge in September and peak activity in November–December, indicating strong year-end seasonality.

## 📈 Monthly Sales Trend Analysis
- Visualized the distribution of sales and identified a strong right skew, indicating that a small number of high-value transactions significantly impact total sales.

- Removed extreme outliers using the Z-score method (±3) to reduce noise and obtain a more reliable representation of underlying sales patterns.

- Aggregated sales data at a monthly level after sorting by order date, revealing a clear upward trend in sales over time.

- Plotted monthly sales trends, which showed consistent growth along with noticeable fluctuations across months.

- Performed seasonal decomposition to separate trend, seasonal, and residual components, confirming the presence of strong annual seasonality.

- Analyzed residuals, which were mostly centered around zero, indicating that trend and seasonality explain most of the variation in sales.

- Compared monthly sales across years (2015–2018), showing that each year outperformed the previous one, reflecting increasing demand and business growth.


# Train–Test Split and Stationarity Check:
- The dataset spans 2015–2018, with 2015–2017 used as the training set and 2018 used as the test set.
- Applied the Augmented Dickey–Fuller (ADF) test to assess stationarity and confirmed that the training data is stationary, meaning its statistical properties (mean and variance) remain constant over time.
- Non-stationarity causes model parameters to change over time, meaning past patterns do not reliably predict future behavior. Since most forecasting models assume stationarity, this leads to poor forecasting performance when the data is non-stationary.
- If the data is non-stationary, we apply differencing to remove trends, seasonal differencing to remove seasonality, and log or Box–Cox transformations to stabilize variance. We apply second-order differencing if the data is not stationary after the first order differencing.
- Seasonal differencing removes repeating seasonal mean shifts and long-term seasonal non-stationarity.
  Example (monthly data, s = 12): High every December, low every January → removed, So after differencing:
  -The average December ≈ average January
  - Mean becomes stable → stationarity achieved
  Seasonal differencing does NOT remove:
  - Correlation between same seasons across years
  - Systematic seasonal dependence
- In some datasets, the mean and variance change together naturally—especially when the variance depends on the mean (e.g., log-normal data). In such cases, dividing the data into groups and comparing mean and variance is not reliable for assessing stationarity. Therefore, statistical tests like the Augmented Dickey–Fuller (ADF) test are used.

# ACF and PACF plots:
## 📊 Autocorrelation Function(ACF) Plot:
- Measures how a value is correlated with its past values
  - ACF(k)=Corr(Yt​,Yt−k​)
  - Depends on past errors
  - ACF helps find q



## 📊 PACF Plots:
- Measures the direct correlation with a lag, after removing effects of intermediate lags
  - PACF(k)=Corr(Yt​,Yt−k​∣Yt−1​,Yt−2​,…,Yt−k+1​)
  - Depends on past values
  - PACF helps find p
 
  
# Forecasting Models:
## 🔹 AR (AutoRegressive) model:
- AR(p):
  - AR(p) Model: Y_t = φ₁ Y_{t−1} + φ₂ Y_{t−2} + … + φₚ Y_{t−p} + ε_t
  - PACF → cuts off after lag p
  - ACF → decays gradually
    
## 🔹 MA (Moving Average) model:
- MA(q):
  - MA(q) Model: Y_t = ε_t + θ₁ ε_(t−1) + … + θ_q ε_(t−q)
  - ACF → cuts off after lag q
  - PACF → decays gradually
- We use ACF to identify the MA order because MA models are defined by how long past shocks (errors) remain correlated with the present, and ACF directly measures correlation caused by those past shocks.
- ACF helps identify how many past shocks (errors) influence the current value; if q = 8, then the current value depends on the last 8 error terms
- ACF measures: “Is today statistically related to the value k periods ago?”- In an MA model, this relation comes only from shared shocks.
## 🔹 ARMA or ARIMA model
- ARIMA (AutoRegressive Integrated Moving Average) combines AR(p), differencing 𝑑, and MA(q) components to model time-series data that becomes stationary after differencing.
- The “Integrated” part (d) handles non-stationarity by differencing the data to remove trends and stabilize the mean over time.
- This model is used when both the ACF and PACF decay gradually, with no sharp cutoff observed in either plot. 
## 🔹 PMDARIMA
- Automatically identifies the optimal ARIMA/SARIMA orders `(p, d, q)` and `(P, D, Q, s)` by using multiple combinations of paramaters.
- Reduces manual trial-and-error by handling differencing, seasonality detection, and model selection.
  
## 🔹 SARIMA or SARMA
- SARIMA (Seasonal AutoRegressive Integrated Moving Average) extends ARIMA by incorporating seasonal AR, differencing, and MA components to model time-series data with seasonality.
- The seasonal terms `(P, D, Q, s)` handle repeating patterns by applying seasonal differencing and seasonal AR/MA effects, making the series stationary across seasons.
- This model is used when the ACF and PACF show significant patterns at seasonal lags (multiples of the seasonal period `s`), along with gradual decay at non-seasonal lags.

## 🔹 FB Prophet
- FB Prophet is a time-series forecasting model designed to handle trends, seasonality, and holiday effects with minimal manual tuning.
- It models time series as an additive combination of trend, seasonality (daily, weekly, yearly), and external events, making it robust to missing data and outliers.
- This model is preferred when the data shows strong seasonal patterns, irregular intervals, or structural changes, and when interpretability is important.

# Comparing the models and Forecasting the model with the best model
- Different forecasting models were compared using error metrics such as MSE and MAE to assess their predictive performance.
- Based on the evaluation results, the model with the best performance was selected as the final forecasting model.
- The best-performing model was used to generate sales forecasts for the next six months.







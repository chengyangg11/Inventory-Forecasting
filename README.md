# Forecasting Inventory for Favorita Supermarket

In this project, I use time-series forecasting to forecast store sales for Corporación Favorita, a large Ecuadorian-based grocery retailer.

Forecasts are especially relevant to brick-and-mortar grocery stores, which must dance delicately with how much inventory to buy. Predict a little over, and grocers are stuck with overstocked, perishable goods. Guess a little under, and popular items quickly sell out, leading to lost revenue and upset customers. More accurate forecasting, thanks to machine learning, could help ensure retailers please customers by having just enough of the right products at the right time.

Current subjective forecasting methods for retail have little data to back them up and are unlikely to be automated. The problem becomes even more complex as retailers add new locations with unique needs, new products, ever-transitioning seasonal tastes, and unpredictable product marketing.
<br><br>
Business objectives:
Based on 4 years of data, optimize demand forecast to help stores reduce overstocked wastage by XX% and out-of-stock instances by XX%
## 1. Data 
There are many factors that influence the inventory level of a supermarket:

<li>Historical Sales Data: This allows us to analyze trends and identify sales patterns by stores, categories, and brands.
<li>Inventory Data: Inventory data includes information about stock levels, supplier lead times, and delivery schedules. This information is crucial for predicting when stockouts may occur and how to optimize stock levels.

<li>Customer Data: Customer data includes information about demographics, purchase history, and shopping habits. As the demographic of a town changes, the consumption of certain products also changes. 

<li>Seasonal Data: Seasonal data includes information about holidays, weather patterns, and other events that may affect consumer behavior. 

<li>Competitor Data: Competitor data includes information about other stores in the area, their pricing strategies, and their promotional efforts. 

<li>Marketing Data: Marketing data includes information about the effectiveness of different marketing channels, and how much we are spending on each of them. 


The company provided 3 types of data:
<li> Oil price data from 2013 to 2017. This is a close proxy for inflation since Ecuador is an oil-dependent economy.
<li> Store locations
<li> Holidays Events (national, regional, or local holiday) 

  
I don't have access to any data about the company's marketing strategy, inventory, competitors, or the customers' demographic in each location. However, I found and added the following external data sources that I believe correlate with the store daily sales:
<li>Oil prices and Exxon stock prices from Yahoo Finance - These will help fill out missing oil price data in the dataset provided by the company
<li>CPI, Population, GDP per CAPITA from world bank 

I then created 2 datasets: one for nationwide aggregated daily sales and one for daily sales at store level 

[Google Colab - Data Cleaning](https://colab.research.google.com/drive/1b-MgQHnU4bJG1scwBxubLt3B1Ca9Yk72#scrollTo=lCqUgX6g_pxS) 

[Data File - Nationwide daily sales](https://drive.google.com/drive/u/0/folders/1pUolHpeB9aaehKY1ZSZDQ78ws6m_DpJR)

[Data File - Daily sales by store](https://drive.google.com/drive/u/0/folders/1pUolHpeB9aaehKY1ZSZDQ78ws6m_DpJR)

## 2. EDA

[Google Colab - EDA](https://colab.research.google.com/drive/12Sr-JmsNdWy2CJsxZeYP0lNE__i5sXl7#scrollTo=RrbzXNzciGaS)

**Seasonality**: Sales data shows weekly and monthly seasonality. Sales rise toward the weekend within a week, and toward December within a year. I also examined the seasonality against salary days and national, regional, and local holidays but there was no material difference among those groups.

Figure: Weekly trend

![image](https://user-images.githubusercontent.com/70331438/224578090-c191e607-7062-49d7-a5fb-fd18ad1e6dde.png)

Figure: Monthly trend

![image](https://user-images.githubusercontent.com/70331438/224578225-4793ddff-6d3a-42f4-8bf5-ce04c947d532.png)


**Macro-economics**: Supermarket sales are strongly correlated with the population in the vicinity,but negatively correlated with the inflation index (CPI) and the global crude oil prices. This is understandable as Ecuador is an oil-dependent economy. If the oil prices go up, inflation will also rise and people’s purchasing power will weaken

![image](https://user-images.githubusercontent.com/70331438/224578241-7fdabb76-d81c-48d3-8836-0c6b3a236a10.png)

**Historical sales**: There is a spike in the first lag of PACF, signalling that yesterday sales have strong influences on the current day figures. Customers’ shopping habits tend to be stable over a small time frame so the recent days’ sales are some of the best predictors for tomorrow’s sales.     

![image](https://user-images.githubusercontent.com/70331438/224578255-9abe786f-846c-4cab-ab91-824f20cf3e0b.png)

## 3. Modelling 

[Google Colab - Modelling](https://colab.research.google.com/drive/1vsbMI9QyX2Tk0tfIkUnTcGEYF228uOtM#scrollTo=BMjQQ59_8G8U)

Big supermarket chain restock daily so we focus on daily sales forecast, as opposed to weekly or monthly. I will first explore nationwide daily sales forecast, then go down to store level. I limit the scope to the top 14 stores that contribute 50% revenue. I will be using 2 main methods:
<li>Univariate Time Series Forecasting (ARIMA, SARIMA, AR, MA, etc.): These methods are capable of only looking at the target variable. This means no other regressors (more variables) can be added into the model.
<li>Multivariate Time Series Forecasting: Linear, Tree, XGBoost
<br><br>
I evaluate these models using MAPE (mean average percentage error). XGBoost has the best performance with MAPE = 0.17. This is the only model that picks up the seasonality dip on the first day of a year. 

![image](https://user-images.githubusercontent.com/70331438/224578286-2b444969-a33e-4b0d-84c7-3621c5d55ca2.png)

## 4. Next Steps

<li>Develop a predictive model for the popular products by store. This requires more granular level of data than what we currently have</li>
<li>Build a webapp that shows a store manager his/her store's historical performance and the next few weeks' outlook to facilitate inventory replenishment</li>

Figure: Streamlit webapp UI

![image](https://user-images.githubusercontent.com/70331438/224578320-8ea97a3c-303f-43a6-8ae7-c31fb04cff54.png)

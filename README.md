# Financial Planning Tools
The purpose of the financial planning tools assignment is to create two financial planning applications: 

(1) one for personal savings 

(2) one for retirement portfolio.


This will allow you to build a portfolio that meets a client’s short-term and long-term savings and retirement goals.

---

## Technologies

The financial planner leverages Python 3.8+ and utilizes the following project libraries and dependencies:
* [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) - a single integrated development environment (IDE) that allows you to write and run Python programs and review the results in one place
* [Pandas](https://pandas.pydata.org/) - a software library designed for open source data analysis and manipulation
- [matplotlib](https://matplotlib.org/) - library for creating visualizations in Python
- [alpaca_trade_api](https://alpaca.markets/data) - SDK used to interact with Alpaca API for stock trading
- MCForecastTools
- pathlib
- os - functions for interacting with the operating system
- requests - library to access data via APIs
- json - library that converts data from an API into a human-readable format
- dotenv


---

## Installation Guide


Download Anaconda for your operating system and the latest Python version, run the installer, and follow the steps. Restart the terminal after completing the installation. Detailed instructions on how to install Anaconda can be found in the [Anaconda documentation](https://docs.anaconda.com/anaconda/install/).

Install the python-dotenv Library

With the python-dotenv library, you can read key-value pairs from an environment file (.env) and add them as environment variables.

To install this library, run the following command in your terminal:

```python
pip install python-dotenv
```
Install the Alpaca SDK

Alpaca is an API for stock trading. With the Alpaca SDK, you can interact with the Alpaca API.

To install this SDK, run the following command in your terminal:

```python
pip install alpaca-trade-api
```
If you have not already done so, you need to generate your API key and secret key from the Alpaca API. For additional instructions, refer to [Alpaca Markets sign-up page](https://app.alpaca.markets/signup)

---

## Usage
The financial planner is hosted on the following GitHub repository at: https://github.com/nguyenthuyt/financial_planner   

### **Run instructions:**
To run the financial planner, simply clone the repository or download the files and launch the **financial_planning_tools.ipynb** in JupyterLab

To launch JupyterLab, follow these steps:

In your open terminal window (Terminal for macOS or Git Bash for Windows), navigate (`CD`) to the repo directory and then confirm that the term (dev) appears at the beginning of your command prompt. Type:
```python
conda activate dev
```

Then type: 
```python
jupyter lab --ContentsManager.allow_hidden=True
```

An instance of the JupyterLab user interface automatically opens in your browser. On the left-hand side menu, double-click the **financial_planning_tools.ipynb** file to open the notebook.

Navigate to **'Run'** on the menu bar and select **'Run All Cells'** from the drop-down menu. Otherwise, run each individual cell with ctrl+enter.

![JupyterLab browser running all cells](images/run_kernels.PNG)


If running each cell individually, first run the cell to import the required libraries and dependencies: 

![JupyterLab notebook import cell](images/import_cell.PNG)


### **Step 1: Import the data**
Before using, it is required to load market data. In this analysis market data is loaded by making API calls by using both the Python Requests library and SDKs to access high quality and current data. 

Before making any API calls, create a new .txt file to store your alpaca_api_key and alpaca_secret_key. Enter your keys using the following format: 

![JupyterLab notebook alpaca api key](images/alpaca_key.PNG)

Rename the text file to '.env'. The file will disappear from the left-hand side menu, however, the file is hidden and still exists. (Note: This is to protect your keys. Never disclose your API keys to a public site and when using GitHub, use environment variables to protect your private API credentials.)

![JupyterLab notebook env file](images/env_file.PNG)

In the financial planner, the Requests library will be used to get the current price of Bitcoin (BTC) and Ethereum (ETH). The endpoint URLs for the respective cryptocurrencies are listed below.  

btc_url = "https://api.alternative.me/v2/ticker/Bitcoin/?convert=USD"

eth_url = "https://api.alternative.me/v2/ticker/Ethereum/?convert=USD"

Using the endpoint URLs, make an API call by typing:

```python
btc_response = requests.get(<insert endpoint URL here>).json()
```
Then use the json dumps function to parse the data by typing:

```python
print(json.dumps(<insert response variable here>, indent=4, sort_keys=True))
```
The resulting data should appear as follows:

![JupyterLab notebook ETH json dump](images/eth_json_dump.PNG)

Next, an API call will be made to Alpaca via the Alpaca SDK to get the current closing prices of the SPDR S&P 500 ETF Trust (ticker: SPY) and of the iShares Core US Aggregate Bond ETF (ticker: AGG).

First, set the variables for the Alpaca API and secret keys. Using the Alpaca SDK, create the Alpaca tradeapi.REST object. In this object, include the parameters for the Alpaca API key, the secret key, and the version number.

```python
alpaca_api_key = os.getenv("ALPACA_API_KEY")
alpaca_secret_key = os.getenv("ALPACA_SECRET_KEY")
alpaca = tradeapi.REST(
    alpaca_api_key,
    alpaca_secret_key,
    api_version="v2")
```

Get the current closing prices for SPY and AGG by using the Alpaca get_barset function.

```python
portfolio_df = alpaca.get_barset(
    tickers,
    timeframe,
    start = start_date,
    end = end_date
).df
```

To confirm the data was imported properly, use the head and/or tail function to review the data:

`display(df.head())`

`display(df.tail())`

![JupyterLab notebook load data](images/collect_data.PNG)

### **Step 2: Evaluate Emergency Savings**
In this section, you’ll use the valuations for the cryptocurrency wallet and for the stock and bond portions of the portfolio to determine if the credit union member has enough savings to build an emergency fund into their financial plan. To do this, complete the following steps:

- Create a Python list named savings_data that has two elements: (1)  total value of the cryptocurrency wallet and (2) total value of the stock and bond portions of the portfolio. Plot a pie chart that visualizes the composition of the member's portfolio.

![JupyterLab notebook portfolio composition](images/pie_chart.PNG)

- Create a variable named emergency_fund_value, and set it equal to three times the value of the member’s monthly_income of $12000.
- Create a series of three if statements to determine if the member’s total portfolio is large enough to fund the emergency portfolio.

```python
if total_portfolio > emergency_fund_value:
    print("Congratulations! You have more than enough money in your emergency fund!")
elif total_portfolio == emergency_fund_value:
    print("Congratulations! You have met your emergency fund goals!")
else:
    funds_needed = emergency_fund_value - total_portfolio
    print(f"You are ${funds_needed: .2f} from reaching your emergency savings goals!")
```
- Run the conditional statement to evaluate the emergency fund needs. 

EMERGENCY FUND ANALYSIS:

Based on this analysis, there is more than enough money in the emergency fund.


### **Step 3: Analyze Retirement Portfolio**

In this phase, you will apply statistical concepts to measure the likelihood of future outcomes and create a Monte Carlo simulation that forecasts the long-term future performance of a retirement portfolio.
 

*(Detailed instructions for calculating and plotting the various results can be found in the **financial_planning_tools.ipynb** file.)*

- Run a Monte Carlo simulation of 500 samples and 30 years for the 60/40 portfolio, and then plot the results.

```python
MC_thirty_year = MCSimulation(
    portfolio_data = mc_portfolio_df,
    weights = [.40, .60],
    num_simulation = 500,
    num_trading_days = 252*30
)
```

- The following image shows the overlay line plot resulting from a simulation with these characteristics. 

![JupyterLab notebook daily returns](images/MC_thirtyyear_sim_plot.PNG)

- Plot the probability distribution of the Monte Carlo simulation. The following image shows the histogram plot resulting from a simulation with these characteristics. 
![JupyterLab notebook cumulative returns](images/MC_thirty_year_dist_plot.PNG)

- Finally, generate summary statistics and using this information, calculate the lower and upper bounds for the expected value of the portfolio with a 95% confidence interval.

- Repeat the steps to analyze different scenarios for the portfolio. In this analysis, the Monte Carlo simulation is re-run for a ten year forecast and a heavier emphasis on stocks of 80%. The following images show the resulting overlay plot and histogram plot.
![JupyterLab notebook box plot S&P 500 and funds](images/MC_tenyear_sim_plot.PNG)
![JupyterLab notebook box plot funds only](images/MC_tenyear_dist_plot.PNG)


RETIREMENT ANALYSIS:

Between the thirty year and ten year scenario, the thirty year portfolio is a better strategy for retirement. The ten year scenario, even with an 80% weighting on stocks, could not produce enough value to support the member's retirement expenses. This is assuming the monthly expenses equal the current monthly income of $12,000.  



### **Quit instructions:**
After saving the file, from the menu bar, navigate to **'File'**, select **'Shutdown'** from the drop-down menu and confirm Shut Down.

![JupyterLab browser selecting shutdown](images/jupyter_shutdown.PNG)
![JupyterLab browser confirming shutdown](images/shutdown_confirm.PNG)

In your open terminal window, deactivate the dev environment by typing:
```python
conda deactivate
```

---

## Contributors

The whale fund analysis was created as part of the Rice Fintech Bootcamp 2022 Program by:

Thuy Nguyen

Email: nguyen_thuyt@yahoo.com

LinkedIn: nguyenthuyt



---

## License

MIT



# Literature Review

Approaches or solutions that have been tried before on similar projects.

**Summary of Each Work**:

- **Source 1**: [Bitcoin Forecasting with Classical Time Series Models on Prices and Volatility]

  - **[[Link](https://arxiv.org/html/2511.06224v1)]()**
  - **Objective**: Evaluate the effectiveness of classical time series models in forecasting Bitcoin prices and volatility, providing a benchmark for comparison with modern machine learning approaches.
  - **Methods**: The study utilized historical Bitcoin price and volatility data from sources such as CoinGecko and Blockchain.com. Classical time series models, including ARIMA (AutoRegressive Integrated Moving Average) for price forecasting and GARCH (Generalized Autoregressive Conditional Heteroskedasticity) for volatility modeling, were applied. Data preprocessing involved handling missing values, normalizing trends, and decomposing time series into components. Models were trained and validated using cross-validation techniques, with performance evaluated using metrics such as Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and Mean Absolute Percentage Error (MAPE).
  - **Outcomes**:  ARIMA models demonstrated moderate accuracy in forecasting Bitcoin prices, particularly for short-term predictions, with an RMSE of approximately 1,200 USD and an R² score of 0.85. The GARCH model effectively captured volatility clustering in Bitcoin prices, achieving a volatility prediction RMSE of 0.045. The study concluded that classical models remain competitive for Bitcoin forecasting, especially when computational resources for deep learning are limited.
  - **Relation to the Project**: It establishes baseline performance for classical models, which can be compared against more advanced techniques (e.g., LSTM, hybrid models) in the project. Understanding the strengths and limitations of ARIMA and GARCH models will help in designing a robust forecasting framework.

- **Source 2**: [Forecasting Bitcoin Price With Neural and
Statistical Models Across Different
Time Granularities]

  - **[[Link](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=11206330)]()**
  - **Objective**: Comparing the performance of neural network models (e.g., LSTM, GRU) and statistical models (e.g., ARIMA, Exponential Smoothing) in forecasting Bitcoin prices across multiple time granularities (hourly, daily, weekly).
  - **Methods**: The study collected Bitcoin price data from CoinMarketCap and Binance APIs, covering hourly, daily, and weekly intervals over a five-year period. Feature engineering included lagged price features, technical indicators (e.g., moving averages, RSI), and external factors (e.g., trading volume). Neural models (LSTM, GRU) were trained using TensorFlow/Keras, while statistical models (ARIMA, Exponential Smoothing) were implemented using Python’s statsmodels library. Models were evaluated using RMSE, MAE, and directional accuracy (percentage of correct up/down predictions).
  - **Outcomes**: Neural models outperformed statistical models across all time granularities, with LSTM achieving the lowest RMSE (850 USD for daily forecasts) and highest directional accuracy (68%). ARIMA performed best for weekly forecasts (RMSE: 1,100 USD), while statistical models struggled with hourly data due to high noise levels. The study highlighted the adaptability of neural models to varying time granularities.
  - **Relation to the Project**: Critical for informing the choice of model architecture and time granularity in the project. If the goal is high-frequency forecasting, neural models may be prioritized, whereas statistical models could suffice for longer-term predictions.


- **Source 3**: [Forecasting Cryptocurrency Prices]

  - **[[Link](https://www.imperial.ac.uk/media/imperial-college/faculty-of-natural-sciences/department-of-mathematics/math-finance/Forecasting_cryptocurrency_prices.pdf)]()**
  - **Objective**: Developing a unified framework for forecasting prices across multiple cryptocurrencies (Bitcoin, Ethereum, Litecoin, etc.) using machine learning techniques.
  - **Methods**: The study aggregated historical price data from CoinGecko for Bitcoin (BTC), Ethereum (ETH), Litecoin (LTC), and Ripple (XRP) over a three-year period. Feature engineering included lagged prices, market capitalization, trading volume, and sentiment scores derived from Twitter and Reddit. Models tested included Random Forest, XGBoost, LSTM, and Prophet. Hyperparameter tuning was performed using grid search, and models were evaluated using RMSE, MAE, and R² metrics.
  - **Outcomes**: XGBoost achieved the best overall performance across cryptocurrencies, with an average RMSE of 250 USD and R² of 0.92. LSTM performed well for Bitcoin and Ethereum but struggled with smaller-cap cryptocurrencies due to limited data. The study found that incorporating sentiment features improved model accuracy by 8–12%, particularly for short-term forecasts.
  - **Relation to the Project**: Aiming to forecast multiple cryptocurrencies or incorporate alternative data sources like sentiment analysis. The findings suggest that ensemble methods (e.g., XGBoost) may outperform deep learning models for multi-cryptocurrency forecasting.
    
- **Source 4**: [FORECASTING BITCOIN PRICES: ARIMA vs LSTM]

  - **[[Link](https://repositorio.iscte-iul.pt/bitstream/10071/19724/1/Master_Joao_Batista_Mendes.pdf)]()**
  - **Objective**: Comparing the performance of ARIMA (a classical statistical model) and LSTM (a deep learning model) in forecasting Bitcoin prices, assessing their strengths and weaknesses.
  - **Methods**: Bitcoin price data from Coinbase was used, spanning five years with daily closing prices. The ARIMA model was implemented with parameters (p, d, q) optimized via auto-ARIMA, while the LSTM model was built using Keras with three layers (input, hidden, output) and trained for 100 epochs. Both models were evaluated on a test set using RMSE, MAE, and directional accuracy. Additionally, the study analyzed forecast horizons (1-day, 7-day, 30-day ahead).
  - **Outcomes**: LSTM outperformed ARIMA across all forecast horizons, with the largest margin observed for 30-day predictions (LSTM RMSE: 1,050 USD vs. ARIMA RMSE: 1,420 USD). ARIMA showed better interpretability and faster training times, but LSTM captured nonlinear trends and volatility spikes more effectively. The study concluded that while ARIMA remains useful for baseline comparisons, LSTM is superior for capturing complex Bitcoin price dynamics.
  - **Relation to the Project**: It empirically demonstrates the trade-offs between classical and deep learning approaches. If the project prioritizes accuracy over interpretability, LSTM or hybrid models may be preferable.
 
- **Source 5**: [Forecasting Cryptocurrency Price Volatility]

  - **[[Link](https://thesis.eur.nl/pub/67785/Thesis__Predicting_Cryptocurrency_Price_Volatility.pdf)]()**
  - **Objective**: Predicting the volatility of cryptocurrency prices, which is critical for risk management and trading strategies, using machine learning and econometric models.
  - **Methods**: The study focused on Bitcoin and Ethereum volatility, using daily price data from 2017–2022. Volatility was measured as the standard deviation of log returns and modeled using GARCH-family models (GARCH, EGARCH, GJR-GARCH) and machine learning models (Random Forest, XGBoost, LSTM). Feature engineering included lagged volatility, trading volume, on-chain metrics (e.g., active addresses), and macroeconomic indicators (e.g., S&P 500 returns). Models were evaluated using RMSE, MAE, and the Diebold-Mariano test for predictive accuracy.
  - **Outcomes**: XGBoost achieved the lowest volatility prediction RMSE (0.032), outperforming GARCH models (RMSE: 0.041) and LSTM (RMSE: 0.038). The study found that incorporating on-chain metrics (e.g., active addresses) reduced prediction error by 15%. GARCH models excelled in capturing volatility clustering but lagged in absolute accuracy compared to machine learning models.
  - **Relation to the Project**: Including volatility forecasting or risk assessment components. The findings suggest that machine learning models, particularly XGBoost, may offer superior volatility predictions when combined with alternative data sources.
 
- **Source 6**: [A data-driven approach to forecasting Bitcoin price using on-chain metrics]

  - **[[Link](https://unitesi.unive.it/retrieve/cfabbce7-876c-4e67-acf2-0ee10c311c8d/Master_Thesis%20VRABII%20SIMION%20and%20FASANO%20GIOVANNI.pdf)]()**
  - **Objective**: To develop a multi-model Bitcoin forecasting and trading system combining supervised, unsupervised, and reinforcement learning methods.
  - **Methods**: Used Linear Regression, SVR, KMeans, GMM, CNN-LSTM, and reinforcement learning agents on daily and hourly Bitcoin/on-chain data.
  - **Outcomes**: Linear Regression and SVR achieved excellent long-term forecasting; KMeans identified buy/sell signals effectively.
  - **Relation to the Project**: It combines long-term forecasting, short-term prediction, and trading signal generation using both market and blockchain metrics.

- **Source 7**: [Machine Learning Approaches for Cryptocurrency Valuation Forecasting]

  - **[[Link](https://ieeexplore.ieee.org/abstract/document/11200830)]()**
  - **Objective**: To improve Bitcoin price prediction accuracy by developing a hybrid machine learning framework that combines LSTM and XGBoost models.
  - **Methods**: The study used 15 years of historical Bitcoin data from Yahoo Finance, applied preprocessing and feature engineering techniques, trained an LSTM model to capture temporal patterns, and combined its extracted features with XGBoost for final price prediction.
  - **Outcomes**: The LSTM-XGBoost hybrid model achieved strong prediction performance with low error rates (MAE: 463.12, RMSE: 837.85) and a high R² score of 0.94, outperforming standalone and traditional models.
  - **Relation to the Project**: The paper is important because it demonstrates how combining LSTM for time-series pattern recognition with XGBoost for nonlinear regression significantly improves Bitcoin price prediction accuracy and robustness.

- **Source 8**: [Comparative Bitcoin Price Prediction Using Multiple
Machine Learning Techniques]

  - **[[Link](https://www.atlantis-press.com/article/126002031.pdf)]()**
  - **Objective**: To improve Bitcoin price prediction by using multiple machine learning and deep learning approaches on historical Bitcoin transaction data.
  - **Methods**: The study used historical Bitcoin price and timestamp data with four machine learning models: Decision Tree (DT), Random Forest, Support Vector Machine (SVM), and Logistic Regression—and evaluated them using prediction horizons and Mean Squared Error (MSE).
  - **Outcomes**: Random Forest and SVM performed best for short-term Bitcoin price prediction, while DT showed stable accuracy for predictions up to six days, whereas Logistic Regression had limited effectiveness.
  - **Relation to the Project**: The paper is most important for a Bitcoin forecasting project because it compares several machine learning models on historical Bitcoin data and identifies Random Forest, SVM, and Decision Tree as effective approaches for short-term Bitcoin price prediction.
 
- **Source 9**: [Improving the Efficiency of Bitcoin Market Price Prediction Using Novel Decision Tree Algorithm and Compare the Prediction Accuracy with K-Nearest Neighbor Algorithm(KNN)]

  - **[[Link](https://ieeexplore.ieee.org/abstract/document/11120001)]()**
  - **Objective**: To compare the performance of a Novel Decision Tree (DT) algorithm and the K-Nearest Neighbor (KNN) algorithm for predicting Bitcoin price trends and software effort estimation using ML techniques.
  - **Methods**: The study used open-source historical Bitcoin datasets with machine learning models (DT and KNN), implemented in Jupyter Notebook, and evaluated the algorithms using prediction accuracy, statistical analysis, and independent sample t-tests.
  - **Outcomes**: The DT algorithm achieved slightly higher accuracy (87.80%) than KNN (86.91%), although the statistical difference between the models was not significant (p > 0.05).
  - **Relation to the Project**: The paper is important because it compares ML algorithms on historical Bitcoin data and shows that the Decision Tree model achieved slightly better prediction accuracy than KNN for forecasting Bitcoin price trends.

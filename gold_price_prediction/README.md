# 📉 Gold Price Prediction with Machine Learning

## 📌 Project Overview
This project explores **Machine Learning** to predict the directional movement of Gold prices (`XAU/USD`) for two distinct time horizons: **Next Day (1-Day)** and **Next Week (5-Days)**.

Using historical data from **Yahoo Finance (2016-2026)**, we engineered technical and intermarket features to train **Random Forest** and **XGBoost** classifiers. The goal was to build a strategy that prioritizes **High Precision** over trade frequency.

---

## 🧠 Technologies Used
* Yahoo Finance API (via `yfinance`)
* Matplotlib (Data visualization)
* Pandas, NumPy, Scikit-learn
* Models: Random Forest Classifier, XGBoost Classifier

---

## 📊 Methodology

### 1. Data & Feature Engineering
We avoided the common pitfall of using "Raw Prices" (which confuses ML models when prices hit new all-time highs). Instead, we focused on **Stationary Features**:
* **Momentum:** RSI, MACD (Normalized by price).
* **Trend:** Slope of SMA (12, 60, 150 days), Distance from SMA (%).
* **Volatility:** Bollinger Band Width.
* **Intermarket Correlations:**
    * **Oil (Crude):** Inflation expectations.
    * **DXY (Dollar Index):** Currency strength (Inverse correlation).
    * **TNX (10-Year Yield):** Interest rate pressure.

### 2. Model Strategy
We developed two distinct strategies based on model performance:

| Horizon | Model Architecture | Logic |
| :--- | :--- | :--- |
| **1-Day** | **Ensemble (RF + XGB)** | Daily noise is high. We average the probabilities of both models. Trade only if Avg Confidence > 55%. |
| **1-Week** | **Random Forest (Solo)** | XGBoost struggled with weekly noise. Random Forest showed **88% Precision** on "UP" moves during training. |

---

## 📈 Performance (Backtest vs. Reality)

### Training Phase (2016 - 2024)
* **Precision (Up Moves):** ~60-70%

### Live Simulation (Last 30 Days - Unseen Data)
*Market Condition: Extreme Volatility & Geopolitical Fear*

| Model | Trades Taken | Wins | Win Rate | Verdict |
| :--- | :--- | :--- | :--- | :--- |
| **1-Day** | 25 | 9 | **36%** | Struggled with rapid intraday reversals. |
| **1-Week** | 20 | 8 | **40%** | Failed to capture the "Fear Premium" driving prices. |

> **Note:** The lower performance in the test set highlights the difficulty of purely technical models during high-fear geopolitical events.

---

## 🛠️ How to Run
To run this project locally, follow these steps on Terminal:

* Clone the repository: `git clone https://github.com/romannguyen99/machine_learning.git`
* Install the required libraries:
  * If you're using Google Colab, you don't need to pip install. Just follow the importing the dependencies section.
    * Launch Google Colab: https://colab.research.google.com/
    * Open the Gold_Price_Prediction.ipynb file and run the notebook cells sequentially.
  * Install dependencies: `pip install pandas numpy yfinance scikit-learn xgboost matplotlib`
* Open the Notebook: `jupyter notebook Gold_price_Predictions.ipynb`

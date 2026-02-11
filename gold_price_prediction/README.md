# 📉 Gold Price Prediction with Machine Learning

## 📌 Project Overview
This project explores the application of **Machine Learning (ML)** to predict the directional movement of Gold prices (`XAU/USD`) for two distinct time horizons: **Next Day (1-Day)** and **Next Week (5-Days)**.

Using historical data from **Yahoo Finance (2016-2024)**, we engineered technical and intermarket features to train **Random Forest** and **XGBoost** classifiers. The goal was to move beyond simple accuracy and build a "Sniper" strategy that prioritizes **High Precision** (Win Rate) over trade frequency.

---

## 🛠️ Tech Stack
* **Language:** Python 3.10+
* **Libraries:** `pandas`, `numpy`, `yfinance`, `scikit-learn`, `xgboost`, `matplotlib`
* **Models:** Random Forest Classifier, XGBoost Classifier

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

| Horizon | Strategy Name | Model Architecture | Logic |
| :--- | :--- | :--- | :--- |
| **1-Day** | *The Consensus* | **Ensemble (RF + XGB)** | Daily noise is high. We average the probabilities of both models. Trade only if Avg Confidence > 55%. |
| **1-Week** | *The Sniper* | **Random Forest (Solo)** | XGBoost struggled with weekly noise. Random Forest showed **88% Precision** on "UP" moves during training. |

---

## 📈 Performance (Backtest vs. Reality)

### Training Phase (2016 - 2023)
* **Precision (Up Moves):** ~60-70%
* **Behavior:** The models successfully identified "Mean Reversion" setups (buying when Gold was oversold in a trend).

### Live Simulation (Last 30 Days - Unseen Data)
*Market Condition: Extreme Volatility & Geopolitical Fear*

| Model | Trades Taken | Wins | Win Rate | Verdict |
| :--- | :--- | :--- | :--- | :--- |
| **1-Day** | 25 | 9 | **36%** | Struggled with rapid intraday reversals. |
| **1-Week** | 20 | 8 | **40%** | Failed to capture the "Fear Premium" driving prices. |

> **Note:** The lower performance in the test set highlights the difficulty of purely technical models during "Black Swan" or high-fear geopolitical events.

---

## 🧠 Key Learnings & Challenges

1.  **The "Raw Price" Trap:** Initial models failed because they couldn't handle Gold breaking $2,500 (unseen territory). Normalizing features to *Percentages* and *Ratios* fixed this.
2.  **Regime Changes:** The model was trained on a "Normal" market. The last 30 days represented a "Fear" market. Technical indicators (like RSI > 70) signaled "Sell," but fear kept pushing prices higher.
3.  **The Human Element:** Gold is heavily driven by sentiment. A pure numerical model cannot see news about war or banking instability.

---

## 🚀 Future Improvements
To bridge the gap between 40% and 60% win rate, the next version (v2.0) will include:

1.  **Sentiment Analysis (NLP):** Scrape financial news (Bloomberg/Twitter) to create a `Fear_Index`. If Fear is high, ignore "Sell" signals.
2.  **Economic Calendar Filter:** Avoid trading on FOMC (Interest Rate Decision) days, where technicals often fail.
3.  **Deep Learning (LSTM):** Experiment with Long Short-Term Memory networks to capture time-series sequences better than Tree-based models.

---

## 📂 Project Structure

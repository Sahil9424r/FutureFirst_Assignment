# Stock Price Prediction

---

## 1. Overall Approach and Assumptions

### Overall Approach
The objective of this assignment is to build a **Python-based machine learning model** that predicts **next-day stock price behavior** using a provided **Data dataset**. The project strictly follows the assignment constraints by modeling **only the relationship between the provided datasets**, without using any external or macroeconomic factors.

The overall workflow consists of the following stages:
1. Data loading and inspection  
2. Data preprocessing  
3. Feature engineering based on day-over-day changes  
4. Target variable formulation  
5. Model training using multiple algorithms  
6. Model evaluation and analysis  

Each stage is implemented explicitly and evaluated independently.

---

### Core Assumptions
1. **Primary Influence**  
   The next day’s stock price is primarily influenced by the **change in the Data dataset from the previous day**.

2. **Scope Limitation**  
   Although real-world stock prices are influenced by many external factors (news, sentiment, macroeconomic indicators), **all such factors are intentionally ignored** for this assignment.  
   Only the relationship between the provided datasets is modeled.

---

## 2. Data Preprocessing and Feature Engineering

### Data Preprocessing

The following preprocessing steps were performed to ensure clean and meaningful input for the models:

#### Date Handling
- Converted `Date` columns to datetime format  
- Sorted records chronologically to preserve time dependency  
**Why:** Time order is critical for next-day prediction tasks.

#### Dataset Merging
- Merged the Data dataset and StockPrice dataset using the `Date` column  
**Why:** Ensures correct alignment between independent and dependent variables.

#### Missing Value Handling
- Missing values introduced by differencing, lagging, and rolling operations were removed  
**Why:** Machine learning models cannot train on incomplete observations.

#### Outlier Handling
- Outliers were handled using the **Interquartile Range (IQR) method**  
**Why:** Financial data often contains extreme spikes. IQR is robust and does not assume normal distribution.

---

### Feature Engineering

To explicitly model how **previous-day changes influence next-day stock prices**, the following features were engineered:

#### Day-over-Day Change
- **Change_in_Data**
**Why:** The assignment states that stock price movement depends on the *change* in data, not its absolute value.

#### Lag Features
- **Change_Lag_1** – previous day’s change  
- **Change_Lag_2** – change from two days prior  
**Why:** Captures short-term memory and momentum effects.

#### Rolling Statistics
- **Change_Mean_3** – rolling mean over 3 days (short-term trend)  
- **Change_Std_3** – rolling standard deviation over 3 days (recent volatility)  
**Why:** Rolling statistics smooth noise and capture trend and volatility.

---

## 3. Model Selection, Training Methodology, and Evaluation

### Target Variable Design

Initial experiments attempted to predict the **absolute next-day stock price**, which resulted in poor performance due to high variance and weak correlation.

To better align with the assignment assumption that **changes drive price movement**, the target variable was reformulated as:


**Why this change was necessary:**
- Aligns change-based features with a change-based target  
- Reduces variance in the target variable  
- Improves numerical stability  
- Still satisfies the requirement to analyze next-day stock price behavior  

---

### Model Selection and Justification

#### Linear Regression
- Baseline model  
- Simple and interpretable  
- Helps understand direct feature influence  

#### Random Forest Regressor
- Captures non-linear relationships  
- Robust to noise and outliers  

#### Gradient Boosting Regressor
- Sequentially improves predictions  
- Best-performing model among those tested  

---

### Training Methodology
- Data split into **80% training** and **20% testing**  
- `shuffle=False` used to preserve time order and prevent data leakage  
- Feature scaling applied using `StandardScaler`  
- A common evaluation function ensured consistent comparison across models  

---

### Evaluation Metrics
- **MAE (Mean Absolute Error)**  
- **RMSE (Root Mean Squared Error)**  
- **R² Score**  

---

## 4. Key Insights, Observations, and Conclusions

### Why R² is Negative
Despite extensive preprocessing and feature engineering, the R² score remains negative. This is **not a modeling error** but a limitation of the dataset.

**Reasons:**
- Daily stock prices are highly volatile  
- Limited predictive signal in the Data dataset  
- External factors are intentionally excluded  
- Feature engineering improves stability but cannot create new information  

A negative R² indicates that previous-day changes alone are insufficient to explain next-day price variance.

---

### Key Insights
- Predicting **price change** is more effective than predicting absolute price  
- Ensemble models reduce error compared to linear regression  
- Feature engineering improves stability but not variance explanation  

---

### Limitations and Challenges
- Limited feature set  
- High noise and volatility  
- Negative R² despite tuning  
- No external data allowed  

---

### Conclusion
This project demonstrates a complete machine learning workflow under constrained assumptions. While day-over-day changes influence stock price movement to some extent, the relationship is weak and noisy. The results highlight both the usefulness and limitations of modeling stock prices using a single explanatory dataset.

---

## 5. Repository Contents
- Complete Jupyter Notebook (`.ipynb`)  
- Reproducible source code  
- This README file  

---


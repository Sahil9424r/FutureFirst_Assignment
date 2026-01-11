# Machine Learning Assignment – Stock Price Prediction

---

## 1. Overall Approach and Assumptions

### Overall Approach
This project is developed as part of a **Machine Learning assignment on Stock Price Prediction**.  
The objective is to build a **Python-based machine learning model** that predicts **next-day stock price behavior** using only the information provided in two datasets:

1. **Data Dataset** – Independent variable(s)  
2. **StockPrice Dataset** – Dependent variable representing stock prices  

The solution strictly follows the assignment constraints and **models only the relationship between the provided datasets**, without using any external or macroeconomic factors.

The workflow is structured as:
1. Data loading and inspection  
2. Data preprocessing  
3. Feature engineering  
4. Target variable formulation  
5. Model training and hyperparameter tuning  
6. Model evaluation and analysis  

Each stage is implemented explicitly and evaluated independently.

---

### Core Assumptions
1. **Primary Influence**  
   The next day’s stock price is primarily influenced by the **change in the Data dataset from the previous day**.

2. **Scope Limitation**  
   External influences such as market news, sentiment, volume, and macroeconomic indicators are intentionally ignored.  
   Only the relationship between the provided datasets is modeled.

---

## 2. Data Preprocessing and Feature Engineering

### Data Preprocessing

The following preprocessing steps were applied:

#### Date Handling
- Converted `Date` columns to datetime format  
- Sorted records chronologically  
**Reason:** Time order is critical for next-day prediction.

#### Dataset Merging
- Merged Data and StockPrice datasets using the `Date` column  
**Reason:** Ensures correct alignment of independent and dependent variables.

#### Missing Value Handling
- Rows with missing values created by differencing, lagging, and rolling operations were removed  
**Reason:** ML models cannot train on incomplete observations.

#### Outlier Handling
- Outliers were handled using the **Interquartile Range (IQR) method**  
**Reason:** Financial data contains extreme values; IQR is robust and distribution-agnostic.

---

### Feature Engineering

To explicitly model day-over-day influence, the following features were engineered:

#### Day-over-Day Change
**Reason:** The assignment states that stock price behavior depends on *changes* in the data.

#### Lag Features
- `Change_Lag_1` – previous day’s change  
- `Change_Lag_2` – change from two days ago  
**Reason:** Captures short-term dependency and momentum.

#### Rolling Statistics
- `Change_Mean_3` – rolling mean over 3 days (trend)  
- `Change_Std_3` – rolling standard deviation over 3 days (volatility)  
**Reason:** Helps smooth noise and capture short-term dynamics.

---

## 3. Target Variable Design

Initial experiments attempted to predict the **absolute next-day stock price**, which resulted in weak performance due to high variance.

To better align with the assumption that **changes drive price movement**, the target variable was reformulated as:


**Benefits of this formulation:**
- Aligns change-based features with a change-based target  
- Reduces target variance  
- Improves numerical stability  
- Still satisfies the assignment objective  

---

## 4. Model Selection and Hyperparameter Tuning

### Models Used
- **Linear Regression** – baseline model  
- **Random Forest Regressor** – non-linear ensemble model  
- **Gradient Boosting Regressor** – boosting-based ensemble model  

These models allow comparison between linear and non-linear approaches.

---

### Hyperparameter Tuning (RandomizedSearchCV)

To improve model performance, **RandomizedSearchCV** was applied to ensemble models.

#### Why RandomizedSearchCV?
- Efficient compared to GridSearchCV  
- Explores a wide hyperparameter space  
- Reduces computational cost  

Tuned parameters included:
- Number of estimators  
- Tree depth  
- Minimum samples per split/leaf  
- Learning rate (for boosting models)

Despite tuning, improvements were limited by the **inherent lack of predictive signal** in the data.

---

## 5. Training Methodology

- Dataset split into **80% training** and **20% testing**
- `shuffle=False` used to preserve time order and avoid data leakage
- Feature scaling applied using `StandardScaler`
- A common evaluation function used for all models

---

## 6. Evaluation Metrics

Models were evaluated using:
- **MAE (Mean Absolute Error)**
- **RMSE (Root Mean Squared Error)**
- **R² Score**

---

## 7. Results and Analysis

### Why R² Remains Negative (Even After Tuning)

Despite extensive preprocessing, feature engineering, and hyperparameter tuning, the R² score remains negative.

This occurs because:
- Daily stock prices are highly volatile and resemble a random walk  
- The provided Data dataset contains limited explanatory signal  
- External influencing factors are intentionally excluded  
- Hyperparameter tuning optimizes model behavior but **cannot create new information**

A negative R² indicates that **previous-day changes alone are insufficient to explain next-day stock price variance**, which is a realistic outcome for financial time-series data.

---

## 8. Key Insights and Observations

- Feature engineering improves stability but not variance explanation  
- Ensemble models reduce error compared to linear regression  
- Hyperparameter tuning yields marginal improvement  
- Data limitations dominate model performance  

---

## 9. Limitations and Challenges

- Restricted to a single explanatory dataset  
- High noise and volatility in stock prices  
- Negative R² despite advanced models and tuning  
- No external data allowed by assignment  

These limitations reflect real-world challenges in financial modeling.

---

## 10. Conclusion

This project demonstrates a complete machine learning workflow under constrained assumptions. While day-over-day changes in the Data dataset influence stock price movement to a limited extent, the relationship is weak and noisy. Even with hyperparameter tuning, model performance remains constrained by data limitations rather than algorithm choice. The results highlight both the usefulness and the inherent limitations of predicting stock prices using a single explanatory data source.

---

## 11. Repository Contents
- Complete Jupyter Notebook (`.ipynb`)
- Source code required to reproduce results
- This README file

---


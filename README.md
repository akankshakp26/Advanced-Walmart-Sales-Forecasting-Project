# 📈 Walmart Retail Intelligence System

A **Streamlit web app** that forecasts Walmart weekly sales using an XGBoost regression model. Features a dark-themed analytics dashboard with interactive charts, per-store performance analysis, and a live prediction interface all powered by a trained ML model.

---

## 🖥️ App Preview
<img width="1915" height="863" alt="Screenshot 2026-05-29 121635" src="https://github.com/user-attachments/assets/c58081d7-8f1c-4a83-aa9d-6f8d0cffe1a1" />
<img width="1918" height="865" alt="Screenshot 2026-05-29 121832" src="https://github.com/user-attachments/assets/638cbaf7-6ca0-47c7-94e7-1a25e211245b" />
<img width="1918" height="871" alt="Screenshot 2026-05-29 121853" src="https://github.com/user-attachments/assets/918b7d51-0a3a-4a62-ba32-410eff184631" />
<img width="1919" height="863" alt="Screenshot 2026-05-29 121915" src="https://github.com/user-attachments/assets/33c4bf2f-8256-4050-a752-3ba5fba3125b" />

---

## 🚀 Pages & Features

| Page | Description |
|------|-------------|
| **Dashboard** | KPI cards (total sales, avg weekly sales, store count, holiday weeks) + sales trend line chart + average sales by store bar chart |
| **Sales Prediction** | Input sliders for store, date, temperature, fuel price, CPI, unemployment → XGBoost predicts weekly sales + gauge chart |
| **Store Analytics** | Per-store weekly sales trend + temperature vs sales scatter plot |
| **Data Insights** | Feature correlation heatmap, dataset preview, statistical summary |

---

## 📁 Project Structure

```
walmart/
├── app.py                  # Streamlit app — all 4 pages & UI logic
├── train.py                # ML training script — feature engineering + XGBoost
├── walmart.csv             # Dataset (6,434 records across 45 Walmart stores)
├── walmart_model.pkl       # Serialised trained XGBRegressor
└── scaler.pkl              # Fitted StandardScaler for inference
```

---

## 📊 Dataset

**`walmart.csv`** — 6,434 rows × 8 columns

| Feature | Type | Description |
|---------|------|-------------|
| `Store` | Integer | Store number (1–45) |
| `Date` | Date | Week start date (`dd-mm-yyyy`) |
| `Weekly_Sales` | Float | **Target** — weekly revenue in USD |
| `Holiday_Flag` | Binary (0/1) | Whether the week includes a public holiday |
| `Temperature` | Float | Average temperature (°F) for the region |
| `Fuel_Price` | Float | Regional fuel price (USD/gallon) |
| `CPI` | Float | Consumer Price Index |
| `Unemployment` | Float | Regional unemployment rate (%) |

---

## 🤖 ML Pipeline

### Feature Engineering (`train.py`)
Date column is parsed and expanded into:

| Engineered Feature | Description |
|---|---|
| `Day`, `Month`, `Year` | Calendar components |
| `Week`, `DayOfWeek` | ISO week number and weekday |
| `Month_sin`, `Month_cos` | Cyclical encoding of month |
| `Week_sin`, `Week_cos` | Cyclical encoding of week |

Cyclical encoding ensures the model understands that week 52 and week 1 are temporally close.

### Model
```python
XGBRegressor(
    n_estimators     = 500,
    learning_rate    = 0.05,
    max_depth        = 8,
    subsample        = 0.8,
    colsample_bytree = 0.8,
    random_state     = 42
)
```

### Evaluation Metrics
| Metric | Description |
|--------|-------------|
| **MAE** | Mean Absolute Error |
| **RMSE** | Root Mean Squared Error |
| **R²** | Coefficient of Determination |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Web App | Streamlit |
| ML Model | XGBoost (`XGBRegressor`) |
| Data Processing | Pandas, NumPy |
| Preprocessing | scikit-learn (`StandardScaler`, `train_test_split`) |
| Visualisation | Plotly Express, Plotly Graph Objects |
| Serialisation | Python `pickle` |
| Python Version | 3.8+ |

---

## ⚙️ Getting Started

### Installation
```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/walmart-retail-intelligence.git
cd walmart-retail-intelligence

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install streamlit xgboost scikit-learn pandas numpy plotly
```

### Train the Model
```bash
python train.py
```
This generates `walmart_model.pkl` and `scaler.pkl`.

### Run the App
```bash
streamlit run app.py
```
Open `http://localhost:8501` in your browser.

---

## 📌 Suggested Improvements

- **`requirements.txt`** — add one to pin dependency versions for reproducibility
- **Model evaluation page** — display MAE, RMSE, R² inside the app itself
- **Upload your own CSV** — let users forecast on custom datasets
- **Date range filter** — add date filtering across all dashboard charts
- **Deploy** — host on [Streamlit Community Cloud](https://streamlit.io/cloud) for a free live demo link

---


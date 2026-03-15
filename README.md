This R-based project investigates the key determinants of used car resale prices through seven incremental linear regression models, progressively adding age, mileage, fuel type, transmission, seller type, brand, and model. Log transformation is applied to both price and km_driven to handle skewness. The final model (Model 7) achieves R²=0.8845. A Random Forest comparison (ntree=200) achieves R²=0.877, and K-means clustering (k=4) identifies four distinct market segments: Budget, Value, Premium, and Prestige/Outlier.

## 📒 Notebook Sections

---

### 1️⃣ 🚗 Business Problem
> Solving price uncertainty in a competitive, information-asymmetric used car market

- 🤷 **The Problem** — Buyers & sellers struggle to determine fair resale value due to
  multiple interacting factors
- 🎯 **The Goal** — Build stepwise regression models revealing exactly how each factor
  (age, mileage, brand, fuel) drives price
- 💼 **Who Benefits** — Dealerships, resellers, auto platforms, and individual buyers

---

### 2️⃣ 📦 Data Loading & Preparation
> Vehicle resale dataset sourced from a car trading platform

| Detail | Value |
|---|---|
| 🎯 Target Variable | `selling_price` (log-transformed) |
| 📋 Key Variables | `year`, `km_driven`, `fuel`, `transmission`, `seller_type`, `brand`, `model` |
| 🔧 Prep Steps | Removed duplicates, standardised column names, encoded categoricals |

**Transformations Applied:**
- 💰 `price` → `log(price)` to fix right-skew & heteroscedasticity
- 🛣️ `km_driven` → `log(km_driven + 1)` for linear usage representation
- 📅 `year` → `age = current_year - year` to capture depreciation

---

### 3️⃣ 🔍 Exploratory Data Analysis (EDA)

| Figure | What It Shows | Key Insight |
|---|---|---|
| 📊 Fig 1 | Price histogram | Right-skewed → justifies log(price) |
| 🏷️ Fig 2 | Brand vs price barplot | Toyota/BMW retain value; Datsun depreciates fast |
| ⛽ Fig 3 | Fuel type vs price | Diesel commands higher medians |
| 📉 Fig 4 | Age vs price scatter | Strong negative concave curve — rapid early depreciation |
| 🔄 Fig 5 | Age vs price by transmission | Automatics fetch higher prices at every age |
| 🔥 Fig 6 | Correlation heatmap | Age strongest negative predictor; km moderate |
| 🔢 Fig 7 | GGpairs plot | Integrated view of price, age & km relationships |

---

### 4️⃣ 📈 7 Stepwise Regression Models
> Each model adds one new variable — watching R² grow step by step

| Model | Variables Added | R² | Key Finding |
|---|---|---|---|
| 1️⃣ Model 1 | `age` | 0.2325 | Each extra year → ~10% price drop |
| 2️⃣ Model 2 | + `log(km_driven)` | ~0.28 | Mileage adds explanatory power |
| 3️⃣ Model 3 | + `fuel_type` | 0.406 | Diesel → +110% premium 🔥 |
| 4️⃣ Model 4 | + `transmission` | 0.593 | Manual → −50% vs automatic 🔥 |
| 5️⃣ Model 5 | + `seller_type` | 0.603 | Individual seller → −13% vs dealer |
| 6️⃣ Model 6 | + `brand` | 0.796 | Brand equity is a massive driver 🔥 |
| 7️⃣ **Model 7** | + `model` | **0.8845** | ⭐ Best — 88% variance explained |

> 💡 **Biggest jump:** Adding `brand` (Model 5→6) boosted R² by **+0.193** — the single largest gain!

---

### 5️⃣ 🩺 Diagnostic Tests
> Validating that regression assumptions hold

| Diagnostic Plot | What It Tests | Result |
|---|---|---|
| 📉 Residuals vs Fitted | Linearity + homoscedasticity | ✅ Random scatter around zero |
| 📐 Q-Q Plot | Normality of residuals | ✅ Points follow 45° line |
| 📊 Scale-Location | Constant variance | ✅ Even spread, no funnel |
| 🔎 Residuals vs Leverage | Influential outliers | ✅ Within Cook's distance |

---

### 6️⃣ 🌲 Random Forest Comparison
> Non-linear benchmark against the best linear model (Model 7)

| Model | RMSE | MAE | R² |
|---|---|---|---|
| 📏 Linear Regression (Model 7) | 245,206 | 123,052 | 0.8349 |
| 🌲 **Random Forest** | **211,980** | **110,345** | **0.8766** ⭐ |

> `ntree = 200` | Sampled up to 15,000 rows for computational efficiency

**🏆 Variable Importance (Random Forest):**
- 🥇 `age` — Most influential (economic depreciation confirmed)
- 🥈 `transmission` — Automatic premium is real
- 🥉 `brand` — Reputation drives resale value
- 4️⃣ `fuel_type` + `km_driven` — Moderate but meaningful

---

### 7️⃣ 🗂️ K-Means Clustering
> Unsupervised market segmentation — 4 clusters via Elbow Method

| Cluster | Profile | Market Segment |
|---|---|---|
| 🟤 Cluster 1 | Old · High km · Low price | 💰 Budget / Economy |
| 🟡 Cluster 2 | Mid-age · Moderate price | 🤝 Value-for-Money |
| 🟢 Cluster 3 | New · Low km · High price | ✨ Premium / Low-Mileage |
| 🔵 Cluster 4 | Mixed age/km · Variable price | 🏆 Prestige / Outlier |

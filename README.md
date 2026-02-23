# Crypto Limit Order Book Analysis

Predicting short-term Bitcoin price movements using high-frequency limit order book data from Coinbase.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

## Overview

This project analyzes 12 days of Bitcoin limit order book (LOB) data at 1-second frequency to:
1. Understand market microstructure dynamics
2. Engineer predictive features from order book depth and flow
3. Build ML models to predict 1-minute price direction

**Key Result:** Achieved 48.3% accuracy predicting price direction (Up/Down/Flat), beating random baseline (33%) by 15 percentage points using XGBoost with engineered LOB features.

## Dataset

- **Source:** [Kaggle - High Frequency Crypto LOB Data](https://www.kaggle.com/datasets/martinsn/high-frequency-crypto-limit-order-book-data)
- **Asset:** Bitcoin (BTC/USD) on Coinbase
- **Period:** April 7-19, 2021
- **Frequency:** 1-second snapshots
- **Size:** 1,030,728 rows × 156 columns
- **Features:** 15 levels of bid/ask depth, limit/market/cancel order breakdown

## Project Structure
```
crypto-lob-analysis/
├── notebooks/
│   ├── 01_eda_and_features.ipynb    # Data exploration & feature engineering
│   ├── 02_predictive_modeling.ipynb  # ML model training & evaluation
│   └── 03_visualization.ipynb        # Interactive visualizations
├── data/
│   └── (download from Kaggle)
├── README.md
└── requirements.txt
```

## Key Findings

### 1. Order Book Imbalance Predicts Price Direction

Order Flow Imbalance (OFI) shows 0.18 correlation with 10-second future price change — statistically significant for financial data.

![OFI Correlation](outputs/figures/ofi_correlation.png)

### 2. Market Microstructure Insights

- **Spread:** Median spread is just $0.01 (0.22 bps) — highly liquid market
- **Depth:** 77-82K USD average depth at best bid/ask, dropping sharply at deeper levels
- **Cancel Rate:** 43% cancel-to-limit ratio indicates active quote management
- **Asymmetry:** Persistent sell-side pressure throughout the dataset

### 3. Model Performance

| Model | Accuracy | vs Random |
|-------|----------|-----------|
| Logistic Regression | 42.8% | +9.8% |
| Random Forest | 45.3% | +12.3% |
| XGBoost | 48.3% | +15.3% |

### 4. Most Important Features

| Rank | Feature | Importance |
|------|---------|------------|
| 1 | volatility_5min | 21.7% |
| 2 | volatility_1min | 14.9% |
| 3 | spread_bps | 6.5% |
| 4 | depth_imbalance_weighted | 6.1% |
| 5 | momentum_1min | 4.2% |

Volatility features dominate — the model first identifies "when" prices move, then uses imbalance/momentum for direction.

## Feature Engineering

Created 60 features from raw LOB data:

| Category | Count | Examples |
|----------|-------|----------|
| Depth Imbalance | 11 | Level-specific, aggregated, weighted imbalance |
| Order Flow | 14 | Limit/cancel flows, market order imbalance |
| Spread & Volatility | 18 | Spread in bps, realized volatility, momentum |
| OFI Features | 10 | Raw OFI, rolling averages, lags |

## Installation
```bash
# Clone repository
git clone https://github.com/yourusername/crypto-lob-analysis.git
cd crypto-lob-analysis

# Install dependencies
pip install -r requirements.txt

# Download data from Kaggle and place in data/ folder
```

## Requirements
```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=0.24.0
xgboost>=1.4.0
plotly>=5.0.0
```

## Usage

1. **EDA & Feature Engineering:**
```
   Open notebooks/01_eda_and_features.ipynb
```

2. **Model Training:**
```
   Open notebooks/02_predictive_modeling.ipynb
```

3. **Visualizations:**
```
   Open notebooks/03_visualization.ipynb
```

## Limitations & Future Work

**Limitations:**
- Test period includes April 2021 crash — results may not generalize to calm markets
- No transaction costs or market impact modeling
- 1-minute horizon may be too long for HFT signals

**Future Improvements:**
- Shorter prediction horizons (10s, 30s)
- Binary classification (Up vs Down only)
- LSTM/Transformer models for sequence patterns
- Backtest as trading strategy with realistic costs
- Cross-asset analysis (ETH, ADA)

## Author

**Rishabh Gupta**
- LinkedIn: [your-linkedin]
- Email: [your-email]

## License

MIT License - feel free to use for learning and research.

## Acknowledgments

- Dataset: [Martin Sn on Kaggle](https://www.kaggle.com/datasets/martinsn/high-frequency-crypto-limit-order-book-data)
- Inspired by academic research on LOB price prediction

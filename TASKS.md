## ✅ Phase 1: Data Engineering & Preprocessing

- [x] **Task:** Establish Project Architecture.
    - *Challenge:* ensuring reproducibility across different environments.
    - *Solution:* Created `00_setup.py` to auto-generate folder trees and robust `requirements.txt`.
- [x] **Task:** Fetch Historical Market Data.
    - *Challenge:* `yfinance` library update caused `KeyError: 'Adj Close'`.
    - *Solution:* Implemented a robust column checker that dynamically looks for `'Adj Close'` or `'Close'` and forces `auto_adjust=False`.
- [x] **Task:** Handle Data Anomalies (Negative Oil Prices).
    - *Challenge:* April 2020 Crude Oil prices went negative, causing `NaN` in Log Return calculations.
    - *Solution:* Implemented a filter `df[df > 0].dropna()` to remove days where pricing logic broke down.

## ✅ Phase 2: Mathematical Modeling (Stochastic Simulation)

- [x] **Task:** Calibrate MV-GBM Model.
    - *Challenge:* Ensuring simulated assets move together (Correlation).
    - *Solution:* Used **Cholesky Decomposition** on the Covariance Matrix to generate correlated random shocks.
- [x] **Task:** Validate "Brown" vs. "Green" correlations.
    - *Result:* Confirmed high correlation (0.50) between Oil and Energy Sector (XLE), validating the proxy choice.
- [x] **Task:** Implement Stress Scenarios.
    - *Method:* Modeled a "Disorderly Transition" via drift shocks (Brown -30%, Green +10%) and a Volatility Multiplier (1.5x).

## ✅ Phase 3: Liquidity Friction Implementation

- [x] **Task:** Model Liquidity Cost as a function of Volatility.
    - *Challenge:* Constant transaction costs do not reflect crisis reality.
    - *Solution:* Implemented a **Quadratic Liquidity Penalty** where cost increases with the square of volatility ($L \propto \sigma^2$).
- [x] **Task:** Quantify the "Liquidity Gap."
    - *Result:* The liquidity haircut doubled from 2.5% (Base) to 5.0% (Stress), proving that liquidity risks amplify solvency risks.

## 📈 Summary of Achievements

| Metric | Before Optimization | Final Result |
| :--- | :--- | :--- |
| **Data Robustness** | Prone to `NaN` (Neg Prices) | 100% Clean Log-Returns |
| **Model Complexity** | Single Asset / No Correlation | Multi-Asset Cholesky Simulation |
| **Risk Measure** | Standard VaR (Frictionless) | **Liquidity-Adjusted VaR** |
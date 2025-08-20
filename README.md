# OrderFlowImbalance
# 📊 Order Flow Imbalance Analysis for AAPL Stock

This project analyzes **high-frequency order book data** for Apple (AAPL) stock to compute **Order Flow Imbalance (OFI)** metrics, aggregate them into 1-minute intervals, and derive a **PCA-weighted Integrated OFI metric**.  

The analysis covers **5,000 order book events from October 21, 2024** (~1 hour and 10 minutes).  
It generates three OFI metrics—**Best-Level OFI, Multi-Level OFI, and Integrated OFI**—and visualizes their temporal trends to uncover intraday market dynamics.  

Additionally, a conceptual framework for **cross-asset OFI impact analysis using LASSO regression** is provided.


 

---

## ✨ Features
- **Data Processing**: Loads and preprocesses high-frequency order book data with 10 bid/ask levels.  
- **OFI Calculation**: Per-event OFI at each level using a conditional price-based method.  
- **Time-Series Aggregation**: 1-minute intervals for:
  - *Best-Level OFI* (top of book)  
  - *Multi-Level OFI* (sum across 10 levels)  
  - *Integrated OFI* (PCA-weighted OFI)  
- **Visualization**: Time-series comparison of OFI metrics.  
- **Cross-Asset Concept**: Outlines a LASSO regression framework for cross-asset impact (e.g., AAPL → MSFT).  

---

## 📊 Dataset
- **File**: `first_25000_rows.csv` (not included due to size; available on request).  
- **Description**: 5,000 AAPL order book events (Oct 21, 2024, 11:54:29–13:04:20 UTC).  
- **Columns**:  
  - `ts_event` (timestamp)  
  - `bid_px_00` to `bid_px_09`, `ask_px_00` to `ask_px_09`  
  - `bid_sz_00` to `bid_sz_09`, `ask_sz_00` to `ask_sz_09`  

---

## ⚙️ Methodology

### Preprocessing
- Convert `ts_event` to UTC datetime.  
- Ensure data is sorted by timestamp (sequence-based sort available but commented out).  

### OFI Calculation
For each level (0–9):  
- **Bid flow**:  
  - Current bid size if price ↑  
  - `-previous size` if price ↓  
  - Size difference if price unchanged  
- **Ask flow**: Reverse logic of bid flow.  
- **Event-level OFI**: `delta_OFI_m = OF_b_m - OF_a_m`  

### Aggregation
- Group into **1-minute buckets**.  
- Compute:  
  - `ofi_level_0` → Best-Level OFI  
  - `sum(ofi_level_0…ofi_level_9)` → Multi-Level OFI  

### PCA for Integrated OFI
- Standardize 10-level OFI matrix (`StandardScaler`).  
- Apply PCA → extract 1st component.  
- Normalize weights (L1 norm).  
- Compute Integrated OFI = weighted sum.  


---

## 📈 Results
- **Time Span**: ~1h10m of AAPL trading (Oct 21, 2024).  
- **Metrics**:  
  - Best-Level OFI → Top of book flow.  
  - Multi-Level OFI → Across all levels.  
  - Integrated OFI → PCA-based.  
- **Visualization**: Intraday OFI trends highlight shifting market pressure.  
- **Applications**:  
  - Market microstructure analysis  
  - Trading strategy development  
  - Predictive features in ML models  



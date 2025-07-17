
# 🏦 DeFi Wallet Credit Scoring (Aave V2 Protocol)

This project assigns a **credit score (ranging from 0 to 1000)** to each wallet address based on its historical transaction behavior on the Aave V2 protocol. The scoring system rewards responsible and consistent users while penalizing risky or bot-like behavior — enabling trust assessment for DeFi participants.

 Problem Statement

You're provided with 100K+ raw transaction-level data points from the Aave V2 protocol. Each transaction involves one of the following actions:

  deposit`
- borrow`
- repay`
- redeemunderlying`
  `liquidationcall`

Your task is to:
- Engineer meaningful features from user transaction history.
- Develop a scoring algorithm that assigns wallet-level credit scores.
- Output a CSV file mapping each wallet to a score between 0 and 1000.

---

## ⚙️ Scoring Pipeline Overview

### ✅ Step 1: Feature Engineering

For each wallet, the following features are extracted:

| Feature | Description |
|--------|-------------|
| `tx_count` | Total number of transactions |
| `deposits`, `borrows`, `repays`, etc. | Count of each action type |
| `repayment_ratio` | Total repaid / total borrowed |
| `liquidation_ratio` | Total liquidations / total borrows |
| `deposit_to_borrow` | Total deposit / total borrow |
| `avg_time_gap` | Average time between transactions |
| `unique_assets` | Number of different assets interacted with |

---

### ✅ Step 2: Rule-Based Scoring Formula

```python
score = 1000
 score -= liquidation_ratio * 400
score += repayment_ratio * 300
score += log1p(tx_count) * 20
score += deposit_to_borrow * 100
score = clip(score, 0, 1000)
````

* ✅ High repayment ratio increases score
* ❌ High liquidation ratio decreases score
* ✅ More transactions & diversity boost reliability
* ✅ Healthy deposit-borrow balance is rewarded

---

## 🚀 How to Run

### 📁 Input Format

The input is a JSON file named `sample.json`:

```json
[
  {
    "wallet_address": "0xabc123...",
    "action": "deposit",
    "amount": 1000,
    "asset": "USDC",
    "timestamp": "2023-09-01T12:00:00Z"
  },
  ...
]
```

### 🔧 Install Dependencies

```bash
pip install -r requirements.txt
```

### ▶️ Run the Scoring Script

```bash
python score_wallets.py
```

### 📤 Output

After execution, you'll get `wallet_scores.csv`:

```csv
wallet,credit_score
0xabc123...,785
0xdef456...,940
...
```

---

## 🧾 Project Structure

```
📂 defi-wallet-credit-score
├── sample.json              # Input transaction data
├── score_wallets.py         # Main scoring script
├── wallet_scores.csv        # Output file with wallet credit scores
├── requirements.txt         # Python dependencies
└── README.md                # Project documentation
```

---

## 🛠️ Future Improvements

* Replace rule-based logic with ML model (e.g. XGBoost, LightGBM)
* Integrate wallet behavior across multiple protocols
* Add features like wallet age, gas usage, interaction patterns
* Deploy as a Streamlit dashboard or API

---

## 👨‍💻 Author

ashish Tripathi
🔗 [LinkedIn](https://www.linkedin.com/in/ashishtripathi-analytics)
💻 [GitHub](https://github.com/your-github-handle)


Let me know if you'd like:
- The actual `score_wallets.py` script
- A matching `requirements.txt`
- A zip-ready project folder to upload to GitHub

# wagego / fundo 样例字段对比

---

## 2. 入参对比

左列 `wagego`，右列 `fundo`。无标注的行两边一致。

### 2.1 头部与 `bank_accounts`

```
{                                                │ {
  "userId": 484579009,                           │   "userId": 484579009,
  "applicationId": 2513560,                      │   "applicationId": 2513560,
  "flowTime": "2026-07-05 23:52:48.0",           │   "flowTime": "2026-07-05 23:52:48.0",
  "bank_accounts": [                             │   "bank_accounts": [
    {                                            │     {
      "bsb": "062692",                           │                                         ← wagego 独有
      "account_number": "76126685",              │       "account_number": "76126685",
                                                 │       "bank_account_id": 1042813323,    ← fundo 的账户主键
      "bank": "cba",                             │       "bank": "cba",
      "institution": "cba",                      │                                         ← wagego 独有
      "account_type": "savings",                 │       "account_type": "transaction"   
      "account_holder": "HEIDI ABIGAIL RISELEY", │                                         ← wagego 独有
      "account_holder_type": "single",           │                                         ← wagego 独有
      "account_name": "You Want Saving"          │                                         ← wagego 独有
    }                                            │     }
  ],                                             │   ],
```

### 2.2 流水

```
  "raw_transactions": [                                             │   "illion_raw_transactions": [                           ← 数组名不同
    {                                                               │     {
      "transaction_date": "2026-09-15",                             │       "transaction_date": "2026-02-05",
      "amount": 1320.15,                                            │       "amount": -12.32,
      "balance": 1339.16,                                           │       "balance": -126.28,
      "dr_cr": "credit",                                            │       "dr_cr": "debit",
      "text": "Direct Credit 421520 Doyle Racing Pty Doyle Racing", │       "text": "UBER *TRIP HELP.UBER.C 14518236738 AUS",
      "category": "External Transfers",                             │       "category": "Transport",
      "third_party": "External Transfers",                          │       "third_party": "UBER",
      "trx_type": "",                                               │       "trx_type": null,
      "secondary_category": "",                                     │                                                          ← wagego 独有
      "account_number": "31890460"                                  │       "account_number": "76126685",
                                                                    │       "bank_account_id": 1042813323,                     ← 账户外键，类型不同
                                                                    │       "transaction_id": 1423884392                       ← fundo 独有
    }                                                               │     }
  ]                                                                 │   ]
}                                                                   │ }
```

---

## 3. 出参对比

### 3.1 顶层与 `stats`

```
{                                                │ {
  "userId": 484579009,                           │   "userId": 484579009,
  "applicationId": 2513560,                      │   "applicationId": 2513560,
  "flowTime": "2026-07-05 23:52:48.0",           │   "flowTime": "2026-07-05 23:52:48.0",
  "stats": {                                     │   "stats": {
    "bscat_txn_raw_input_cnt": 1,                │     "bscat_txn_raw_input_cnt": 1,
    "bscat_transaction_date_max": "2026-09-15",  │     "bscat_transaction_date_max": "2026-02-05",    ← 各自样例的流水最大日期
    "bscat_product": "wagego"                    │     "bscat_product": "fundo"                       ← 取值随产品不同
  },                                             │   },
```

### 3.2 `bank_accounts`

出参与各自入参**逐字相同**，未做加工，对比见 2.1。

### 3.3 `transactions`

```
  "transactions": [                                     │   "transactions": [
    {                                                   │     {
      "secondary_category": "",                         │                                                          ← wagego 独有
      "account_number": "31890460",                     │       "account_number": "76126685",
                                                        │       "bank_account_id": 1042813323,                     ← 账户外键，类型不同
      "transaction_date": "2026-09-15",                 │       "transaction_date": "2026-02-05",
      "amount": -9.97,                                  │       "amount": -12.32,
      "balance": 19.01,                                 │       "balance": -126.28,
      "dr_cr": "debit",                                 │       "dr_cr": "debit",
      "text": "1630-REDDY EXPRESS BROA BROADMEADOW AU", │       "text": "UBER *TRIP HELP.UBER.C 14518236738 AUS",
      "category": "Automotive",                         │       "category": "Transport",
      "third_party": "Reddy Express",                   │       "third_party": "UBER",
      "trx_type": "",                                   │       "trx_type": null,
                                                        │       "transaction_id": 1423884392,                      ← fundo 独有
──────────────────────────────────────── ── 以下为出参新增的加工字段 ── ─────────────────────────────────────────
      "bscat_counterparty": "Uber",                     │       "bscat_counterparty": "Uber",
      "bscat": "Transport",                             │       "bscat": "Transport",
      "bscat_stream_id": null                           │       "bscat_stream_id": null
    }                                                   │     }
  ],                                                    │   ],
```

### 3.4 `summaries`

两产品**逐字相同**（子结构、字段名、取值全一致），故只列一份：

```json
  "summaries": {
    "income_summary": [
      {
        "bscat_stream_id": "wage_001",
        "bscat_income_category": "salary_payg",
        "bscat_transaction_start_date": "2025-11-20",
        "bscat_transaction_end_date": "2026-05-07",
        "bscat_status": "active",
        "bscat_transaction_count": 13,
        "bscat_total_income_amount": 39962.91,
        "bscat_average_income_amount": 3074.07,
        "bscat_median_income_amount": 3214.0,
        "bscat_latest_income_amount": 3314.01,
        "bscat_estimated_monthly_income": 6963.666666666667,
        "bscat_frequency": "fortnightly",
        "bscat_frequency_day": "Thursday",
        "bscat_predicted_next_income_date": "2026-05-21"
      }
    ],
    "liability_summary": [
      {
        "bscat_stream_id": "loan_004",
        "bscat_liability_category": "Non SACC Loans",
        "bscat_transaction_start_date": "2025-11-19",
        "bscat_transaction_end_date": "2026-03-11",
        "bscat_status": "Closed",
        "bscat_funded_amount": 0.0,
        "bscat_repaid_amount": 750.48,
        "bscat_repayment_amount": null,
        "bscat_recent_fn_repay_amount": 0.0,
        "bscat_frequency": "fortnightly",
        "bscat_frequency_day": "Wednesday",
        "bscat_predicted_closing_date": "NA"
      }
    ],
    "category_summary": [
      {
        "bscat": "Transport",
        "bscat_transaction_start_date": "2026-02-05",
        "bscat_transaction_end_date": "2026-02-05",
        "bscat_transaction_count": 1,
        "bscat_total_amount": 12.32,
        "bscat_average_amount": 12.32,
        "bscat_median_amount": 12.32,
        "bscat_latest_amount": 12.32
      }
    ]
  }
```

> **不重复输出**：`applicationId`（顶层已有）、`bscat` / `bscat_counterparty`（`transactions[]` 已有）均不在 summary 里重复；summary 只保留 `bscat_stream_id` 作关联主键，加上本表独有的聚合指标。`category_summary` 的 `bscat` 是聚合维度本身，故保留。

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
  "product": "wagego",                           │   "product": "fundo",                   ← 取值随产品不同
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

### 3.1 顶层与 `bscat_stats`

```
{                                         │ {
  "userId": 484579009,                    │   "userId": 484579009,
  "applicationId": 2513560,               │   "applicationId": 2513560,
  "flowTime": "2026-07-05 23:52:48.0",    │   "flowTime": "2026-07-05 23:52:48.0",
  "bscat_stats": {                        │   "bscat_stats": {
    "txn_raw_input_cnt": 1,               │     "txn_raw_input_cnt": 1,
    "transaction_date_max": "2026-09-15", │     "transaction_date_max": "2026-02-05",      ← 各自样例的流水最大日期
    "product": "wagego"                   │     "product": "fundo"                         ← 取值随产品不同
  },                                      │   },
```

### 3.2 `bank_accounts`

出参与各自入参**逐字相同**，未做加工，对比见 2.1。

### 3.3 `bscat_transactions`

```
  "bscat_transactions": [                               │   "bscat_transactions": [
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
      "counterparty": "Uber",                           │       "counterparty": "Uber",
      "bscat": "Transport",                             │       "bscat": "Transport",
      "stream_id": null                                 │       "stream_id": null
    }                                                   │     }
  ],                                                    │   ],
```

### 3.4 `bscat_summaries`

两产品**逐字相同**（子结构、字段名、取值全一致），故只列一份：

```json
  "bscat_summaries": {
    "income_summary": [
      {
        "stream_id": "wage_001",
        "income_category": "salary_payg",
        "transaction_start_date": "2025-11-20",
        "transaction_end_date": "2026-05-07",
        "status": "active",
        "transaction_count": 13,
        "total_income_amount": 39962.91,
        "average_income_amount": 3074.07,
        "median_income_amount": 3214.0,
        "latest_income_amount": 3314.01,
        "estimated_monthly_income": 6963.666666666667,
        "frequency": "fortnightly",
        "frequency_day": "Thursday",
        "predicted_next_income_date": "2026-05-21"
      }
    ],
    "liability_summary": [
      {
        "stream_id": "loan_004",
        "liability_category": "Non SACC Loans",
        "transaction_start_date": "2025-11-19",
        "transaction_end_date": "2026-03-11",
        "status": "Closed",
        "funded_amount": 0.0,
        "repaid_amount": 750.48,
        "repayment_amount": null,
        "recent_fn_repay_amount": 0.0,
        "frequency": "fortnightly",
        "frequency_day": "Wednesday",
        "predicted_closing_date": "NA"
      }
    ],
    "category_summary": [
      {
        "bscat": "Transport",
        "transaction_start_date": "2026-02-05",
        "transaction_end_date": "2026-02-05",
        "transaction_count": 1,
        "total_amount": 12.32,
        "average_amount": 12.32,
        "median_amount": 12.32,
        "latest_amount": 12.32
      }
    ]
  }
```

> **不重复输出**：`applicationId`（顶层已有）、`bscat` / `counterparty`（`bscat_transactions[]` 已有）均不在 summary 里重复；summary 只保留 `stream_id` 作关联主键，加上本表独有的聚合指标。`category_summary` 的 `bscat` 是聚合维度本身，故保留。

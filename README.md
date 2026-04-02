## Notebook

[Open analysis notebook](notebooks/ecommerce_analytics.ipynb)

# ecommerce-analytics

Ecommerce analytics project focused on revenue trends, order behavior, and customer spending patterns using Python (pandas) and SQL.

---

## Project Overview

This project analyzes ecommerce order data to identify business trends and customer behavior.

The goal is to simulate a real-world analytics workflow:
- clean and validate data
- calculate business metrics
- translate SQL logic into pandas
- extract actionable business insights

---

## Dataset

The dataset represents ecommerce orders and includes:

- `order_id` — unique order identifier  
- `user_id` — customer identifier  
- `order_date` — date of order  
- `amount` — order value  
- `status` — order status (e.g., completed, cancelled)  

For advanced analysis (D4), a transaction-style dataset was created to simulate sequential user behavior.

---

## Tools Used

- Python (pandas)
- SQL (analytical queries and window logic)
- Google Colab / Jupyter Notebook
- GitHub

---

## Project Structure

ecommerce-analytics/
│
├── notebooks/
│   └── ecommerce_analytics.ipynb
│
├── images/
│
└── README.md

---

## Business Questions

This project explores key analytical questions:

- What is the total revenue and number of orders?
- What share of orders is successfully completed?
- How does revenue change over time?
- Which users generate the most revenue?
- How does user spending evolve across transactions?

---

## Current Analyses

### D1 — Revenue Overview
- total revenue
- total orders
- completed orders
- completion rate
- daily revenue trend

## Example Output

### Daily Revenue Trend

![Revenue Trend](images/revenue_trend.png)

### D2 — User Revenue Analysis
- revenue by user
- top customers
- contribution of users to total revenue
import matplotlib.pyplot as plt

# revenue by user
user_revenue = (
    df[df['status'] == 'completed']
    .groupby('user_id', as_index=False)['amount']
    .sum()
    .sort_values('amount', ascending=False)
)

# take top 10 users
top_users = user_revenue.head(10)

# plot
plt.figure()
plt.bar(top_users['user_id'].astype(str), top_users['amount'])

plt.title('Top 10 Users by Revenue')
plt.xlabel('User ID')
plt.ylabel('Revenue')

plt.xticks(rotation=45)
plt.tight_layout()

plt.savefig('images/top_users_revenue.png')
plt.show()


### D3 — Order Status Analysis
- completed vs cancelled orders
- operational performance indicators

### D4 — User Spending Growth Over Time
- sequential comparison of user transactions
- calculation of growth between current and previous order
- detection of significant growth events (≥20%)
- identification of users with increasing spending behavior

---

## Key Insights

- Completion rate reflects operational efficiency in order processing  
- Revenue trends reveal changes in business performance over time  
- A small group of users typically generates a large share of revenue  
- Sequential analysis helps detect behavioral changes, not just absolute values  
- Users with repeated spending growth may represent high-value or upsell-ready segments  

---

## Technical Patterns Used

This project demonstrates several reusable analytics patterns:

- aggregation and KPI calculation (`SUM`, `COUNT`, ratios)
- filtering by business logic (e.g., completed orders only)
- grouping and sorting with `groupby`
- time-based analysis
- window logic translated from SQL to pandas:
df.groupby('user_id')['amount'].shift(1)
```sql
LAG(amount) OVER (PARTITION BY user_id ORDER BY created_at)

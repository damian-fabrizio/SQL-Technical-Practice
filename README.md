# SQL-Technical-Practice
Showcasing problem solving ability in SQL using actual interview problems from major companies 

## JPMorgan Chase - Card Launch Success

**Key Skills**
- Common Table Expressions
- Window Functions

**Goal** 
- Output the name of the credit card, and how many cards were issued in its launch month

**Solution** 
```sql
-- create CTE to hold rank of cards
-- window function inside CTE that ranks the cards
WITH card_rank AS (
SELECT card_name, issued_amount,
  ROW_NUMBER() OVER (
    -- separate cards by name
    PARTITION BY card_name
    -- order by earliest issue year/month
    ORDER BY issue_year, issue_month
  ) ranked_cards
FROM monthly_cards_issued
ORDER BY ranked_cards DESC
)
-- select first result for each card (assumed to be issue date)
SELECT card_name, issued_amount
FROM card_rank
WHERE ranked_cards = 1;
```
**Output**

<img width="900" height="127" alt="image" src="https://github.com/user-attachments/assets/f7547031-e51b-48f5-b8e7-527abbd191d3" />

---
## Walmart - Histogram of Users and Purchases

**Key Skills**
- Common Table Expressions
- Window Functions
- Grouping
- Aggregation

**Goal**
- Output the user's most recent transaction date, user ID, and the number of products, sorted in chronological order by the transaction date

**Solution**
```sql
-- CTE with window function inside that ranks users transactions chronologically
WITH latest_transaction AS(
SELECT transaction_date, user_id, product_id,
  RANK() OVER(
    PARTITION BY user_id
    -- most recent transaction shown first
    ORDER BY transaction_date DESC
  ) transaction_rank
FROM user_transactions
)
-- return transaction ocurring most recently
SELECT transaction_date, user_id, COUNT(product_id) AS purchase_count
FROM latest_transactions
WHERE transaction_rank = 1
GROUP BY transaction_date, user_id
ORDER BY transaction_date ASC;
```
**Output**

<img width="898" height="154" alt="image" src="https://github.com/user-attachments/assets/cfd51fa0-a886-450c-93e5-ac39abf0bc98" />


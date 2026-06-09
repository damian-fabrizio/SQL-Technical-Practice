# SQL-Technical-Practice
Showcasing problem solving ability in SQL using actual interview problems from major companies 

## JPMorgan Chase - Card Launch Success

** Key Skills **
- Common Table Expressions
- Window Functions

** Goal ** 
- Output the name of the credit card, and how many cards were issued in its launch month

** Solution ** 
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
---

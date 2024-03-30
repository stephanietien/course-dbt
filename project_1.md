## Project 1 answers

How many users do we have?
130

```
SELECT COUNT(*) AS cnt
FROM dev_db.dbt_stephanietiengetmapleca.stg_postgres__users;
```

On average, how many orders do we receive per hour?
7.5 per hour

```
WITH
orders AS (
    SELECT 
        DATE_TRUNC(hour, created_at) AS hour_ordered,
        COUNT(DISTINCT order_id) AS cnt
    FROM dev_db.dbt_stephanietiengetmapleca.stg_postgres__orders
    GROUP BY hour_ordered
)

SELECT 
    AVG(cnt)
FROM 
    orders;
```

On average, how long does an order take from being placed to being delivered?
3.9 days

```
WITH

delivery_time AS (
    SELECT
        order_id,
        DATEDIFF(minute, 'created_at', 'delivered_at') AS delivery_time_minutes
    FROM
        dev_db.dbt_stephanietiengetmapleca.stg_postgres__orders
)

SELECT
    AVG(delivery_time_minutes) AS avg_delivery_time_minutes
FROM
    delivery_time;
```

How many users have only made one purchase? Two purchases? Three+ purchases?
Note: you should consider a purchase to be a single order. In other words, if a user places one order for 3 products, they are considered to have made 1 purchase.
25 users have made 1 purchase
28 users have made 2 purchases
71 users have made 3 or more purchases

```
SELECT
    user_id,
    COUNT(order_id) AS num_orders
FROM 
    dev_db.dbt_stephanietiengetmapleca.stg_postgres__orders
GROUP BY
    user_id
HAVING 
    num_orders >= 3;
```

On average, how many unique sessions do we have per hour?
61.3 sessions per hour

```
WITH

sessions_by_hour AS (
    SELECT
        DATE_TRUNC(hour, created_at) AS hour,
        COUNT(session_id) AS num_sessions
    FROM
        dev_db.dbt_stephanietiengetmapleca.stg_postgres__events
    GROUP BY
        hour
)

SELECT
    AVG(num_sessions) AS avg_sessions
FROM
    sessions_by_hour;
```

# SQL Multi-Database Analysis Practice

A comprehensive SQL practice project covering **40+ real-world analytical queries** across 5 relational databases — designed to demonstrate end-to-end SQL proficiency for data analysis roles.

---

## Databases Covered

| Database | Description |
|---|---|
| `sql_store` | Retail store — customers, orders, products, shippers |
| `sql_invoicing` | Invoicing system — clients, invoices, payments, payment methods |
| `sql_inventory` | Inventory management — products and stock levels |
| `sql_hr` | HR system — employees, offices, reporting hierarchy |
| `mydbms` | Custom practice database — persons, payment details |

---

## Concepts Demonstrated

### DQL (Data Query Language)
- `SELECT` with calculated columns (e.g. 10% price increase)
- `WHERE` with comparison, `IN`, `BETWEEN`, `LIKE`, `IS NULL`
- `ORDER BY` (single and multi-column, ASC/DESC)
- `LIMIT` for top-N queries
- `DISTINCT` for unique value retrieval
- `GROUP BY` + aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)
- `HAVING` for post-aggregation filtering

### Joins
- `INNER JOIN` across multiple tables
- `LEFT JOIN` and `RIGHT JOIN`
- `CROSS JOIN`
- **Cross-database joins** (sql_store ↔ sql_inventory)
- **Self-join** on employees table to retrieve manager names
- Multi-table joins (3+ tables in one query)

### Subqueries & CTEs
- Subqueries in `WHERE` clause
- Derived tables in `FROM` clause
- **CTE (Common Table Expression)** using `WITH` for premium customer identification
- Subqueries in `UPDATE` statements

### DDL & DML
- `CREATE DATABASE`, `CREATE TABLE` with primary key, foreign key, auto-increment
- `ALTER TABLE` — adding columns, modifying constraints
- `INSERT`, `UPDATE`, `DELETE`
- `UPDATE` with subquery to conditionally modify data

---

## Key Queries Solved

### Business Problems
1. Display product price with 10% increase
2. Get order items for order #6 with total price > 30
3. Find top 3 loyal customers by points
4. List customers born between 1990–2000
5. Get orders placed in 2019
6. Find customers from specific states using `IN`
7. Identify customers with null phone numbers
8. Mark customers with > 3000 points as 'GOLD CUSTOMERS' using `UPDATE` + subquery
9. Calculate total payment per client per payment method
10. **Find orders with above-average item count using CTE**

### Advanced Analysis
```sql
-- CTE approach for above-average order identification
WITH TOTAL_ORDERS(ORDER_ID, TOTAL_ORDER_PER_CUSTOMER) AS (
    SELECT ORDER_ID, COUNT(*) AS TOTAL_ORDER_PER_CUSTOMER
    FROM ORDER_ITEMS
    GROUP BY ORDER_ID
),
AVG_ORDERS(AVG_ORDER_PER_CUSTOMER) AS (
    SELECT AVG(TOTAL_ORDER_PER_CUSTOMER)
    FROM TOTAL_ORDERS
)
SELECT *
FROM TOTAL_ORDERS AS T_O
JOIN AVG_ORDERS AS A_O
ON T_O.TOTAL_ORDER_PER_CUSTOMER > A_O.AVG_ORDER_PER_CUSTOMER;
```

```sql
-- Self-join to find employee-manager pairs
SELECT E1.*, B.BRANCH_ADDRESS, E2.EMP_NAME AS MANAGER
FROM EMPLOYEES AS E1
JOIN BRANCH AS B ON E1.BRANCH_ID = B.BRANCH_ID
JOIN EMPLOYEES AS E2 ON E2.EMP_ID = B.MANAGER_ID;
```

```sql
-- UPDATE with subquery — mark gold customers
UPDATE ORDERS
SET COMMENTS = 'GOLD CUSTOMERS'
WHERE CUSTOMER_ID IN (
    SELECT CUSTOMER_ID FROM CUSTOMERS WHERE POINTS > 3000
);
```

---

## How to Run

1. Open MySQL Workbench or any MySQL client
2. Run `sql_store_create.sql` first (creates all 5 databases and inserts data)
3. Run `sql_queries_practice.sql` to execute all analytical queries
4. Each query is numbered and commented with its business question

---

## Files

```
sql-multi-db-practice/
├── sql_store_create.sql         # Database schema + data (all 5 DBs)
├── sql_queries_practice.sql     # All 40+ analytical queries
└── README.md                    # This file
```

---

## Skills Demonstrated

`MySQL` `SELECT` `JOIN` `LEFT JOIN` `CROSS JOIN` `Self-Join` `Cross-DB Join`
`GROUP BY` `HAVING` `ORDER BY` `LIMIT` `DISTINCT` `WHERE` `IN` `BETWEEN` `LIKE`
`Subqueries` `CTEs` `Derived Tables` `UPDATE with Subquery`
`DDL` `DML` `DQL` `Primary Key` `Foreign Key` `AUTO_INCREMENT`
`COUNT` `SUM` `AVG` `MAX` `MIN` `IFNULL`

---

## Related Projects

- [Library Management System](../library-management-sql) — Advanced SQL with stored procedures, CTAS, overdue tracking
- [Steel Surface Defect Analysis](../steel-defect-analysis) — Python EDA on real manufacturing data
- [Uber Rides Analysis](../uber-rides-analysis) — Python EDA with feature engineering

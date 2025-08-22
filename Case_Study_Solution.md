## Q1. What is the total amount each customer spent at the restaurant?
```sql
SELECT sales.customer_id,
       SUM(menu.price) AS total_spend
FROM sales
JOIN menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id;
```
<img width="401" height="165" alt="image" src="https://github.com/user-attachments/assets/0b9d8f8e-d391-4395-8c3f-b37e7ef44b83" />

- Customer A spent $76.
- Customer B spent $74.
- Customer C spent $36.


Q2. How many days has each customer visited the restaurant?

```sql
SELECT customer_id, 
       COUNT(order_date) AS Total_Visit
FROM sales
GROUP BY customer_id;
```
<img width="388" height="165" alt="image" src="https://github.com/user-attachments/assets/54a64a2a-83bd-4055-90df-576528acc9a6" />

- Customer A visited 4 times.
- Customer B visited 6 times.
- Customer C visited 2 times.

  

Q3. What was the first item from the menu purchased by each customer?

```sql
SELECT customer_id,
       product_name,
       order_date
FROM (
    SELECT s.customer_id,
           m.product_name,
           s.order_date,
           ROW_NUMBER() OVER (PARTITION BY s.customer_id ORDER BY s.order_date) AS rn
    FROM sales s
    JOIN menu m
      ON s.product_id = m.product_id
) t
WHERE rn = 1;
```
<img width="401" height="114" alt="image" src="https://github.com/user-attachments/assets/d23a0f6a-c67c-43f5-9690-3965aa56d26a" />

- Customer A's first order was sushi.
- Customer B's first order was curry.
- Cusomer C'S first order was Ramen.
-  All the order were placed on 1st Jan 2021

  
Q4. What is the most purchased item on the menu and how many times was it purchased by all customers?


```sql
SELECT product_id, 
       COUNT(product_id) AS Total_purchase
FROM Sales
GROUP BY product_id
ORDER BY Total_purchase DESC
LIMIT 1;
```
<img width="281" height="59" alt="image" src="https://github.com/user-attachments/assets/ca5f61e6-ea9d-4199-a768-b290cc765f8d" />

- Product 3, which is Ramen is the highest purchased item, it has been purchased 8 times in total.
  

Q5. Which item was the most popular for each customer?


```sql
SELECT customer_id, product_id, purchase_count
FROM (
    SELECT customer_id,
           product_id,
           COUNT(product_id) AS purchase_count,
           ROW_NUMBER() OVER (
               PARTITION BY customer_id
               ORDER BY COUNT(product_id) DESC
           ) AS rn
    FROM sales
    GROUP BY customer_id, product_id
) t
WHERE rn = 1;
```
<img width="404" height="113" alt="image" src="https://github.com/user-attachments/assets/8668726a-8b68-46ea-aa93-b29f5aaeb318" />

- Customer A and C's favourite item is ramen.
- Customer B enjoys all items on the menu.

Q6. Which item was purchased first by the customer after they became a member?


```sql
SELECT customer_id,
       product_name,
       order_date
FROM (
    SELECT s.customer_id,
           m.product_name,
           s.order_date,
           ROW_NUMBER() OVER (
               PARTITION BY s.customer_id
               ORDER BY s.order_date
           ) AS rn
    FROM sales s
    JOIN menu m
      ON s.product_id = m.product_id
    JOIN members mem
      ON s.customer_id = mem.customer_id
    WHERE s.order_date >= mem.join_date    -- only sales after membership
) t
WHERE rn = 1;
```
<img width="401" height="82" alt="image" src="https://github.com/user-attachments/assets/b46604ff-9f27-4772-82f6-7f3f85e501f3" />

- Customer A's first order as a member is ramen.
- Customer B's first order as a member is sushi.

Q7. Which item was purchased just before the customer became a member?


```sql
SELECT customer_id, 
       product_name, 
       order_date
FROM (
    SELECT s.customer_id, 
           m.product_name, 
           s.order_date,
           ROW_NUMBER() OVER (
               PARTITION BY s.customer_id 
               ORDER BY s.order_date
           ) AS rn
    FROM sales s
    JOIN menu m
      ON s.product_id = m.product_id
    JOIN members mem
      ON s.customer_id = mem.customer_id
    WHERE s.order_date < mem.join_date   -- before becoming member
) t
WHERE rn = 1;
```
<img width="400" height="84" alt="image" src="https://github.com/user-attachments/assets/a0e7d97c-3211-420f-a5e8-171c6c77fd45" />

- Customers A purchased sushi before becaming a memebr
- Customer B purchased curry before becaming a member
  
Q8. What is the total items and amount spent for each member before they became a member?


```sql
SELECT sales.customer_id, 
       SUM(menu.price) AS total_spend
FROM sales
JOIN menu
  ON sales.product_id = menu.product_id
JOIN members
  ON sales.customer_id = members.customer_id
WHERE sales.order_date < members.join_date
GROUP BY sales.customer_id
ORDER BY customer_id;
```
<img width="279" height="82" alt="image" src="https://github.com/user-attachments/assets/57147fc4-5514-4e8f-8c9f-2169593d7da5" />

- Customer A spent $25.
- Customer B spent $52.
  
Q9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?


```sql
SELECT sales.customer_id,
       SUM(
           CASE
               WHEN sales.product_id < 3 THEN menu.price * 10
               ELSE menu.price * 20
           END
       ) AS total_points
FROM sales
JOIN menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id;
```
<img width="267" height="110" alt="image" src="https://github.com/user-attachments/assets/cde0a19b-4856-4c4b-9db9-1c2cad0fd255" />

- Customer A spend 1120
- Customer B spend 980
- Customer C spend 720
  
Q10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?


```sql
SELECT sales.customer_id,
       SUM(
           CASE
               WHEN sales.order_date <= DATE_ADD(members.join_date, INTERVAL 7 DAY) 
                    THEN menu.price * 20
               ELSE menu.price * 10
           END
       ) AS Amount_Spend
FROM sales
JOIN menu
  ON sales.product_id = menu.product_id
JOIN members
  ON sales.customer_id = members.customer_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id;
```
<img width="294" height="85" alt="image" src="https://github.com/user-attachments/assets/ef4955be-ad5d-4fbf-9d8c-996b2f98bf8a" />

- Customer A's total points are 760.
- Customer B' total points are 740.

Q11. Create a detailed dataset that combines customer orders with product details and membership status, and also assigns a ranking to each product purchased after the customer became a member.


```sql
SELECT sales.customer_id,
       sales.product_id,
       menu.product_name,
       CASE
           WHEN members.join_date IS NULL THEN 'N'        -- never became a member
           WHEN sales.order_date < members.join_date THEN 'N'
           ELSE 'Y'
       END AS membership,
       CASE 
           WHEN members.join_date IS NULL THEN 'N'        
           WHEN sales.order_date < members.join_date THEN 'N'
           ELSE 'Y'
       END AS ranking
FROM sales
JOIN menu
  ON sales.product_id = menu.product_id
LEFT JOIN members
  ON sales.customer_id = members.customer_id;
```
<img width="888" height="443" alt="image" src="https://github.com/user-attachments/assets/b4a0c1dd-5546-477f-b714-63fc36934235" />
<img width="884" height="199" alt="image" src="https://github.com/user-attachments/assets/780356a8-4e11-4ca8-8f4a-8db60c87571d" />

# Challenge Set 9
## Part I: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK?
    
    ```sql
    SELECT * 
    FROM customers 
    WHERE Country = 'UK';
    ```

2. What is the name of the customer who has the most orders?

    ```sql
    SELECT customers.customername, customers.customerid, orders.*, COUNT(*) 
    FROM customers 
    JOIN orders 
    ON customers.customerid = orders.customerid 
    GROUP BY customername
    ORDER BY COUNT(*) desc
    LIMIT 5;
    ```


3. Which supplier has the highest average product price?
    ```sql
    SELECT suppliers.suppliername, suppliers.supplierid, suppliers.suppliername, 
            products.*, SUM(products.price)/COUNT(*) as avg_price
    FROM suppliers 
    JOIN products
    ON suppliers.supplierid = products.supplierid
    GROUP BY suppliername
    ORDER BY avg_price desc
    LIMIT 1;
    ```

4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)
    ```sql
    SELECT COUNT(DISTINCT(country)) FROM customers;
    ```

5. What category appears in the most orders?

    ```sql
    SELECT products.categoryid, products.productid, orderdetails.productid, orderdetails.quantity, 
           categories.categoryid, categories.description, SUM(orderdetails.quantity) as
           sum_orders_by_category
    FROM products
    JOIN orderdetails
    ON products.productid = orderdetails.productid
    JOIN categories
    ON products.categoryid = categories.categoryid
    GROUP BY products.categoryid
    ORDER BY sum_orders_by_category desc;
    ```
6. What was the total cost for each order?
    ```sql
    SELECT orders.orderid, orderdetails.orderid, orderdetails.quantity, orderdetails.productid, 
           products.productid, products.price, products.price * orderdetails.quantity as 
           total_cost_per_order
    FROM orders
    JOIN orderdetails
    ON orders.orderid = orderdetails.orderid
    JOIN products
    ON orderdetails.productid = products.productid
    GROUP BY orders.orderid
    ORDER BY total_cost_per_order desc;
    ```

7. Which employee made the most sales (by total price)?

```sql
SELECT orders.orderid, orderdetails.orderid, orders.employeeid, 
        orderdetails.quantity, orderdetails.productid, products.productid, 
        products.price, SUM(products.price * orderdetails.quantity) as 
        total_sales_per_employee, employees.employeeid, employees.lastname, 
        employees.firstname
FROM orders
JOIN orderdetails
ON orders.orderid = orderdetails.orderid
JOIN products
ON orderdetails.productid = products.productid
JOIN employees
ON orders.employeeid = employees.employeeid
GROUP BY orders.employeeid
ORDER BY total_sales_per_employee desc
LIMIT 1;
```

8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)
```sql
SELECT firstname, lastname FROM employees
WHERE notes
LIKE '%BS%';
```

9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)

```sql
SELECT suppliers.supplierid, suppliers.suppliername, products.supplierid, SUM(products.price)/COUNT(products.price) as avg_product_price
FROM suppliers
JOIN products
ON suppliers.supplierid = products.supplierid
GROUP BY suppliers.supplierid
HAVING COUNT(products.price) >=3
ORDER BY avg_product_price desc
limit 1;
```

# Case_Study_1
**Schema (PostgreSQL v15)**

    CREATE TABLE customers (
        customer_id integer PRIMARY KEY,
        first_name varchar(100),
        last_name varchar(100),
        email varchar(100)
    );
    
    CREATE TABLE products (
        product_id integer PRIMARY KEY,
        product_name varchar(100),
        price decimal
    );
    
    CREATE TABLE orders (
        order_id integer PRIMARY KEY,
        customer_id integer,
        order_date date
    );
    
    CREATE TABLE order_items (
        order_id integer,
        product_id integer,
        quantity integer
    );
    
    INSERT INTO customers (customer_id, first_name, last_name, email) VALUES
    (1, 'John', 'Doe', 'johndoe@email.com'),
    (2, 'Jane', 'Smith', 'janesmith@email.com'),
    (3, 'Bob', 'Johnson', 'bobjohnson@email.com'),
    (4, 'Alice', 'Brown', 'alicebrown@email.com'),
    (5, 'Charlie', 'Davis', 'charliedavis@email.com'),
    (6, 'Eva', 'Fisher', 'evafisher@email.com'),
    (7, 'George', 'Harris', 'georgeharris@email.com'),
    (8, 'Ivy', 'Jones', 'ivyjones@email.com'),
    (9, 'Kevin', 'Miller', 'kevinmiller@email.com'),
    (10, 'Lily', 'Nelson', 'lilynelson@email.com'),
    (11, 'Oliver', 'Patterson', 'oliverpatterson@email.com'),
    (12, 'Quinn', 'Roberts', 'quinnroberts@email.com'),
    (13, 'Sophia', 'Thomas', 'sophiathomas@email.com');
    
    INSERT INTO products (product_id, product_name, price) VALUES
    (1, 'Product A', 10.00),
    (2, 'Product B', 15.00),
    (3, 'Product C', 20.00),
    (4, 'Product D', 25.00),
    (5, 'Product E', 30.00),
    (6, 'Product F', 35.00),
    (7, 'Product G', 40.00),
    (8, 'Product H', 45.00),
    (9, 'Product I', 50.00),
    (10, 'Product J', 55.00),
    (11, 'Product K', 60.00),
    (12, 'Product L', 65.00),
    (13, 'Product M', 70.00);
    
    INSERT INTO orders (order_id, customer_id, order_date) VALUES
    (1, 1, '2023-05-01'),
    (2, 2, '2023-05-02'),
    (3, 3, '2023-05-03'),
    (4, 1, '2023-05-04'),
    (5, 2, '2023-05-05'),
    (6, 3, '2023-05-06'),
    (7, 4, '2023-05-07'),
    (8, 5, '2023-05-08'),
    (9, 6, '2023-05-09'),
    (10, 7, '2023-05-10'),
    (11, 8, '2023-05-11'),
    (12, 9, '2023-05-12'),
    (13, 10, '2023-05-13'),
    (14, 11, '2023-05-14'),
    (15, 12, '2023-05-15'),
    (16, 13, '2023-05-16');
    
    INSERT INTO order_items (order_id, product_id, quantity) VALUES
    (1, 1, 2),
    (1, 2, 1),
    (2, 2, 1),
    (2, 3, 3),
    (3, 1, 1),
    (3, 3, 2),
    (4, 2, 4),
    (4, 3, 1),
    (5, 1, 1),
    (5, 3, 2),
    (6, 2, 3),
    (6, 1, 1),
    (7, 4, 1),
    (7, 5, 2),
    (8, 6, 3),
    (8, 7, 1),
    (9, 8, 2),
    (9, 9, 1),
    (10, 10, 3),
    (10, 11, 2),
    (11, 12, 1),
    (11, 13, 3),
    (12, 4, 2),
    (12, 5, 1),
    (13, 6, 3),
    (13, 7, 2),
    (14, 8, 1),
    (14, 9, 2),
    (15, 10, 3),
    (15, 11, 1),
    (16, 12, 2),
    (16, 13, 3);
    

---

**Query #1**

    SELECT products.product_id, products.product_name, max(products.price) as most_expensive
    FROM products
    GROUP BY products.product_id, products.product_name
    ORDER BY most_expensive desc
    LIMIT 1;

| product_id | product_name | most_expensive |
| ---------- | ------------ | -------------- |
| 13         | Product M    | 70.00          |

---
**Query #2**

    SELECT customers.customer_id, customers.first_name, customers.last_name, count(orders.order_id) as most_orders
    FROM customers
    JOIN orders
    ON customers.customer_id = orders.customer_id
    GROUP BY customers.customer_id, customers.first_name, customers.last_name
    ORDER BY most_orders desc
    LIMIT 3;

| customer_id | first_name | last_name | most_orders |
| ----------- | ---------- | --------- | ----------- |
| 2           | Jane       | Smith     | 2           |
| 1           | John       | Doe       | 2           |
| 3           | Bob        | Johnson   | 2           |

---
**Query #3**

    SELECT products.product_name, SUM(products.price*order_items.quantity) as total_revenue
    FROM products
    JOIN order_items
    ON products.product_id = order_items.product_id
    GROUP BY products.product_name, products.price, order_items.quantity
    ORDER BY total_revenue desc;

| product_name | total_revenue |
| ------------ | ------------- |
| Product M    | 420.00        |
| Product J    | 330.00        |
| Product F    | 210.00        |
| Product L    | 130.00        |
| Product K    | 120.00        |
| Product I    | 100.00        |
| Product H    | 90.00         |
| Product C    | 80.00         |
| Product G    | 80.00         |
| Product L    | 65.00         |
| Product E    | 60.00         |
| Product B    | 60.00         |
| Product K    | 60.00         |
| Product C    | 60.00         |
| Product D    | 50.00         |
| Product I    | 50.00         |
| Product H    | 45.00         |
| Product B    | 45.00         |
| Product G    | 40.00         |
| Product E    | 30.00         |
| Product B    | 30.00         |
| Product A    | 30.00         |
| Product D    | 25.00         |
| Product C    | 20.00         |
| Product A    | 20.00         |

---

[View on DB Fiddle](https://www.db-fiddle.com/f/5NT4w4rBa1cvFayg2CxUjr/4)

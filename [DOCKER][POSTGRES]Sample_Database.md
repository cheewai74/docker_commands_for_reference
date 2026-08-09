```
SELECT 1 FROM pg_database WHERE datname = 'sample_database';
-- If the result is empty, manually run:
CREATE DATABASE sample_database;

\c sample_database

-- ===========================================================================
-- 1. DROP EXISTING TABLES (Enures a clean slate during repeated testing)
-- ===========================================================================
DROP TABLE IF EXISTS order_items CASCADE;
DROP TABLE IF EXISTS orders CASCADE;
DROP TABLE IF EXISTS products CASCADE;
DROP TABLE IF EXISTS users CASCADE;

-- ===========================================================================
-- 2. CREATE SCHEMA TABLES
-- ===========================================================================

-- Users Table
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Products Table
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    stock_quantity INT DEFAULT 0 CHECK (stock_quantity >= 0)
);

-- Orders Table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(user_id) ON DELETE CASCADE,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(20) DEFAULT 'Pending' CHECK (status IN ('Pending', 'Shipped', 'Delivered', 'Cancelled'))
);

-- Order Items Junction Table
CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INT REFERENCES orders(order_id) ON DELETE CASCADE,
    product_id INT REFERENCES products(product_id) ON DELETE RESTRICT,
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10, 2) NOT NULL
);

-- ===========================================================================
-- 3. INSERT SAMPLE DATA
-- ===========================================================================

-- Mock Users
INSERT INTO users (first_name, last_name, email, created_at) VALUES
('John', 'Doe', 'john.doe@example.com', '2026-01-15 10:00:00'),
('Jane', 'Smith', 'jane.smith@example.com', '2026-02-01 11:30:00'),
('Alice', 'Johnson', 'alice.j@example.com', '2026-02-10 14:15:00'),
('Bob', 'Miller', 'bob.miller@example.com', '2026-02-20 09:45:00');

-- Mock Products
INSERT INTO products (product_name, category, price, stock_quantity) VALUES
('Wireless Mouse', 'Electronics', 25.99, 120),
('Mechanical Keyboard', 'Electronics', 89.99, 45),
('Water Bottle', 'Home & Kitchen', 15.50, 200),
('Desk Lamp', 'Home & Kitchen', 34.99, 60),
('Running Shoes', 'Apparel', 75.00, 30);

-- Mock Orders
INSERT INTO orders (user_id, order_date, status) VALUES
(1, '2026-02-22 16:20:00', 'Delivered'),
(2, '2026-02-23 10:05:00', 'Shipped'),
(1, '2026-02-24 11:00:00', 'Pending'),
(3, '2026-02-24 15:30:00', 'Pending');

-- Mock Order Line Items
INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES
(1, 1, 1, 25.99), -- John bought 1 Wireless Mouse
(1, 3, 2, 15.50), -- John bought 2 Water Bottles
(2, 2, 1, 89.99), -- Jane bought 1 Mechanical Keyboard
(3, 4, 1, 34.99), -- John bought 1 Desk Lamp
(4, 5, 1, 75.00), -- Alice bought 1 Running Shoes
(4, 1, 1, 25.99); -- Alice bought 1 Wireless Mouse

```

# database
my


PROJECT: SPACE_MARKET
SQL PREPARATION CODES
-- BRAND TABLE
CREATE TABLE brands (
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100) NOT NULL,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
-- CATEGORY TABLE
CREATE TABLE categories (
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100) NOT NULL,
parent_id INT DEFAULT NULL,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE SET NULL
);
-- PRODUCTS TABLE
CREATE TABLE products (
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(255) NOT NULL,
description TEXT,
price DECIMAL(10, 2) NOT NULL,
stock INT NOT NULL,
brand_id INT NOT NULL,
category_id INT NOT NULL,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
FOREIGN KEY (brand_id) REFERENCES brands(id) ON DELETE CASCADE,
FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE CASCADE
);
-- CART ITEMS TABLE
CREATE TABLE cartitems (
id INT AUTO_INCREMENT PRIMARY KEY,
product_id INT NOT NULL,
quantity INT NOT NULL DEFAULT 1,
user_id INT NOT NULL,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);
-- ORDERS TABLE
CREATE TABLE orders (
id INT AUTO_INCREMENT PRIMARY KEY,
user_id INT NOT NULL,
total_amount DECIMAL(10, 2) NOT NULL,
status ENUM('pending', 'completed', 'cancelled') NOT NULL DEFAULT 'pending',
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
-- ORDER ITEMS TABLE
CREATE TABLE orderitems (
id INT AUTO_INCREMENT PRIMARY KEY,
order_id INT NOT NULL,
product_id INT NOT NULL,
quantity INT NOT NULL,
price DECIMAL(10, 2) NOT NULL,
FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);
Adding sample data
-- BRANDS
INSERT INTO brands (name) VALUES
('Nike'),
('Samsung'),
('Penguin Books');
-- CATEGORIES
INSERT INTO categories (name, parent_id) VALUES
('Clothing', NULL),
('Technology', NULL),
('Books', NULL),
('Men Clothing', 1),
('Women Clothing', 1);
-- PRODUCTS
INSERT INTO products (name, description, price, stock, brand_id, category_id) VALUES
('Nike Running Shoes', 'Comfortable and lightweight running shoes.', 120.00, 50, 1, 4),
('Samsung Galaxy S23', 'Latest model with advanced features.', 999.99, 30, 2, 2),
('The Great Gatsby', 'Classic novel by F. Scott Fitzgerald.', 15.00, 100, 3, 3),
('Nike Jacket', 'Waterproof jacket for all seasons.', 80.00, 25, 1, 5);
-- CART ITEMS (örnek kullanıcı için)
INSERT INTO cartitems (product_id, quantity, user_id) VALUES
(1, 2, 101),
(3, 1, 101);
-- ORDERS
INSERT INTO orders (user_id, total_amount, status) VALUES
(101, 135.00, 'completed');
-- ORDER ITEMS
INSERT INTO orderitems (order_id, product_id, quantity, price) VALUES
(1, 1, 2, 120.00),
(1, 3, 1, 15.00);

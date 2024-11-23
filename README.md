-- NIKE DATABASE: Veritabanını oluştur
CREATE DATABASE nike;

-- Oluşturulan veritabanını kullan
USE nike;

-- BRANDS TABLOSU
CREATE TABLE brands (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- CATEGORIES TABLOSU
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    parent_id INT DEFAULT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE SET NULL
);

-- PRODUCTS TABLOSU
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

-- CART ITEMS TABLOSU
CREATE TABLE cartitems (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT NOT NULL,
    quantity INT NOT NULL DEFAULT 1,
    user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- ORDERS TABLOSU
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    status ENUM('pending', 'completed', 'cancelled') NOT NULL DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- ORDER ITEMS TABLOSU
CREATE TABLE orderitems (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- ÖRNEK VERİLERİ EKLEME

-- BRANDS
INSERT INTO brands (name) VALUES ('Nike');

-- CATEGORIES
INSERT INTO categories (name) VALUES ('Footwear');
INSERT INTO categories (name, parent_id) VALUES ('Sneakers', 1);
INSERT INTO categories (name, parent_id) VALUES ('Sports Shoes', 1);

-- PRODUCTS
INSERT INTO products (name, description, price, stock, brand_id, category_id) VALUES
('Air Force 1', 'Classic Nike sneakers', 120.99, 50, 1, 2),
('Air Max 90', 'Comfortable and stylish', 150.00, 30, 1, 2),
('ZoomX Vaporfly', 'High-performance running shoes', 250.00, 20, 1, 3);

-- CART ITEMS
INSERT INTO cartitems (product_id, quantity, user_id) VALUES
(1, 2, 101), -- 2 adet Air Force 1, kullanıcı 101
(2, 1, 102); -- 1 adet Air Max 90, kullanıcı 102

-- ORDERS
INSERT INTO orders (user_id, total_amount, status) VALUES
(101, 241.98, 'completed'), -- Kullanıcı 101'in tamamlanan siparişi
(102, 150.00, 'pending'); -- Kullanıcı 102'nin bekleyen siparişi

-- ORDER ITEMS
INSERT INTO orderitems (order_id, product_id, quantity, price) VALUES
(1, 1, 2, 120.99), -- Sipariş 1: 2 adet Air Force 1
(2, 2, 1, 150.00); -- Sipariş 2: 1 adet Air Max 90

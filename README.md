CREATE TABLE Customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(15),
    address TEXT
);


-- Insert Customers
INSERT INTO Customers (first_name, last_name, email, phone, address) 
VALUES 
('John', 'Doe', 'john@example.com', '1234567890', '123 Main St'),
('Jane', 'Smith', 'jane@example.com', '0987654321', '456 Oak St');


CREATE TABLE Products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    description TEXT,
    price DECIMAL(10,2),
    stock_quantity INT
);



-- Insert Products
INSERT INTO Products (product_name, description, price, stock_quantity) 
VALUES 
('Laptop', 'Dell XPS 13', 1200.00, 10),
('Smartphone', 'iPhone 14', 999.00, 15),
('Headphones', 'Sony WH-1000XM4', 350.00, 20);

CREATE TABLE Orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2),
    status VARCHAR(20) DEFAULT 'Pending',
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE
);

-- Insert an Order
INSERT INTO Orders (customer_id, total_amount, status) 
VALUES (1, 0, 'Pending');

-- Order_Items Table
CREATE TABLE Order_Items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    subtotal DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES Products(product_id) ON DELETE CASCADE
);


INSERT INTO Order_Items (order_id, product_id, quantity, subtotal) 
VALUES 
(1, 1, 1, 1200.00),
(1, 3, 2, 700.00);


-- Payments Table

CREATE TABLE Payments (
    payment_id SERIAL PRIMARY KEY,
    order_id INT,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    payment_method VARCHAR(20),
    amount DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id) ON DELETE CASCADE
);

-- Insert Payment
INSERT INTO Payments (order_id, payment_method, amount) 
VALUES (1, 'Credit Card', 1900.00);

SELECT o.order_id, c.first_name, c.last_name, o.order_date, o.total_amount, o.status 
FROM Orders o
JOIN Customers c ON o.customer_id = c.customer_id;

SELECT oi.order_id, p.product_name, oi.quantity, oi.subtotal 
FROM Order_Items oi
JOIN Products p ON oi.product_id = p.product_id
WHERE oi.order_id = 1;


SELECT p.payment_id, o.order_id, p.payment_method, p.amount, p.payment_date 
FROM Payments p
JOIN Orders o ON p.order_id = o.order_id
WHERE o.order_id = 1;

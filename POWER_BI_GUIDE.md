-- 1. Total sales and revenue
SELECT SUM(quantity_sold) AS total_units,
       ROUND(SUM(revenue), 2) AS total_revenue
FROM sales;

-- 2. Monthly revenue trend
SELECT DATE_FORMAT(sale_date, '%Y-%m') AS sales_month,
       ROUND(SUM(revenue), 2) AS monthly_revenue
FROM sales
GROUP BY DATE_FORMAT(sale_date, '%Y-%m')
ORDER BY sales_month;

-- 3. Best-selling products
SELECT p.product_name, p.category,
       SUM(s.quantity_sold) AS units_sold,
       ROUND(SUM(s.revenue), 2) AS revenue
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY p.product_id, p.product_name, p.category
ORDER BY units_sold DESC
LIMIT 10;

-- 4. Low-stock items
SELECT st.store_name, p.product_name, i.stock_quantity, i.reorder_level
FROM inventory i
JOIN products p ON i.product_id = p.product_id
JOIN stores st ON i.store_id = st.store_id
WHERE i.stock_quantity <= i.reorder_level
ORDER BY i.stock_quantity;

-- 5. Category performance
SELECT p.category,
       SUM(s.quantity_sold) AS units_sold,
       ROUND(SUM(s.revenue), 2) AS revenue
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY p.category
ORDER BY revenue DESC;

-- 6. Gross profit estimate (before operating expenses)
SELECT p.category,
       ROUND(SUM(s.quantity_sold * (p.unit_price - p.unit_cost)), 2) AS estimated_gross_profit
FROM sales s
JOIN products p ON s.product_id = p.product_id
GROUP BY p.category
ORDER BY estimated_gross_profit DESC;

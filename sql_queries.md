# SQL запросы для проверки данных

База данных: схема интернет-магазина (PostgreSQL).  
Запросы используются для валидации заказов, пользователей, остатков и промокодов.

## 1. Проверка последних заказов пользователя

```sql
SELECT o.id, o.total, o.status, o.created_at
FROM orders o
JOIN users u ON o.user_id = u.id
WHERE u.email = 'testuser@example.com'
ORDER BY o.created_at DESC
LIMIT 5;
```

## 2. Состав последнего заказа

```sql
SELECT oi.product_id, p.name, oi.quantity, oi.price
FROM order_items oi
JOIN products p ON oi.product_id = p.id
WHERE oi.order_id = (SELECT MAX(id) FROM orders WHERE user_id = 1);
```

## 3. Остатки товаров после оформления заказа

```sql
SELECT p.name, p.stock
FROM products p
WHERE p.id IN (SELECT product_id FROM order_items WHERE order_id = 123);
```

## 4. Проверка использования промокода

```sql
SELECT * FROM promo_codes
WHERE code = 'SUMMER2026' AND used = 0;
```

## 5. Поиск дублирующихся промокодов

```sql
SELECT code, COUNT(*) AS cnt
FROM promo_codes
GROUP BY code
HAVING COUNT(*) > 1;
```

## 6. Незавершённые заказы (пользователи)

```sql
SELECT u.email, o.id, o.status, o.created_at
FROM orders o
JOIN users u ON o.user_id = u.id
WHERE o.status = 'pending';
```

## 7. Проверка целостности данных при оплате наличными (после обнаружения бага BR-004)

```sql
SELECT id, payment_method, status
FROM orders
WHERE payment_method = 'cash' AND status = 'confirmed';
```

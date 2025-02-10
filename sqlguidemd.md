# Мини-учебник для начинающих разработчиков SQL: Основы и продвинутые запросы

## Введение в SQL
**SQL (Structured Query Language)** — язык программирования для управления реляционными базами данных. Он позволяет:
- Создавать и изменять структуру данных (таблицы, индексы).
- Добавлять, обновлять, удалять данные.
- Извлекать данные с помощью гибких запросов.

### Основные концепции
- **Таблицы**: Коллекции данных, организованные в строки и столбцы.
- **Столбцы (поля)**: Определяют тип данных (например, `INT`, `VARCHAR`).
- **Строки (записи)**: Конкретные экземпляры данных.

---

## Основные Команды SQL

### 1. Создание таблицы: `CREATE TABLE`

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

- **`PRIMARY KEY`**: Уникальный идентификатор записи.
- **`AUTO_INCREMENT`**: Автоматическая генерация значений.
- **`UNIQUE`**: Гарантирует уникальность значений в столбце.

### 2. Вставка данных: `INSERT INTO`

```sql
INSERT INTO Users (name, email)
VALUES ('Иван Иванов', 'ivan@example.com');
```

### 3. Выбор данных: `SELECT`

```sql
SELECT name, email FROM Users;
```

### 3.1 Фильтрация данных `WHERE`:

``` sql
SELECT * FROM Users WHERE created_at > '2023-01-01';
```

### 3.2 Сортировка `ORDER BY`:

```sql
SELECT * FROM Users ORDER BY name DESC;
```

### 3.3 Лимит записей `LIMIT`:
```sql
SELECT * FROM Users LIMIT 10;
```

### 4. Обновление данных: `UPDATE`

```sql
UPDATE Users 
SET email = 'new@example.com' 
WHERE id = 1;
```
⚠️ Важно: Всегда используйте **`WHERE`**, чтобы не обновить все строки!

### 5. Удаление данных: `DELETE`

```sql
DELETE FROM Users WHERE id = 1;
```

---

## Продвинутые Запросы
### 1. Объединение таблиц: JOIN
**`INNER JOIN`**: Выбирает записи с совпадениями в обеих таблицах.
```sql
SELECT Users.name, Orders.amount
FROM Users
INNER JOIN Orders ON Users.id = Orders.user_id;
```
**`LEFT JOIN`**: Все записи из левой таблицы + совпадения из правой.
```sql
SELECT Users.name, Orders.amount
FROM Users
LEFT JOIN Orders ON Users.id = Orders.user_id;
```
⚠️ Важно: На начальных этапах изучения SQL уделите внимания именно этим способам соединения таблиц, а после досконального изучения можете приступить к `RIGHT`, `FULL OUTER`, `CROSS`, `SELF JOIN`'ам.

### 2. Агрегатные функции
- COUNT(): Количество записей.
- SUM(): Сумма значений.
- AVG(): Среднее значение.
- MAX()/MIN(): Максимальное/минимальное значение.

Пример с **GROUP BY**:
```sql
SELECT user_id, SUM(amount) AS total
FROM Orders
GROUP BY user_id;
```

### 3. Подзапросы (Subqueries)
### Запрос внутри другого запроса:

``` sql
SELECT name 
FROM Users 
WHERE id IN (SELECT user_id FROM Orders WHERE amount > 100);
```

---
## Оптимизация и Безопасность
### 1. Индексы
Ускоряют поиск данных. Пример создания индекса:
```sql
CREATE INDEX idx_email ON Users (email);
```

### 2. Транзакции
```sql
START TRANSACTION;
UPDATE Account SET balance = balance - 100 WHERE id = 1;
UPDATE Account SET balance = balance + 100 WHERE id = 2;
COMMIT; -- или ROLLBACK при ошибке
```
---

## Советы

### 1. Всегда делайте резервные копии перед массовыми операциями

### 2. Используйте понятные имена таблиц и столбцов (например, user_orders вместо tbl1)

### 3. Тестируйте запросы на тестовой базе перед выполнением в основной базе данных

---

## Частые Ошибки

### 1. Отсутствие WHERE в UPDATE/DELETE: Может привести к изменению всех данных.

### 2. Использование `SELECT * from table`: Выбирайте только нужные столбцы для экономии ресурсов.

### 3. Игнорирование индексов: Запросы к большим таблицам без индексов работают медленно

---

## Заключение

### SQL — мощный инструмент для работы с данными. Начните с простых запросов, постепенно осваивая более сложные. Для углубления знаний изучайте документацию вашей СУБД (MySQL, PostgreSQL и т.д.) и практикуйтесь на реальных проектах.

# PostgreSQL Задание

### 1. Напишите SQL запрос, который создает 2 таблицы: dep_a, dep_b
Таблица dep_a должна иметь следующие поля:
a (целое число, уникальное значение)
name_a (строка длинной не более 100 символов, не уникальное)
Таблица dep_b должна иметь следующие поля:
b (целое число, уникальное значение)
name_b (строка длинной не более 100 символов, не уникальное)
```sql
CREATE TABLE IF NOT EXISTS dep_a (
a INTEGER PRIMARY KEY,
name_a VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS dep_b (
b INTEGER PRIMARY KEY,
name_b VARCHAR(100) NOT NULL
);
```

### 2. Напишите SQL запрос, который наполняет таблицу dep_a следующими данными:
```sql
INSERT INTO dep_a (a, name_a) VALUES (1, 'Андрей'), (2, 'Ольга'), (3, 'Борис'), (4, 'Светлана');
```

### 3. Напишите SQL запрос, который наполняет таблицу dep_b следующими данными:
```sql
INSERT INTO dep_b (b, name_b) VALUES (1, 'Ольга'), (2, 'Андрей'), (3, 'Валентина'), (4, 'Петр');
```

### 4. Напишите SQL запросы, которые возвращают следующие результаты:

## 4.1  Запрос 1
```sql
SELECT t1.*, t2.* from dep_a t1 INNER JOIN dep_b t2 ON t1.name_a = t2.name_b
```
## 4.2 Запрос 2
```sql
SELECT t1.*, t2.* from dep_a t1 LEFT JOIN dep_b t2 ON t1.name_a = t2.name_b
```
## 4.3 Запрос 3
```sql
SELECT t1.*, t2.* from dep_a t1 LEFT JOIN dep_b t2 ON t1.name_a = t2.name_b WHERE t2.name_b IS NULL
```
## 4.4 Запрос 4
```sql
SELECT t1.*, t2.* from dep_a t1 RIGHT JOIN dep_b t2 ON t1.name_a = t2.name_b WHERE t2.name_b IS NULL
```
## 4.5 Запрос 5
```sql
SELECT t1.*, t2.* from dep_a t1 RIGHT JOIN dep_b t2 ON t1.name_a = t2.name_b WHERE t1.name_a IS NULL
```
## 4.6 Запрос 6
```sql
SELECT t1.*, t2.* from dep_a t1 RIGHT JOIN dep_b t2 ON t1.name_a = t2.name_b WHERE t1.name_a IS NULL
```
## 4.7 Запрос 7
```sql
SELECT t1.*, t2.* from dep_a t1 FULL OUTER JOIN dep_b t2 on t1.name_a = t2.name_b WHERE t1.name_a is null OR t2.name_b is null
```
## 4.8 Запрос 8
```sql
SELECT COUNT(*) FROM dep_a
```
## 4.9 Запрос 9
```sql
ALTER TABLE dep_b ADD COLUMN amount integer;
```

### 5 Опишите процесс создания резервной копии базы данных с именем «demo» и восстановления этой базы на другом сервере с именем «demo_new». Ответ долженсодержать SQL запросы или команды, которые необходимо выполнить.

Для создания резервной копии базы данных PostgreSQL с именем «demo», необходимо выполнить следующие шаги:
1. Открыть терминал и войти в командную строку PostgreSQL с помощью команды:
```shell
psql
```

2. Выбрать базу данных «demo» с помощью команды:
```shell
\c demo
```

3. Создать резервную копию базы данных с помощью команды:
```shell
pg_dump demo > demo_backup.sql
```

Для восстановления базы данных «demo» на другом сервере с именем «demo_new», необходимо выполнить следующие шаги:
1. Скопировать файл резервной копии «demo_backup.sql» на сервер «demo_new».

2. Открыть терминал и войти в командную строку PostgreSQL с помощью команды:
```shell
psql
```

3. Создать базу данных «demo_new» с помощью команды:
```sql
CREATE DATABASE demo_new;
```

4. Восстановить резервную копию базы данных с помощью команды:
```sql
\i demo_backup.sql
```

База данных «demo» была восстановлена на новом сервере с именем «demo_new».

### 6 Опишите как можно посмотреть последние несколько SQL запросов, которые были сделаны в БД. Ответ должен содержать пример запроса или команды.

Чтобы посмотреть последние несколько SQL запросов в PostgreSQL, можно использовать системную таблицу 'pg_stat_activity'. Она содержит информацию о текущих активных соединениях с базой данных, включая последние SQL запросы, которые были выполнены.

Пример запроса, который позволяет посмотреть последние 10 запросов, выполненных в базе данных:
```sql
SELECT query_start, application_name, backend_type, usename, datname, query
FROM pg_stat_activity
WHERE query != '<IDLE>'  -- фильтр для исключения неактивных соединений
ORDER BY query_start DESC
LIMIT 10;
```
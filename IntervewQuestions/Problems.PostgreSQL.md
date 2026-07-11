# 🏗️ CREATE TABLE (5 Problems)

🟢 Problem 1

Create a `users` table with:

```text
id -> auto increment primary key
name -> required
email -> unique and required
age -> integer
```

```pgsql
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	name VARCHAR(50) NOT NULL,
	email TEXT UNIQUE NOT NULL,
	age INT 
);
```

🟢 Problem 2

Create a `products` table with:

```text
id -> auto increment primary key
name -> required
price -> decimal number
stock -> default 0
```

```pgsql
CREATE TABLE products (
	id SERIAL PRIMARY KEY,
	name VARCHAR(50) NOT NULL,
	price NUMERIC,
	stock INT DEFAULT 0
);
```

🟡 Problem 3

Create an `orders` table with:

```text
id -> auto increment primary key
user_id -> references users(id)
total_amount -> decimal
created_at -> current timestamp by default
```

```pgsql
CREATE TABLE orders (
	id SERIAL PRIMARY KEY,
	user_id INTEGER REFERENCES users(id),
	total_amount NUMERIC(10,2),
	created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

🟡 Problem 4

Create a `profiles` table implementing a One-to-One relationship with users.

Fields:

```text
id
user_id
bio
```

Ensure one user can have only one profile.

```pgsql
CREATE TABLE profiles (
	id SERIAL PRIMARY KEY,
	user_id INTEGER UNIQUE NOT NULL,
	bio TEXT,
	FOREIGN KEY (user_id) REFERENCES users(id)
);
```

🔴 Problem 5

Implement a Many-to-Many relationship between:

```text
students
courses
```

Create only the junction table.

```pgsql
CREATE TABLE enrollments (
	student_id INTEGER NOT NULL,
	course_id INTEGER NOT NULL,
	PRIMARY KEY(student_id, course_id),
	FOREIGN KEY(student_id) REFERENCES students(id),
	FOREIGN KEY(course_id) REFERENCES courses(id)
);
```

# 🔍 SELECT (5 Problems)

Assume:

```text
users(id, name, email, age, is_active)
```

### 🟢 Problem 1

Get all users.

```pgsql
SELECT * FROM users;
```

### 🟢 Problem 2

Get only:

```text
name
email
```

columns.

```pgsql
SELECT name,email FROM users;
```

### 🟡 Problem 3

Get users older than 25.

```pgsql
SELECT * FROM users
WHERE age > 25;
```

### 🟡 Problem 4

Get active users sorted by age descending.

```pgsql
SELECT * FROM USERS
WHERE is_active = true
ORDER BY age DESC;
```

### 🔴 Problem 5

Get the first 10 users after skipping the first 20.

(Pagination question)

```pgsql
SELECT * FROM USERS
LIMIT 10
OFFSET 20;
```

# ✏️ UPDATE (5 Problems)

### 🟢 Problem 1

Update age to 25 for user with id 1.

```pgsql
UPDATE users
SET age = 25
WHERE id = 1;
```

### 🟢 Problem 2

Set user with id 5 as inactive.

```pgsql
UPDATE users
SET is_active = false
WHERE id = 5;
```

### 🟡 Problem 3

Increase salary by 10% for user with id 3.

Assume:

```text
salary column exists
```

```pgsql
UPDATE users
SET salary = salary + (salary * 0.1)
WHERE id = 3;
```

### 🟡 Problem 4

Update both:

```text
name
email
```

for user id 2.

```pgsql
UPDATE users
SET name = 'jack', email = 'jack@g.com'
WHERE id = 2;
```

### 🔴 Problem 5

Deactivate all users younger than 18.

```pgsql
UPDATE users
SET is_active = false
WHERE age < 18;
```

# 🗑️ DELETE (5 Problems)

### 🟢 Problem 1

Delete user with id 1.

```pgsql
DELETE FROM users
WHERE id = 1;
```

### 🟢 Problem 2

Delete all inactive users.

```pgsql
DELETE FROM users
WHERE is_active = false;
```

### 🟡 Problem 3

Delete users aged below 18.

```pgsql
DELETE FROM users
WHERE age < 18
```

### 🟡 Problem 4

Delete users whose name starts with A.

```pgsql
DELETE FROM users
WHERE name LIKE 'A%';
```

### 🔴 Problem 5

Delete users whose age is NULL.

```pgsql
DELETE FROM users
WHERE age IS NULL
```

# 🔗 JOINS (5 Problems)

Assume:

```text
users(id, name)
orders(id, user_id, product)
```

### 🟢 Problem 1

Get user names along with their products.

```pgsql
SELECT u.name, o.product
FROM users u
JOIN orders o
ON u.id = o.user_id;
```

### 🟢 Problem 2

Get all users, including those without orders.

```pgsql
SELECT u.name, o.product
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id;
```

### 🟡 Problem 3

Find users who have no orders.

```pgsql
SELECT u.name, o.product
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id;

```

### 🟡 Problem 4

Get:

```text
user name
order id
product
```

```pgsql
SELECT u.name, o.id, o.product
FROM users u
JOIN orders o
ON u.id = o.user_id;
```

### 🔴 Problem 5

Find all orders along with user names, ensuring every order appears even if the user record is missing.

(Hint: Which join?)

```pgsql
SELECT u.name o.product
FROM users u
RIGHT JOIN orders o
ON u.id = o.user_id;
```

# 🔧 ALTER TABLE (5 Problems)

Assume:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

### 🟢 Problem 1

Add an email column.

```pgsql
ALTER TABLE users
ADD COLUMN email TEXT;
```

### 🟢 Problem 2

Rename column:

```text
name → full_name
```

```pgsql
ALTER TABLE users
RENAME COLUMN name TO full_name;
```

### 🟡 Problem 3

Make email mandatory.

```pgsql
ALTER TABLE users
ALTER COLUMN email
SET NOT NULL;
```

### 🟡 Problem 4

Add a unique constraint to email.

### 🔴 Problem 5

Rename table:

```text
users → customers
```

```pgsql
ALTER TABLE users
RENAME to customers;
```

# 🎯 Interview Challenge Round

These are the kind of questions interviewers suddenly throw in.

### 🔥 Challenge 1

What's wrong with:

```sql
SELECT *
FROM users
WHERE age = NULL;
```

### 🔥 Challenge 2

What happens here?

```sql
UPDATE users
SET is_active = FALSE;
```

### 🔥 Challenge 3

Difference between:

```sql
DELETE FROM users;
```

and

```sql
DROP TABLE users;
```

### 🔥 Challenge 4

Difference between:

```sql
JOIN
```

and

```sql
LEFT JOIN
```

### 🔥 Challenge 5

Why is this useful?

```sql
PRIMARY KEY(student_id, course_id)
```

in a junction table.

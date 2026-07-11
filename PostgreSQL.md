# ---Introduction

### 🏗️ SQL vs PostgreSQL

**📌 SQL**

**SQL (Structured Query Language)** is a language.

Think of it like:

* English is a language
* JavaScript is a language
* SQL is a language for talking to databases

Example:

```sql
SELECT * FROM users;
```

```sql
INSERT INTO users(name)
VALUES('Arjun');
```

These are SQL statements.

SQL itself is  **not a database** .

**📌 PostgreSQL**

PostgreSQL is an actual  **Database Management System (DBMS)** .

Think of it like:

* SQL = Language
* PostgreSQL = Software that understands SQL

Other DBMSs:

* MySQL
* PostgreSQL
* Microsoft SQL Server
* Oracle Database

All understand SQL, but each has extra features.

### 🚗 Real-world Analogy

Imagine driving.

* Traffic rules = SQL
* Toyota Car = PostgreSQL
* Honda Car = MySQL
* BMW Car = SQL Server

Everyone follows traffic rules.

But every car has additional features.

Same with databases.

### 🔥 What PostgreSQL Actually Is

PostgreSQL is:

✅ Open Source

✅ Relational Database

✅ ACID Compliant

✅ Production Grade

✅ Highly Scalable

✅ Used by startups and large companies

Many modern apps use:

```
Next.js
Node.js
Prisma
PostgreSQL
```

which is exactly why your interviewer mentioned it.

### 📊 Example Database

Suppose we have a users table.

| id | name  | email                          |
| -- | ----- | ------------------------------ |
| 1  | Arjun | [a@gmail.com](mailto:a@gmail.com) |
| 2  | Rahul | [r@gmail.com](mailto:r@gmail.com) |

Create table:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

Insert:

```sql
INSERT INTO users(name,email)
VALUES('Arjun','a@gmail.com');
```

Get users:

```sql
SELECT * FROM users;
```

This syntax is SQL.

PostgreSQL executes it.

### 🔥 Why Companies Prefer PostgreSQL

Compared to MySQL, PostgreSQL is often preferred for complex applications because it has:

**-- Better Data Integrity**

Ensures data stays consistent.

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE id = 1;

UPDATE accounts
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

Either both happen or neither happens.

No half-completed transaction.

**-- Better JSON Support**

PostgreSQL can store JSON almost like MongoDB.

```sql
CREATE TABLE users (
   id SERIAL PRIMARY KEY,
   profile JSONB
);
```

Example data:

```json
{
  "name":"Arjun",
  "skills":["React","Node"]
}
```

This is why many developers call PostgreSQL:

> "The SQL database that stole MongoDB's best features."

**-- Better Complex Queries**

For analytics, reporting, aggregation, and enterprise systems PostgreSQL is generally stronger.

### 📌 PostgreSQL vs MongoDB

Since you're a MERN developer, this comparison matters more.

**--- MongoDB**

Document based.

```json
{
  "_id":1,
  "name":"Arjun",
  "skills":["React","Node"]
}
```

No fixed schema.

Flexible.

**-- PostgreSQL**

Table based.

```sql
users
```

| id | name  |
| -- | ----- |
| 1  | Arjun |

More structured.

Relationships are stronger.

### 🔗 PostgreSQL Relationships

Suppose:

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

Orders

| id | user_id |
| -- | ------- |
| 1  | 1       |

`user_id` points to users table.

```sql
FOREIGN KEY(user_id)
REFERENCES users(id)
```

This is called a relationship.

PostgreSQL is extremely good at this.

### ⚡ Prisma + PostgreSQL

Instead of writing raw SQL:

```sql
SELECT * FROM users;
```

you write:

```ts
const users = await prisma.user.findMany();
```

Prisma converts that into PostgreSQL queries.

Architecture:

```text
Frontend (Next.js)
        ↓
Backend (API/tRPC)
        ↓
Prisma
        ↓
PostgreSQL
```

### 💼 Interview Answer

If an interviewer asks:

**"What's the difference between SQL and PostgreSQL?"**

A good answer:

> SQL is the language used to interact with relational databases, whereas PostgreSQL is a relational database management system that implements SQL and provides additional features such as transactions, indexing, JSON support, constraints, and advanced querying capabilities. SQL is the language; PostgreSQL is the database software that executes it.

That's usually enough to satisfy most Full Stack interviews. 🚀

---

# ----PostgreSQL vs MySQL

Both are:

✅ Relational Databases
✅ Open Source
✅ Use SQL
✅ ACID Compliant
✅ Widely used in production

For basic CRUD applications, they feel very similar.

### 🏗️ Philosophy

**-- MySQL**

Focused on:

* Simplicity
* Speed
* Ease of use

Traditionally popular for:

* Web applications
* CMS systems
* Blogs
* E-commerce sites

**-- PostgreSQL**

Focused on:

* Standards compliance
* Advanced features
* Data integrity
* Complex queries

Traditionally popular for:

* SaaS products
* Enterprise applications
* Analytics systems
* Data-heavy applications

### 📊 JSON Support

**-- MySQL**

Supports JSON.

```sql
CREATE TABLE users (
    id INT,
    profile JSON
);
```

Good enough for many use cases.

**-- PostgreSQL**

Supports  **JSONB** , which is one of its biggest strengths.

```sql
CREATE TABLE users (
    id SERIAL,
    profile JSONB
);
```

JSONB is:

* Faster to query
* Indexable
* More powerful

This is why many modern startups choose PostgreSQL.

### 🔗 Relationships & Complex Queries

**-- MySQL**

Good for standard joins and relationships.

```sql
SELECT *
FROM users
JOIN orders
ON users.id = orders.user_id;
```

**-- PostgreSQL**

Excellent for:

* Complex joins
* Nested queries
* Analytics
* Reporting

When data becomes complicated, PostgreSQL usually shines.

### ⚡ Performance

This depends on workload.

**-- MySQL**

Often faster for:

* Simple read-heavy operations
* Basic web applications

**-- PostgreSQL**

Often better for:

* Complex queries
* Large datasets
* Heavy concurrent workloads

### 🛡️ Data Integrity

**-- MySQL**

Good integrity.

**-- PostgreSQL**

Generally considered stricter and more standards-compliant.

PostgreSQL tends to catch data issues earlier and provides more advanced constraints.

### 🔥 Features

PostgreSQL has more advanced features built in:

* JSONB
* Materialized Views
* Window Functions
* Common Table Expressions (CTEs)
* Custom Data Types
* Full Text Search
* Advanced Indexing

MySQL has many of these now too, but PostgreSQL is usually considered more feature-rich.

### 🚀 Prisma + Next.js Ecosystem

For modern stacks like:

```text
Next.js
Prisma
Auth.js
tRPC
PostgreSQL
```

PostgreSQL is extremely common.

Many tutorials, SaaS boilerplates, and AI-powered products use PostgreSQL by default.

### Interview Answer (30-second version)

> Both MySQL and PostgreSQL are relational databases that use SQL. MySQL is generally simpler and often used for traditional web applications, while PostgreSQL provides more advanced features, stronger standards compliance, better support for complex queries, and powerful JSONB capabilities. For modern full-stack applications using Prisma and Next.js, PostgreSQL is often the preferred choice.

For your assignment, you don't need to master PostgreSQL internals. Learn:

* Tables
* Primary keys
* Foreign keys
* CRUD
* Basic joins
* Prisma models and migrations

That is enough to start building within a day. 🚀

---

# ----Postico

If you're learning PostgreSQL, **Postico** is just a GUI tool for working with PostgreSQL databases on macOS. Think of it as the PostgreSQL equivalent of MongoDB Compass. It lets you view tables, run queries, edit data, and manage databases without using the terminal. ([eggerapps.at](https://eggerapps.at/postico/docs/?utm_source=chatgpt.com "Postico User Guide"))

### 🏗️ Where Postico Fits

```text
Your App
   ↓
Prisma
   ↓
PostgreSQL Database
   ↑
Postico
```

Your application talks to PostgreSQL through Prisma.

Postico is only for  **you** , the developer, to inspect and manage the database.

### 🚀 What You Can Do in Postico

✅ Connect to local or remote PostgreSQL databases

✅ View tables and rows

✅ Insert/update/delete records

✅ Run SQL queries

✅ Explore schemas and relationships

✅ Import/export data (CSV, JSON, etc.) ([Sugggest](https://sugggest.com/software/postico?utm_source=chatgpt.com "Postico: Modern PostgreSQL Client for macOS | Sugggest"))

### Example

Suppose Prisma creates a `users` table.

Instead of running:

```sql
SELECT * FROM users;
```

in a terminal, you can open Postico and visually see:

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Rahul |

Much easier during development.

### Similar Tools

* Postico → PostgreSQL-focused, macOS only
* pgAdmin → Official PostgreSQL GUI
* DBeaver → Supports many databases
* TablePlus → Modern multi-database GUI

### Do You Need It?

For your assignment:

**Not necessarily.**

If you're using:

```text
Next.js
Prisma
PostgreSQL
```

you can do almost everything through Prisma migrations and queries.

However, having a GUI like Postico (or DBeaver/TablePlus on Windows) makes debugging much faster because you can immediately inspect what's inside the database.

### Since You're on Windows

Postico is  **macOS-only** . ([Experience League](https://experienceleague.adobe.com/en/docs/experience-platform/query/clients/postico?utm_source=chatgpt.com "Connect Postico to Query Service | Adobe Experience Platform"))

I would recommend:

* DBeaver (free, most common)
* TablePlus (clean UI)
* pgAdmin (official)

For getting productive within a day, **DBeaver + PostgreSQL + Prisma** is probably the easiest setup. 🎯

---

# ----Convention, Keywords, Capitalization, Quotes, Snake case, Alias & Comments

### 🔤 Capitalization

PostgreSQL is generally  **case-insensitive for SQL keywords** .

All of these work:

```sql
select * from users;
```

```sql
SELECT * FROM users;
```

```sql
SeLeCt * FrOm users;
```

But the convention is:

✅ SQL Keywords in UPPERCASE

```sql
SELECT *
FROM users
WHERE age > 18;
```

Keywords:

```sql
SELECT
FROM
WHERE
JOIN
GROUP BY
ORDER BY
INSERT
UPDATE
DELETE
CREATE
ALTER
DROP
```

### ✅ Table and Column Names in lowercase

```sql
SELECT name, email
FROM users;
```

Not:

```sql
SELECT Name, Email
FROM Users;
```

Most PostgreSQL developers use:

```sql
users
user_profiles
created_at
first_name
```

which is called  **snake_case** .

### 🐍 Snake Case Convention

Preferred:

```sql
first_name
last_name
created_at
updated_at
```

Avoid:

```sql
FirstName
LastName
CreatedAt
```

because PostgreSQL has some quirks with uppercase identifiers.

### ⚠️ Double Quotes and Case Sensitivity

Suppose you create:

```sql
CREATE TABLE Users (
    id SERIAL PRIMARY KEY
);
```

PostgreSQL actually stores it as:

```sql
users
```

(lowercase)

So this works:

```sql
SELECT * FROM users;
```

But if you use quotes:

```sql
CREATE TABLE "Users" (
    id SERIAL PRIMARY KEY
);
```

Now PostgreSQL preserves the case.

You must always write:

```sql
SELECT * FROM "Users";
```

This fails:

```sql
SELECT * FROM users;
```

### Interview Tip

Most developers avoid quoted identifiers.

Use:

```sql
users
orders
products
```

instead of:

```sql
"Users"
"Orders"
"Products"
```

# ; Semicolon

Semicolon means:

> End of SQL statement.

Example:

```sql
SELECT * FROM users;
```

Multiple queries:

```sql
SELECT * FROM users;

SELECT * FROM orders;
```

PostgreSQL knows where one query ends and another begins.

**-- Is Semicolon Mandatory?**

Depends.

If you're running one query:

```sql
SELECT * FROM users
```

many tools still execute it.

But convention is:

```sql
SELECT * FROM users;
```

Always add it.

### Indentation

Bad:

```sql
SELECT id,name,email FROM users WHERE age>18;
```

Good:

```sql
SELECT
    id,
    name,
    email
FROM users
WHERE age > 18;
```

### Aliases

Bad:

```sql
SELECT users.name
FROM users;
```

Good:

```sql
SELECT u.name
FROM users u;
```

Or:

```sql
SELECT u.name
FROM users AS u;
```

> #### 🎯 Whats's with users.name, etc ?
>
> Let's break it down.
>
> Suppose you have a table called `users`:
>
> | id | name  | email                                  |
> | -- | ----- | -------------------------------------- |
> | 1  | Arjun | [arjun@gmail.com](mailto:arjun@gmail.com) |
>
> `users.name`
>
> The syntax is:
>
> ```sql
> table_name.column_name
> ```
>
> So:
>
> ```sql
> users.name
> ```
>
> means:
>
>> The `name` column from the `users` table.
>>
>
> Example:
>
> ```sql
> SELECT users.name
> FROM users;
> ```
>
> Result:
>
> | name  |
> | ----- |
> | Arjun |
>
> ##### Why not just write `name`?
>
> You often can:
>
> ```sql
> SELECT name
> FROM users;
> ```
>
> This works because PostgreSQL knows you're selecting from only one table.
>
> **-- Then why use `users.name`?**
>
> Because when multiple tables are involved, PostgreSQL needs clarity.
>
> Example:
>
> ### users
>
> | id | name  |
> | -- | ----- |
> | 1  | Arjun |
>
> ### orders
>
> | id  | name         |
> | --- | ------------ |
> | 101 | Premium Plan |
>
> Notice both tables have a `name` column.
>
> Now:
>
> ```sql
> SELECT name
> FROM users
> JOIN orders
> ON users.id = orders.user_id;
> ```
>
> PostgreSQL gets confused:
>
> ❌ Which `name`?
>
> * `users.name` ?
> * `orders.name` ?
>
> So we write:
>
> ```sql
> SELECT users.name, orders.name
> FROM users
> JOIN orders
> ON users.id = orders.user_id;
> ```
>
> Now it's clear.
>
> ##### What is `u.name`?
>
> `u` is an alias (nickname).
>
> Instead of repeatedly writing:
>
> ```sql
> SELECT users.name
> FROM users;
> ```
>
> we can give the table a short name:
>
> ```sql
> SELECT u.name
> FROM users u;
> ```
>
> or
>
> ```sql
> SELECT u.name
> FROM users AS u;
> ```
>
> Meaning:
>
> ```text
> users → u
> ```
>
> Now PostgreSQL treats:
>
> ```sql
> u.name
> ```
>
> as:
>
> ```sql
> users.name
> ```
>
> ##### Why aliases become useful
>
> Imagine a large query:
>
> ```sql
> SELECT
>     users.name,
>     users.email,
>     orders.total_amount,
>     orders.created_at
> FROM users
> JOIN orders
> ON users.id = orders.user_id;
> ```
>
> Looks okay, but gets long.
>
> Using aliases:
>
> ```sql
> SELECT
>     u.name,
>     u.email,
>     o.total_amount,
>     o.created_at
> FROM users u
> JOIN orders o
> ON u.id = o.user_id;
> ```
>
> Much cleaner.

### String Quotes

Strings use  **single quotes** .

Correct:

```sql
SELECT *
FROM users
WHERE name = 'Arjun';
```

Wrong:

```sql
SELECT *
FROM users
WHERE name = "Arjun";
```

Double quotes are for identifiers.

### Comments

**Single line:**

```sql
-- Get all users
SELECT * FROM users;
```

**Multi-line:**

```sql
/*
Get active users
for dashboard
*/
SELECT * FROM users;
```

### Common PostgreSQL Naming Conventions

**Tables**

Plural:

```sql
users
orders
products
payments
```

**Primary Keys**

```sql
id
```

Eg:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY
);
```

**Foreign Keys**

```sql
user_id
order_id
product_id
```

Example:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id)
);
```

### Timestamps

Very common:

```sql
created_at
updated_at
deleted_at
```

### Query Formatting Convention

Most professional SQL code looks like:

```sql
SELECT
    u.id,
    u.name,
    u.email
FROM users u
LEFT JOIN orders o
    ON u.id = o.user_id
WHERE u.age > 18
ORDER BY u.name;
```

Notice:

* Keywords uppercase
* Table names lowercase
* snake_case
* Semicolon at end
* Indentation for readability

### What You Need for Interviews

If someone asks about SQL/PostgreSQL conventions:

✅ SQL keywords in uppercase

✅ Table/column names in snake_case lowercase

✅ Strings in single quotes

✅ Avoid quoted identifiers

✅ End statements with semicolons

✅ Use meaningful names like `user_id`, `created_at`

That's about 95% of what developers follow in real projects. 🚀

One extra PostgreSQL-specific convention you'll see everywhere when using Prisma:

```sql
users
posts
comments
```

in PostgreSQL ↔

```prisma
model User
model Post
model Comment
```

Prisma models are typically  **PascalCase** , while PostgreSQL tables are  **snake_case/lowercase** . This is a very common setup in modern Next.js + Prisma projects.

---

# ----Data Types

Data types define  **what kind of data a column can store** .

Think of them as rules for a column.

Example:

```sql
CREATE TABLE users (
    id INTEGER,
    name VARCHAR(100),
    age INTEGER
);
```

Here:

* `id` stores numbers
* `name` stores text
* `age` stores numbers

You cannot store `"Arjun"` in `age` because PostgreSQL knows `age` must be an integer.

### 🔢 Numeric Types

##### INTEGER (INT)

Stores whole numbers.

```sql
age INTEGER
```

Examples:

```text
18
25
100
```

Not allowed:

```text
18.5
```

##### BIGINT

For very large integers.

```sql
population BIGINT
```

Example:

```text
1234567890123
```

##### SMALLINT

Smaller range than INTEGER.

```sql
rating SMALLINT
```

Example:

```text
1
2
3
4
5
```

##### DECIMAL / NUMERIC

For exact decimal values.

```sql
price NUMERIC(10,2)
```

Examples:

```text
99.99
1499.50
```

`10` = total digits

`2` = digits after decimal

##### REAL / DOUBLE PRECISION

For approximate decimal values.

```sql
temperature DOUBLE PRECISION
```

Example:

```text
36.5
98.234567
```

Usually use `NUMERIC` for money.

### 📝 Text Types

##### VARCHAR(n)

Variable-length string with a limit.

```sql
name VARCHAR(100)
```

Examples:

```text
Arjun
John
```

Maximum length = 100.

##### TEXT

Unlimited text.

```sql
description TEXT
```

Examples:

```text
Blog posts
Product descriptions
Comments
```

In PostgreSQL, many developers simply use `TEXT`.

##### CHAR(n)

Fixed length.

```sql
country_code CHAR(2)
```

Example:

```text
IN
US
UK
```

If shorter, PostgreSQL pads spaces.

Less commonly used.

### 📅 Date & Time Types

##### DATE

Stores date only.

```sql
birth_date DATE
```

Example:

```text
2026-06-12
```

##### TIME

Stores time only.

```sql
start_time TIME
```

Example:

```text
14:30:00
```

##### TIMESTAMP

Stores date and time.

```sql
created_at TIMESTAMP
```

Example:

```text
2026-06-12 14:30:00
```

Very common.

##### TIMESTAMPTZ

Timestamp with timezone.

```sql
created_at TIMESTAMPTZ
```

Most production applications prefer this.

### ✅ Boolean

Stores true or false.

```sql
is_active BOOLEAN
```

Examples:

```text
TRUE
FALSE
```

### 🔑 Serial Types

Used for auto-incrementing IDs.

##### SERIAL

```sql
id SERIAL PRIMARY KEY
```

Records:

```text
1
2
3
4
```

automatically generated.

##### BIGSERIAL

Same idea but supports larger values.

```sql
id BIGSERIAL PRIMARY KEY
```

### 📦 JSON Types

One of PostgreSQL's strongest features.

##### JSON

```sql
profile JSON
```

Example:

```json
{
  "name": "Arjun",
  "skills": ["React", "Node"]
}
```

##### JSONB

```sql
profile JSONB
```

Most developers use this instead.

Advantages:

* Faster
* Indexable
* Searchable

Example:

```sql
SELECT *
FROM users
WHERE profile->>'name' = 'Arjun';
```

### 📋 Arrays

PostgreSQL can store arrays directly.

```sql
skills TEXT[]
```

Example:

```text
{"React","Node","MongoDB"}
```

Insert:

```sql
INSERT INTO users(skills)
VALUES (ARRAY['React','Node']);
```

### 📧 UUID

Used for unique IDs.

```sql
id UUID
```

Example:

```text
550e8400-e29b-41d4-a716-446655440000
```

Many modern apps use UUIDs instead of integers.

### 🌐 ENUM

Restricts values to a predefined list.

Create enum:

```sql
CREATE TYPE user_role AS ENUM (
    'ADMIN',
    'USER',
    'MODERATOR'
);
```

Use:

```sql
role user_role
```

Allowed:

```text
ADMIN
USER
MODERATOR
```

Not allowed:

```text
CEO
```

### 🎯 Most Common Types You'll Actually Use

For your Next.js + Prisma + PostgreSQL projects, you'll see these constantly:

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255),
    age INTEGER,
    is_active BOOLEAN,
    created_at TIMESTAMPTZ,
    updated_at TIMESTAMPTZ
);
```

Or in Prisma:

```prisma
model User {
  id        String   @id @default(uuid())
  name      String
  email     String   @unique
  age       Int
  isActive  Boolean
  createdAt DateTime
  updatedAt DateTime
}
```

### 🚀 Interview Cheat Sheet

| Type                  | Example Use              |
| --------------------- | ------------------------ |
| INTEGER               | Age, Quantity            |
| BIGINT                | Large numbers            |
| NUMERIC(10,2)         | Price, Salary            |
| VARCHAR(100)          | Name, Title              |
| TEXT                  | Description, Content     |
| BOOLEAN               | Active/Inactive          |
| DATE                  | Birth date               |
| TIMESTAMP/TIMESTAMPTZ | Created at               |
| SERIAL                | Auto-increment ID        |
| UUID                  | Unique IDs               |
| JSONB                 | Flexible structured data |
| ENUM                  | Role, Status             |

If you're preparing for interviews or that assignment, focus first on:

✅ INTEGER
✅ VARCHAR/TEXT
✅ BOOLEAN
✅ DATE/TIMESTAMP
✅ SERIAL or UUID
✅ JSONB

These cover about  **90% of what you'll use in real-world full-stack development** . 🚀

---

# ----Char vs Varchar

### 📌 CHAR vs VARCHAR

Both store  **text/string data** , but they handle length differently.

| CHAR                       | VARCHAR                       |
| -------------------------- | ----------------------------- |
| Fixed length               | Variable length               |
| Pads extra spaces          | Stores only actual characters |
| Faster in some niche cases | More storage efficient        |
| Rarely used                | Most common choice            |

### CHAR (Fixed Length)

Suppose:

```sql
country_code CHAR(5)
```

You insert:

```sql
'IN'
```

PostgreSQL stores:

```text
'IN   '
```

(3 spaces added to make total length 5)

Because `CHAR(5)`  **always occupies 5 characters** .

Example:

```sql
CREATE TABLE countries (
    code CHAR(5)
);
```

Insert:

```sql
INSERT INTO countries(code)
VALUES ('IN');
```

Stored internally:

```text
IN___
```

(`_` represents spaces)

### VARCHAR (Variable Length)

Suppose:

```sql
name VARCHAR(50)
```

Insert:

```sql
'Arjun'
```

PostgreSQL stores:

```text
Arjun
```

Only the actual characters.

No padding.

Example:

```sql
CREATE TABLE users (
    name VARCHAR(50)
);
```

Insert:

```sql
INSERT INTO users(name)
VALUES ('Arjun');
```

Stored:

```text
Arjun
```

### Storage Comparison

**-- CHAR(10)**

Store:

```text
Bob
```

Actually stored:

```text
Bob_______
```

10 characters.

**-- VARCHAR(10)**

Store:

```text
Bob
```

Actually stored:

```text
Bob
```

3 characters.

### Real-world Usage

**-- Good use for CHAR**

Data with fixed length:

```sql
country_code CHAR(2)
```

Examples:

```text
IN
US
UK
```

```sql
gender CHAR(1)
```

Examples:

```text
M
F
```

**-- Good use for VARCHAR**

Almost everything else:

```sql
name VARCHAR(100)
email VARCHAR(255)
title VARCHAR(200)
```

### VARCHAR vs TEXT

In PostgreSQL, many developers don't even use VARCHAR.

They use:

```sql
name TEXT
```

because PostgreSQL handles it very efficiently.

You'll often see:

```sql
CREATE TABLE users (
    name TEXT,
    email TEXT
);
```

instead of:

```sql
CREATE TABLE users (
    name VARCHAR(100),
    email VARCHAR(255)
);
```

### Interview Answer 🎯

> `CHAR(n)` is a fixed-length string type. PostgreSQL pads spaces if the value is shorter than the specified length. `VARCHAR(n)` is a variable-length string type that stores only the actual characters entered. `VARCHAR` is generally preferred for names, emails, and most text fields, while `CHAR` is useful for fixed-size values such as country codes or status codes.

### What You'll Use in Real Projects

For a Next.js + Prisma + PostgreSQL application:

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255)
);
```

or simply:

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    name TEXT,
    email TEXT
);
```

You'll almost never need `CHAR` except for specific fixed-length fields like:

```sql
country_code CHAR(2)
currency_code CHAR(3)
```

So the practical rule is:

✅ Fixed-length data → `CHAR`
✅ Most user-entered text → `VARCHAR` or `TEXT` 🚀

---

# ----VarChar vs Text

This is actually a much better question because in  **PostgreSQL** , the difference between `VARCHAR` and `TEXT` is much smaller than many people think.

🎯 Short Answer

For PostgreSQL:

```sql
name VARCHAR(100)
```

and

```sql
name TEXT
```

have  **essentially the same performance and storage characteristics** .

The main difference is:

* `VARCHAR(n)` enforces a maximum length.
* `TEXT` has no length limit.

### Example

**VARCHAR**

```sql
CREATE TABLE users (
    name VARCHAR(10)
);
```

Valid:

```sql
INSERT INTO users(name)
VALUES ('Arjun');
```

❌ Invalid:

```sql
INSERT INTO users(name)
VALUES ('VeryLongName');
```

Error because length > 10.

**TEXT**

```sql
CREATE TABLE users (
    name TEXT
);
```

Valid:

```sql
INSERT INTO users(name)
VALUES ('Arjun');
```

Valid:

```sql
INSERT INTO users(name)
VALUES ('VeryVeryVeryLongName');
```

No length restriction.

### Common Myth

Many developers coming from other databases think:

```text
VARCHAR = Fast
TEXT = Slow
```

In PostgreSQL this is generally false.

The PostgreSQL documentation itself notes that there is **no performance advantage** to using `VARCHAR(n)` instead of `TEXT` except for the length constraint.

### When to Use VARCHAR

Use it when the length is a business rule.

Example:

**Phone Number**

```sql
phone VARCHAR(15)
```

You know a phone number shouldn't be 500 characters.

**Username**

```sql
username VARCHAR(30)
```

Your product requirements may say:

> Username max length = 30

Database can enforce that.

### When to Use TEXT

Use it when you don't care about length.

Examples:

```sql
description TEXT
bio TEXT
comment TEXT
content TEXT
```

A comment could be:

```text
Hello
```

or

```text
5000 characters...
```

No reason to set an arbitrary limit.

### What Do Modern Teams Do?

You'll see two approaches:

**Approach 1**

Everything as TEXT

```sql
CREATE TABLE users (
    name TEXT,
    email TEXT
);
```

Validation happens in application code.

Example:

```ts
if (name.length > 100) {
  throw Error("Too long");
}
```

Very common.

**Approach 2**

Business rules in database

```sql
CREATE TABLE users (
    name VARCHAR(100),
    email VARCHAR(255)
);
```

Database also enforces limits.

Also very common.

### What About Prisma?

Suppose you write:

```prisma
model User {
  name String
}
```

Prisma typically maps this to PostgreSQL:

```sql
TEXT
```

not `VARCHAR`.

Most Prisma developers don't explicitly choose `VARCHAR` unless they need a length constraint.

### Interview Answer 🎯

> In PostgreSQL, `VARCHAR(n)` and `TEXT` have similar storage and performance characteristics. The primary difference is that `VARCHAR(n)` enforces a maximum length, while `TEXT` allows strings of any length. `VARCHAR` is useful when the length is a business requirement, whereas `TEXT` is often used when no explicit limit is needed.

---

# ----Keys and their types

This is one of the most important PostgreSQL/SQL interview topics. Almost every backend interview will ask about  **Primary Keys** ,  **Foreign Keys** , and sometimes  **Candidate Keys** ,  **Composite Keys** , and  **Unique Keys** .

### 🔑 What is a Key?

A key is a column (or set of columns) used to:

* Identify records uniquely
* Create relationships between tables
* Prevent duplicate data
* Improve data integrity

Think of a database as a company.

Employees may have the same name:

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Arjun |

How do we distinguish them?

Using a unique identifier:

| employee_id | name  |
| ----------- | ----- |
| 1001        | Arjun |
| 1002        | Arjun |

That identifier is a key.

### 🎯 Primary Key (PK)

A Primary Key uniquely identifies every row.

Properties:

✅ Unique

✅ Cannot be NULL

✅ One primary key per table

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

Data:

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Rahul |

##### What happens if duplicate?

```sql
INSERT INTO users(id, name)
VALUES (1, 'John');
```

❌ Error

Because id=1 already exists.

##### What happens if NULL?

```sql
INSERT INTO users(id, name)
VALUES (NULL, 'John');
```

❌ Error

Primary keys cannot be NULL.

##### 🏗 How PostgreSQL Implements Primary Keys

When you write:

```sql
id SERIAL PRIMARY KEY
```

PostgreSQL internally creates:

**Unique Constraint**

```sql
UNIQUE(id)
```

**Not Null Constraint**

```sql
NOT NULL
```

**Unique Index**

A special data structure for fast lookup.

Conceptually:

```text
1 -> Row A
2 -> Row B
3 -> Row C
```

Instead of scanning the whole table.

### 🔑 Unique Key

Ensures uniqueness.

Unlike Primary Key:

* Multiple unique keys allowed
* Can contain NULL values

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email TEXT UNIQUE
);
```

Valid:

| id | email                          |
| -- | ------------------------------ |
| 1  | [a@gmail.com](mailto:a@gmail.com) |
| 2  | [b@gmail.com](mailto:b@gmail.com) |

Invalid:

| id | email                          |
| -- | ------------------------------ |
| 1  | [a@gmail.com](mailto:a@gmail.com) |
| 2  | [a@gmail.com](mailto:a@gmail.com) |

❌ Duplicate email

### 🔗 Foreign Key (FK)

Creates relationships between tables.

Suppose:

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

Orders

| id  | user_id |
| --- | ------- |
| 101 | 1       |

`user_id` points to Users table.

Implementation:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id)
);
```

Equivalent to:

```sql
FOREIGN KEY(user_id)
REFERENCES users(id)
```

##### Why Foreign Keys Matter

Without FK:

```sql
user_id = 999999
```

Even if user doesn't exist.

Database allows bad data.

With FK:

```sql
INSERT INTO orders(user_id)
VALUES (999999);
```

❌ Error

Because user doesn't exist.

##### PostgreSQL Internally

When inserting:

```sql
INSERT INTO orders(user_id)
VALUES (5);
```

PostgreSQL checks:

```text
Does users.id = 5 exist?
```

If yes:

✅ Insert

Else:

❌ Reject

### One-to-Many Relationship

Most common relationship.

One user.

Many orders.

```text
User
 ├─ Order 1
 ├─ Order 2
 └─ Order 3
```

Schema:

```sql
users
```

| id |
| -- |
| 1  |

```sql
orders
```

| id | user_id |
| -- | ------- |
| 1  | 1       |
| 2  | 1       |
| 3  | 1       |

### Many-to-Many Relationship

Example:

Users ↔ Courses

One user can enroll in many courses.

One course can have many users.

Create junction table:

```sql
CREATE TABLE enrollments (
    user_id INTEGER,
    course_id INTEGER,
    PRIMARY KEY(user_id, course_id)
);
```

Data:

| user_id | course_id |
| ------- | --------- |
| 1       | 101       |
| 1       | 102       |
| 2       | 101       |

### Composite Key

Primary key made from multiple columns.

Example:

```sql
PRIMARY KEY(user_id, course_id)
```

Combination must be unique.

Valid:

| user_id | course_id |
| ------- | --------- |
| 1       | 101       |
| 1       | 102       |

Invalid:

| user_id | course_id |
| ------- | --------- |
| 1       | 101       |
| 1       | 101       |

❌ Duplicate combination

### Candidate Key

A column that *could* become a primary key.

Example:

| id | email                          |
| -- | ------------------------------ |
| 1  | [a@gmail.com](mailto:a@gmail.com) |

Both:

```text
id
email
```

are unique.

Either could be chosen.

Candidate keys = possible primary keys.

### Alternate Key

Candidate key not chosen as primary.

Example:

```sql
id PRIMARY KEY
email UNIQUE
```

Here:

* id → Primary Key
* email → Alternate Key

### Super Key

Any set of columns that uniquely identifies rows.

Example:

```text
{id}
{id,email}
{id,name,email}
```

All unique.

All are super keys.

### Natural Key

Real-world data used as key.

Examples:

```text
Passport Number
Driving License Number
SSN
Email
```

Example:

```sql
email TEXT PRIMARY KEY
```

Possible, but usually avoided.

### Surrogate Key

Artificially created key.

Example:

```sql
id SERIAL PRIMARY KEY
```

or

```sql
id UUID PRIMARY KEY
```

Most modern applications use surrogate keys.

### SERIAL vs UUID

**-- SERIAL**

```sql
id SERIAL PRIMARY KEY
```

Produces:

```text
1
2
3
4
```

Advantages:

* Small
* Fast
* Easy to read

Disadvantages:

* Predictable

Users can guess:

```text
/api/users/1
/api/users/2
/api/users/3
```

**-- UUID**

```sql
id UUID PRIMARY KEY
```

Example:

```text
550e8400-e29b-41d4-a716-446655440000
```

Advantages:

* Hard to guess
* Great for distributed systems

Disadvantages:

* Larger
* Harder to read

### Prisma Example

PostgreSQL:

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    email TEXT UNIQUE
);
```

Prisma:

```prisma
model User {
  id    String @id @default(uuid())
  email String @unique
}
```

### Interview Cheat Sheet 🎯

| Key           | Purpose                         |
| ------------- | ------------------------------- |
| Primary Key   | Unique row identifier           |
| Foreign Key   | Creates table relationships     |
| Unique Key    | Prevent duplicates              |
| Composite Key | Multiple columns as key         |
| Candidate Key | Potential primary key           |
| Alternate Key | Candidate key not selected      |
| Super Key     | Any unique column combination   |
| Natural Key   | Real-world identifier           |
| Surrogate Key | Artificial identifier (ID/UUID) |

### Most Important for Interviews

Focus heavily on:

✅ Primary Key

✅ Foreign Key

✅ Unique Key

✅ Composite Key

✅ UUID vs SERIAL

If you can explain those with examples and why they exist, you'll answer about 90% of SQL/PostgreSQL key-related interview questions correctly. 🚀

---

# ----Creating Tables & Inserting values

### 🏗️ CREATE TABLE

Used to create a new table in the database.

Basic syntax:

```sql
CREATE TABLE table_name (
    column_name data_type constraints
);
```

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255)
);
```

This creates a table named `users`.

##### Breaking It Down

```sql
id SERIAL PRIMARY KEY
```

**`id`**

Column name.

**`SERIAL`**

Auto-incrementing integer.

PostgreSQL generates:

```text
1
2
3
4
...
```

automatically.

**`PRIMARY KEY`**

Makes `id`:

* Unique
* Not NULL
* Indexed

```sql
name VARCHAR(100)
```

Means:

```text
Column: name
Type: String
Max Length: 100
```

Examples:

```text
Arjun
Rahul
John
```

```sql
email VARCHAR(255)
```

Stores emails.

Example:

```text
arjun@gmail.com
```

##### Creating a More Realistic Table

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

##### What Each Part Means

**NOT NULL**

```sql
name VARCHAR(100) NOT NULL
```

Cannot be empty.

Valid:

```text
Arjun
```

Invalid:

```text
NULL
```

**UNIQUE**

```sql
email VARCHAR(255) UNIQUE
```

No duplicates allowed.

Valid:

```text
a@gmail.com
b@gmail.com
```

Invalid:

```text
a@gmail.com
a@gmail.com
```

**DEFAULT**

```sql
is_active BOOLEAN DEFAULT TRUE
```

If no value supplied:

```text
TRUE
```

gets inserted automatically.

**CURRENT_TIMESTAMP**

```sql
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

Automatically stores creation time.

Example:

```text
2026-06-13 10:45:22
```

### 📥 INSERT

Used to add records (rows).

Syntax:

```sql
INSERT INTO table_name(columns)
VALUES(values);
```

Example 1

```sql
INSERT INTO users(name, email)
VALUES ('Arjun', 'arjun@gmail.com');
```

What happens?

PostgreSQL creates:

| id | name  | email                                  |
| -- | ----- | -------------------------------------- |
| 1  | Arjun | [arjun@gmail.com](mailto:arjun@gmail.com) |

Notice:

We didn't provide:

```sql
id
```

because SERIAL generated it automatically.

# Example 2

Insert Full Record

```sql
INSERT INTO users(
    name,
    email,
    age,
    is_active
)
VALUES (
    'Rahul',
    'rahul@gmail.com',
    25,
    TRUE
);
```

Result:

| id | name  | email                                  | age | is_active |
| -- | ----- | -------------------------------------- | --- | --------- |
| 2  | Rahul | [rahul@gmail.com](mailto:rahul@gmail.com) | 25  | TRUE      |

##### Column Order Matters

Correct:

```sql
INSERT INTO users(name, age)
VALUES ('Arjun', 24);
```

Meaning:

```text
name = Arjun
age = 24
```

Wrong:

```sql
INSERT INTO users(name, age)
VALUES (24, 'Arjun');
```

PostgreSQL tries:

```text
name = 24
age = Arjun
```

and throws an error.

##### Inserting Without Specifying Columns

Possible:

```sql
INSERT INTO users
VALUES (
    1,
    'Arjun',
    'arjun@gmail.com',
    24,
    TRUE
);
```

But not recommended.

Why?

Because if table structure changes later:

```sql
ALTER TABLE users ADD COLUMN phone TEXT;
```

old inserts break.

Professionals almost always specify column names.

##### Insert Multiple Rows

```sql
INSERT INTO users(name, email)
VALUES
('Arjun', 'a@gmail.com'),
('Rahul', 'r@gmail.com'),
('John', 'j@gmail.com');
```

Result:

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Rahul |
| 3  | John  |

##### Insert Using DEFAULT

Suppose:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    is_active BOOLEAN DEFAULT TRUE
);
```

Insert:

```sql
INSERT INTO users(name)
VALUES ('Arjun');
```

Result:

| id | name  | is_active |
| -- | ----- | --------- |
| 1  | Arjun | TRUE      |

PostgreSQL automatically applies default value.

##### Returning Inserted Data

Very common in backend development.

```sql
INSERT INTO users(name,email)
VALUES(
    'Arjun',
    'arjun@gmail.com'
)
RETURNING *;
```

Returns:

| id | name  | email                                  |
| -- | ----- | -------------------------------------- |
| 1  | Arjun | [arjun@gmail.com](mailto:arjun@gmail.com) |

Useful when creating APIs.

##### Example with Foreign Keys

Users:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

Orders:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    amount NUMERIC(10,2)
);
```

Insert user:

```sql
INSERT INTO users(name)
VALUES ('Arjun');
```

Creates:

```text
id = 1
```

Insert order:

```sql
INSERT INTO orders(user_id, amount)
VALUES (1, 199.99);
```

Valid because user 1 exists.

Invalid:

```sql
INSERT INTO orders(user_id, amount)
VALUES (999, 199.99);
```

❌ Foreign key violation.

### Common Constraints During CREATE TABLE

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email TEXT UNIQUE,
    name TEXT NOT NULL,
    age INTEGER CHECK(age >= 18),
    is_active BOOLEAN DEFAULT TRUE
);
```

##### CHECK

```sql
age INTEGER CHECK(age >= 18)
```

Valid:

```text
20
25
30
```

Invalid:

```text
15
```

PostgreSQL rejects it.

### Interview-Level Example

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

Insert:

```sql
INSERT INTO users(
    id,
    name,
    email
)
VALUES(
    '550e8400-e29b-41d4-a716-446655440000',
    'Arjun',
    'arjun@gmail.com'
);
```

---

# ----Select & Update

If you understand **SELECT** and **UPDATE** deeply, you've covered a huge portion of real-world backend work. Most interview SQL questions revolve around `SELECT`, and many production bugs come from bad `UPDATE` statements. 😄

Let's use this table throughout:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT,
    age INTEGER,
    salary NUMERIC(10,2),
    is_active BOOLEAN
);
```

Data:

| id | name  | age | salary | is_active |
| -- | ----- | --- | ------ | --------- |
| 1  | Arjun | 24  | 50000  | TRUE      |
| 2  | Rahul | 30  | 70000  | TRUE      |
| 3  | John  | 19  | 30000  | FALSE     |

### 🔍 SELECT

Used to retrieve data.

##### Select All

```sql
SELECT * FROM users;
```

`*` means all columns.

Result:

| id | name  | age | salary |
| -- | ----- | --- | ------ |
| 1  | Arjun | 24  | 50000  |
| 2  | Rahul | 30  | 70000  |
| 3  | John  | 19  | 30000  |

##### Select Specific Columns

```sql
SELECT name, age
FROM users;
```

Result:

| name  | age |
| ----- | --- |
| Arjun | 24  |
| Rahul | 30  |
| John  | 19  |

##### WHERE

Filters rows.

```sql
SELECT *
FROM users
WHERE age > 25;
```

Result:

| name  |
| ----- |
| Rahul |

##### Comparison Operators

```sql
=
!=
<>
>
<
>=
<=
```

Example:

```sql
SELECT *
FROM users
WHERE age >= 24;
```

##### AND / OR

**-- AND**

Both conditions must be true.

```sql
SELECT *
FROM users
WHERE age > 20
AND salary > 60000;
```

Result:

Rahul

**-- OR**

At least one condition.

```sql
SELECT *
FROM users
WHERE age > 25
OR salary > 60000;
```

##### IN

Instead of multiple ORs.

Bad:

```sql
WHERE age = 19
OR age = 24
OR age = 30
```

Better:

```sql
WHERE age IN (19,24,30)
```

##### BETWEEN

```sql
SELECT *
FROM users
WHERE age BETWEEN 20 AND 30;
```

Includes both ends.

Equivalent:

```sql
WHERE age >= 20
AND age <= 30
```

##### LIKE

Pattern matching.

**--Starts With**

```sql
WHERE name LIKE 'A%'
```

Matches:

```text
Arjun
Alex
Anand
```

**--Ends With**

```sql
WHERE name LIKE '%n'
```

Matches:

```text
John
Arjun
```

##### Contains

```sql
WHERE name LIKE '%jun%'
```

Matches:

```text
Arjun
```

##### ILIKE (PostgreSQL Specific)

Case-insensitive.

```sql
WHERE name ILIKE 'arjun'
```

Matches:

```text
Arjun
ARJUN
arjun
```

Interviewers love this PostgreSQL-specific question.

##### ORDER BY

Sort results.

Ascending:

```sql
SELECT *
FROM users
ORDER BY salary;
```

or

```sql
ORDER BY salary ASC;
```

Descending:

```sql
ORDER BY salary DESC;
```

Highest salary first.

##### LIMIT

Return only N rows.

```sql
SELECT *
FROM users
LIMIT 2;
```

Returns first 2 rows.

##### OFFSET

Skip rows.

```sql
SELECT *
FROM users
LIMIT 10 OFFSET 20;
```

Pagination:

```text
Skip 20
Take 10
```

Used heavily in APIs.

##### DISTINCT

Remove duplicates.

Data:

| age |
| --- |
| 24  |
| 24  |
| 30  |

```sql
SELECT DISTINCT age
FROM users;
```

Result:

```text
24
30
```

##### Aliases

```sql
SELECT name AS username
FROM users;
```

Result column becomes:

```text
username
```

Table alias:

```sql
SELECT u.name
FROM users u;
```

Same as:

```sql
SELECT users.name
FROM users;
```

##### NULL (VERY IMPORTANT)

Many interview candidates get this wrong.

Suppose:

| name  | age  |
| ----- | ---- |
| Arjun | 24   |
| Rahul | NULL |

Wrong:

```sql
WHERE age = NULL
```

Never works.

Correct:

```sql
WHERE age IS NULL
```

Not null:

```sql
WHERE age IS NOT NULL
```

Interview Question:

Why doesn't this work?

```sql
SELECT *
FROM users
WHERE age = NULL;
```

Because NULL means:

> Unknown

And unknown cannot be compared using `=`.

##### SELECT Execution Order (Interview Favorite)

Query:

```sql
SELECT name
FROM users
WHERE age > 20
ORDER BY salary DESC
LIMIT 5;
```

Logical execution:

```text
FROM
WHERE
SELECT
ORDER BY
LIMIT
```

Not the order you write.

Many interviewers ask this.

### UPDATE

Used to modify existing rows.

Basic Update

```sql
UPDATE users
SET salary = 60000
WHERE id = 1;
```

Result:

| id | salary |
| -- | ------ |
| 1  | 60000  |

##### MOST DANGEROUS SQL MISTAKE

```sql
UPDATE users
SET salary = 60000;
```

No WHERE clause.

Result:

| id | salary |
| -- | ------ |
| 1  | 60000  |
| 2  | 60000  |
| 3  | 60000  |

Every row updated.

Many developers have done this in production 😅

**Interview Question:**

What happens if WHERE is omitted?

Answer:

> Every row in the table gets updated.

##### Update Multiple Columns

```sql
UPDATE users
SET
    age = 25,
    salary = 55000
WHERE id = 1;
```

##### Update Based on Existing Value

Increase salary by 10%.

```sql
UPDATE users
SET salary = salary * 1.10
WHERE id = 1;
```

Notice:

```sql
salary = salary * 1.10
```

The right side uses the old value.

##### RETURNING

PostgreSQL feature.

```sql
UPDATE users
SET salary = salary + 5000
WHERE id = 1
RETURNING *;
```

Returns updated row immediately.

Extremely common in APIs.

##### Conditional Update

```sql
UPDATE users
SET is_active = FALSE
WHERE age < 18;
```

##### Update with IN

```sql
UPDATE users
SET salary = salary + 5000
WHERE id IN (1,2,3);
```

##### Update with Subquery

Advanced interview question.

Suppose:

```sql
employees
```

| id | department_id |
| -- | ------------- |
| 1  | 10            |

```sql
departments
```

| id | bonus |
| -- | ----- |
| 10 | 5000  |

Update employee bonus:

```sql
UPDATE employees
SET bonus = (
    SELECT bonus
    FROM departments
    WHERE departments.id =
          employees.department_id
);
```

### Common Interview Traps

##### Trap 1

```sql
SELECT *
FROM users
WHERE name = 'arjun';
```

Won't match:

```text
Arjun
```

Because PostgreSQL string comparisons are case-sensitive.

Use:

```sql
ILIKE
```

##### Trap 2

```sql
UPDATE users
SET salary = salary++;
```

Invalid SQL.

Correct:

```sql
UPDATE users
SET salary = salary + 1;
```

##### Trap 3

```sql
SELECT *
FROM users
LIMIT 10;
```

Without:

```sql
ORDER BY
```

Result order is not guaranteed.

Many candidates don't know this.

### Real Backend Examples

Get user by email:

```sql
SELECT *
FROM users
WHERE email = 'arjun@gmail.com';
```

Login lookup:

```sql
SELECT *
FROM users
WHERE email = 'arjun@gmail.com'
AND is_active = TRUE;
```

Update profile:

```sql
UPDATE users
SET
    name = 'Arjun Suresh',
    age = 24
WHERE id = 1
RETURNING *;
```

### High-Frequency Interview Questions 🎯

Q1 Difference between:

```sql
WHERE age = NULL
```

and

```sql
WHERE age IS NULL
```

Q2 What happens if UPDATE has no WHERE clause?

Q3 Why use aliases like `u.name`?

Q4 Difference between `LIKE` and `ILIKE`?

Q5 Difference between:

```sql
SELECT DISTINCT age
```

and

```sql
SELECT age
```

Q6 What is the execution order of:

```sql
SELECT
FROM
WHERE
ORDER BY
LIMIT
```

(Logically: FROM → WHERE → SELECT → ORDER BY → LIMIT)

---

# ----Delete, Drop & Truncate

### 🗑️ DELETE

`DELETE` is used to remove rows from a table.

Basic syntax:

```sql
DELETE FROM table_name
WHERE condition;
```

Think of it as:

```text
Find rows matching condition
↓
Remove them permanently
```

**📋 Sample Table**

We'll use:

| id | name  | age | is_active |
| -- | ----- | --- | --------- |
| 1  | Arjun | 24  | TRUE      |
| 2  | Rahul | 30  | TRUE      |
| 3  | John  | 19  | FALSE     |
| 4  | Alex  | 22  | FALSE     |

##### 🎯 Delete One Row

```sql
DELETE FROM users
WHERE id = 1;
```

Before:

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Rahul |

After:

| id | name  |
| -- | ----- |
| 2  | Rahul |

##### 🚨 Most Dangerous SQL Mistake

```sql
DELETE FROM users;
```

Notice:

```sql
WHERE
```

is missing.

Result:

❌ Every row is deleted.

Before:

| id |
| -- |
| 1  |
| 2  |
| 3  |

After:

```text
Empty Table
```

💼 Interview Question

❓ What happens if DELETE has no WHERE clause?

Answer:

> All rows in the table are deleted.

##### 🔍 Delete Using Multiple Conditions

```sql
DELETE FROM users
WHERE age < 20
AND is_active = FALSE;
```

Deletes:

| id | name |
| -- | ---- |
| 3  | John |

##### 📦 Delete Using IN

Instead of:

```sql
DELETE FROM users
WHERE id = 1
OR id = 2
OR id = 3;
```

Use:

```sql
DELETE FROM users
WHERE id IN (1,2,3);
```

Cleaner.

##### 📊 Delete Using BETWEEN

```sql
DELETE FROM users
WHERE age BETWEEN 18 AND 25;
```

Deletes users aged 18–25 inclusive.

##### 🔤 Delete Using LIKE

```sql
DELETE FROM users
WHERE name LIKE 'A%';
```

Deletes:

```text
Arjun
Alex
Anand
```

##### 🔥 PostgreSQL-Specific RETURNING

One of the coolest PostgreSQL features.

```sql
DELETE FROM users
WHERE id = 1
RETURNING *;
```

Deletes row and returns:

| id | name  |
| -- | ----- |
| 1  | Arjun |

Useful in APIs.

Example:

```js
const deletedUser = await prisma.user.delete(...)
```

Under the hood, PostgreSQL often does something similar.

### ⚡ Delete vs Drop

Very common interview question.

##### 🗑️ DELETE

Removes rows.

Keeps table structure.

```sql
DELETE FROM users;
```

After:

```text
users table still exists
```

##### 💣 DROP

Removes entire table.

```sql
DROP TABLE users;
```

After:

```text
users table no longer exists
```

📊 Example

Before:

```text
users
```

| id | name  |
| -- | ----- |
| 1  | Arjun |

After:

```sql
DELETE FROM users;
```

Table exists:

```text
users
```

but empty.

After:

```sql
DROP TABLE users;
```

Table itself is gone.

### ⚡ DELETE vs TRUNCATE

Extremely common interview question.

**🗑️ DELETE**

```sql
DELETE FROM users;
```

Removes rows one by one.

Can use WHERE.

```sql
DELETE FROM users
WHERE age > 25;
```

**🚀 TRUNCATE**

```sql
TRUNCATE TABLE users;
```

Removes all rows instantly.

Cannot filter.

No WHERE.

**💼 Interview Question**

❓ When would you use TRUNCATE instead of DELETE?

Answer:

> When I need to remove all rows from a table and don't need filtering. TRUNCATE is usually much faster.

### 🔑 Foreign Key Problems

Suppose:

users

| id |
| -- |
| 1  |

orders

| id  | user_id |
| --- | ------- |
| 100 | 1       |

Foreign key:

```sql
FOREIGN KEY(user_id)
REFERENCES users(id)
```

Now:

```sql
DELETE FROM users
WHERE id = 1;
```

PostgreSQL says:

❌ Cannot delete.

Because orders still reference user 1.

##### 🔗 ON DELETE CASCADE

Very important.

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER
        REFERENCES users(id)
        ON DELETE CASCADE
);
```

Now:

```sql
DELETE FROM users
WHERE id = 1;
```

PostgreSQL automatically deletes:

```text
User
↓
All related orders
```

**💼 Interview Question**

❓ What is ON DELETE CASCADE?

Answer:

> When a parent row is deleted, PostgreSQL automatically deletes all child rows referencing it.

##### 🚫 ON DELETE RESTRICT

Default behavior.

```sql
REFERENCES users(id)
ON DELETE RESTRICT
```

Now:

```sql
DELETE FROM users
WHERE id = 1;
```

❌ Error if related orders exist.

##### 🔄 ON DELETE SET NULL

```sql
REFERENCES users(id)
ON DELETE SET NULL
```

Before:

| id  | user_id |
| --- | ------- |
| 100 | 1       |

Delete user:

```sql
DELETE FROM users
WHERE id = 1;
```

After:

| id  | user_id |
| --- | ------- |
| 100 | NULL    |

Order survives.

Reference removed.

### 🎯 Deleting with Subqueries

Advanced interview topic.

Delete inactive users:

```sql
DELETE FROM users
WHERE id IN (
    SELECT id
    FROM users
    WHERE is_active = FALSE
);
```

### 📈 EXISTS Example

```sql
DELETE FROM users u
WHERE EXISTS (
    SELECT 1
    FROM banned_users b
    WHERE b.email = u.email
);
```

Delete users appearing in banned list.

### ⚠️ NULL Trap

Suppose:

| id | age  |
| -- | ---- |
| 1  | NULL |

Wrong:

```sql
DELETE FROM users
WHERE age = NULL;
```

Deletes nothing.

Correct:

```sql
DELETE FROM users
WHERE age IS NULL;
```

### 🔒 Safe Production Practice

Before deleting:

Step 1

Run SELECT

```sql
SELECT *
FROM users
WHERE age < 18;
```

Verify rows.

Step 2

Run DELETE

```sql
DELETE FROM users
WHERE age < 18;
```

Many senior developers do exactly this.

---

# ----JOINS & Its Types

### 🔗 What is a JOIN?

A JOIN combines data from multiple tables based on a relationship.

Imagine:

👤 Users

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Rahul |
| 3  | John  |

📦 Orders

| id  | user_id | product  |
| --- | ------- | -------- |
| 101 | 1       | Laptop   |
| 102 | 1       | Mouse    |
| 103 | 2       | Keyboard |

Notice:

```text
orders.user_id → users.id
```

Foreign Key relationship.

### 🤔 Why Do We Need JOINs?

Suppose you want:

```text
Arjun bought Laptop
Arjun bought Mouse
Rahul bought Keyboard
```

Users table doesn't contain products.

Orders table doesn't contain names.

You need data from both tables.

That's exactly what JOINs do.

### 🎯 INNER JOIN

Most common interview question.

📌 Syntax

```sql
SELECT *
FROM users
INNER JOIN orders
ON users.id = orders.user_id;
```

🔍 How It Works

PostgreSQL checks:

```text
users.id == orders.user_id
```

and combines matching rows.

📊 Result

| name  | product  |
| ----- | -------- |
| Arjun | Laptop   |
| Arjun | Mouse    |
| Rahul | Keyboard |

Notice:

```text
John disappeared
```

because he has no orders.

**🧠 Mental Model**

INNER JOIN means:

```text
Give me only matching rows
```

Visualize:

```text
Users      Orders

  ○────○
```

Only overlapping data survives.

**💼 Interview Question**

❓ What does INNER JOIN return?

Answer:

> Only rows that have matching values in both tables.

> #### ⚠️ Inner join can also be called as join
>
> When SQL says simply:
>
> ```sql
> SELECT
>     u.name,
>     o.product
> FROM users u
> JOIN orders o
> ON u.id = o.user_id;
> ```
>
> it means:
>
> ```sql
> SELECT
>     u.name,
>     o.product
> FROM users u
> INNER JOIN orders o
> ON u.id = o.user_id;
> ```
>
> ✅ `JOIN` = `INNER JOIN`
>
> They are exactly the same.
>
> #### 📌 Why Does SQL Allow This?
>
> Because  **INNER JOIN is by far the most commonly used JOIN** .
>
> So SQL provides a shorthand:
>
> ```sql
> JOIN
> ```
>
> instead of writing:
>
> ```sql
> INNER JOIN
> ```
>
> every time.

### 🔗 LEFT JOIN

Very important.

📌 Syntax

```sql
SELECT *
FROM users
LEFT JOIN orders
ON users.id = orders.user_id;
```

📊 Result

| name  | product  |
| ----- | -------- |
| Arjun | Laptop   |
| Arjun | Mouse    |
| Rahul | Keyboard |
| John  | NULL     |

Notice:

```text
John appears
```

even though he has no orders.

**🧠 Mental Model**

LEFT JOIN means:

```text
Keep everything from left table
```

Visualize:

```text
Users ← Keep all
Orders ← Optional
```

**💼 Interview Question**

❓ Difference Between INNER JOIN and LEFT JOIN?

INNER JOIN:

```text
Only matching rows
```

LEFT JOIN:

```text
All rows from left table
+
Matching rows from right table
```

### 🔗 RIGHT JOIN

Opposite of LEFT JOIN.

```sql
SELECT *
FROM users
RIGHT JOIN orders
ON users.id = orders.user_id;
```

Means:

```text
Keep all rows from orders
```

Even if user missing.

In real projects:

```text
LEFT JOIN used a lot
RIGHT JOIN rarely used
```

Many companies almost never use RIGHT JOIN.

### 🔗 FULL JOIN

Returns:

```text
All rows from both tables
```

Matching where possible.

NULL where no match.

Example:

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |
| 2  | Rahul |

Orders

| id  | user_id |
| --- | ------- |
| 101 | 1       |
| 102 | 99      |

FULL JOIN result:

| name  | user_id |
| ----- | ------- |
| Arjun | 1       |
| Rahul | NULL    |
| NULL  | 99      |

### 🎯 JOIN Condition

Most important part:

```sql
ON users.id = orders.user_id
```

Without it:

```sql
SELECT *
FROM users
JOIN orders;
```

❌ Error.

PostgreSQL doesn't know how to match rows.

### ⚠️ Duplicate Rows

Interview favorite.

Data:

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

Orders

| id  | user_id |
| --- | ------- |
| 101 | 1       |
| 102 | 1       |

Query:

```sql
SELECT *
FROM users
JOIN orders
ON users.id = orders.user_id;
```

Result:

| name  | order |
| ----- | ----- |
| Arjun | 101   |
| Arjun | 102   |

People often ask:

> Why is Arjun appearing twice?

Because:

```text
1 user
2 matching orders
```

One row produced per match.

### 🔥 Aliases (Very Important)

Instead of:

```sql
SELECT users.name,
       orders.product
FROM users
JOIN orders
ON users.id = orders.user_id;
```

Use:

```sql
SELECT u.name,
       o.product
FROM users u
JOIN orders o
ON u.id = o.user_id;
```

Cleaner.

Professional SQL always uses aliases.

### 🚨 Ambiguous Column Error

Suppose:

Users

```text
id
name
```

Orders

```text
id
user_id
```

Now:

```sql
SELECT id
FROM users
JOIN orders
ON users.id = orders.user_id;
```

❌ Error

Which id?

```text
users.id ?
orders.id ?
```

Correct:

```sql
SELECT users.id
```

or

```sql
SELECT u.id
```

### 🔍 Finding Missing Data

Extremely common.

**Users without Orders**

```sql
SELECT *
FROM users u
LEFT JOIN orders o
ON u.id = o.user_id
WHERE o.id IS NULL;
```

Result:

```text
Users who never ordered
```

Interviewers love this one.

### 🎯 Self Join

Joining table with itself.

Example:

### Employees

| id | name      | manager_id |
| -- | --------- | ---------- |
| 1  | CEO       | NULL       |
| 2  | Manager   | 1          |
| 3  | Developer | 2          |

Query:

```sql
SELECT
    e.name,
    m.name AS manager_name
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id;
```

Result:

| Employee  | Manager |
| --------- | ------- |
| Manager   | CEO     |
| Developer | Manager |

### 🔥 Multiple Joins

Real-world example:

Users

Orders

Products

```sql
SELECT
    u.name,
    p.product_name
FROM users u
JOIN orders o
ON u.id = o.user_id
JOIN products p
ON p.id = o.product_id;
```

This is very common.

### ⚡ JOIN Execution Order

Query:

```sql
SELECT u.name
FROM users u
JOIN orders o
ON u.id = o.user_id
WHERE u.age > 20;
```

Logical order:

```text
FROM
JOIN
ON
WHERE
SELECT
```

Not the order written.

Interview favorite.

### 🎯 Cross Join

Every row matched with every row.

Users

| id |
| -- |
| 1  |
| 2  |

Products

| id |
| -- |
| A  |
| B  |

```sql
SELECT *
FROM users
CROSS JOIN products;
```

Result:

| User | Product |
| ---- | ------- |
| 1    | A       |
| 1    | B       |
| 2    | A       |
| 2    | B       |

Total:

```text
rows_users × rows_products
```

Usually dangerous.

Can explode into millions of rows.

### 🚨 Common JOIN Mistakes

**❌ Wrong Join Condition**

```sql
ON users.id = orders.id
```

instead of

```sql
ON users.id = orders.user_id
```

Returns wrong results.

**❌ Missing ON Clause**

```sql
JOIN orders
```

without ON.

Usually error or cross join behavior.

**❌ Selecting ***

```sql
SELECT *
```

on large joins.

Returns many unnecessary columns.

Prefer:

```sql
SELECT
    u.name,
    o.product
```

**❌ Forgetting NULL Handling**

```sql
LEFT JOIN ...
WHERE orders.id IS NULL
```

very useful.

Many candidates miss it.

### 💼 Real Backend Examples

👤 Get User's Orders

```sql
SELECT
    u.name,
    o.product
FROM users u
JOIN orders o
ON u.id = o.user_id;
```

❤️ Get Wishlist Products

```sql
SELECT
    p.name
FROM wishlist w
JOIN products p
ON p.id = w.product_id;
```

---

# ----Relationship & Its Types

### 🔗 What Are Relationships?

Relationships define how tables are connected to each other.

Think about a real application:

```text
User
↓
Orders
↓
Products
```

These entities are not isolated.

They are related.

PostgreSQL uses:

```text
Primary Keys
+
Foreign Keys
```

to create these relationships.

### 🎯 Why Do We Need Relationships?

Without relationships:

👤 Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

📦 Orders

| id  | user_name |
| --- | --------- |
| 101 | Arjun     |

Looks fine initially.

Imagine Arjun changes his name:

```text
Arjun → Arjun Suresh
```

Now:

Users

| id | name         |
| -- | ------------ |
| 1  | Arjun Suresh |

Orders

| id  | user_name |
| --- | --------- |
| 101 | Arjun     |

Data becomes inconsistent.

Better:

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

Orders

| id  | user_id |
| --- | ------- |
| 101 | 1       |

Now Orders reference Users through ID.

Much safer.

### 👤 One-to-One (1:1)

One record relates to exactly one record.

📋 Example

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

User Profiles

| id | user_id | bio       |
| -- | ------- | --------- |
| 1  | 1       | Developer |

One user.

One profile.

##### 🏗️ Implementation

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

```sql
CREATE TABLE profiles (
    id SERIAL PRIMARY KEY,
    user_id INTEGER UNIQUE,
    bio TEXT,
    FOREIGN KEY(user_id)
        REFERENCES users(id)
);
```

🤔 Why UNIQUE?

Without:

```sql
UNIQUE
```

this becomes:

```text
One User
↓
Many Profiles
```

which is not one-to-one anymore.

> #### 🎯 Syntaxes for Foreign Key
>
> **🔹 Inline Syntax**
>
> ```sql
> CREATE TABLE orders (
>     id SERIAL PRIMARY KEY,
>     user_id INTEGER REFERENCES users(id)
> );
> ```
>
> This is called:
>
> ```text
> Inline Constraint Definition
> ```
>
> **🔹 Table-Level Syntax**
>
> ```sql
> CREATE TABLE orders (
>     id SERIAL PRIMARY KEY,
>     user_id INTEGER,
>
>     FOREIGN KEY(user_id)
>         REFERENCES users(id)
> );
> ```
>
> This is called:
>
> ```text
> Table-Level Constraint Definition
> ```
>
> #### 🎯 Are They Equivalent?
>
> Yes.
>
> These are identical:
>
> Version 1
>
> ```sql
> user_id INTEGER REFERENCES users(id)
> ```
>
> Version 2
>
> ```sql
> user_id INTEGER,
>
> FOREIGN KEY(user_id)
> REFERENCES users(id)
> ```
>
> Both create the same foreign key.
>
> ##### 🤔 Then Why Use Table-Level Constraints?
>
> Because they're more flexible.
>
> Example:
>
> ```sql
> CREATE TABLE orders (
>     id SERIAL PRIMARY KEY,
>     user_id INTEGER,
>
>     FOREIGN KEY(user_id)
>         REFERENCES users(id)
>         ON DELETE CASCADE
> );
> ```
>
> Much easier to read.

##### 💼 Real Examples

User ↔ Profile

```text
1 User
1 Profile
```

User ↔ Passport

```text
1 Person
1 Passport
```

User ↔ Settings

```text
1 User
1 Settings Record
```

##### 🔥 Interview Question

❓ How do you enforce One-to-One?

Answer:

> By making the foreign key UNIQUE.

### 👥 One-to-Many (1:N)

Most common relationship in backend development.

📋 Example

Users

| id | name  |
| -- | ----- |
| 1  | Arjun |

Orders

| id  | user_id |
| --- | ------- |
| 101 | 1       |
| 102 | 1       |
| 103 | 1       |

One user.

Many orders.

##### 🏗️ Implementation

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER,
    FOREIGN KEY(user_id)
        REFERENCES users(id)
);
```

Notice:

```text
No UNIQUE
```

Multiple rows can reference same user.

##### 💼 Real Examples

User → Orders

```text
1 User
Many Orders
```

User → Posts

```text
1 User
Many Posts
```

##### 🎯 Most Important Relationship

In real projects:

```text
90%+
```

of relationships are One-to-Many.

### 🤝 Many-to-Many (M:N)

The most confusing relationship for beginners.

📋 Example

Students and Courses.

Student

| id | name  |
| -- | ----- |
| 1  | Arjun |

Course

| id | course |
| -- | ------ |
| 10 | React  |

Can Arjun take many courses?

✅ Yes

Can React have many students?

✅ Yes

This is:

```text
Many ↔ Many
```

❌ Wrong Attempt

Adding:

```sql
course_id
```

inside students.

Won't work.

A student can take many courses.

##### ✅ Correct Solution

Create a junction table.

Students

| id | name  |
| -- | ----- |
| 1  | Arjun |

Courses

| id | course |
| -- | ------ |
| 10 | React  |
| 11 | Node   |

Enrollments

| student_id | course_id |
| ---------- | --------- |
| 1          | 10        |
| 1          | 11        |

##### 🏗️ Implementation

```sql
CREATE TABLE enrollments (
    student_id INTEGER,
    course_id INTEGER,
    PRIMARY KEY(student_id, course_id),
    FOREIGN KEY(student_id)
        REFERENCES students(id),
    FOREIGN KEY(course_id)
        REFERENCES courses(id)
);
```

> #### 🔥 Why Is Many-to-Many Usually Written Using Table-Level Constraints?
>
> Because there are multiple constraints.
>
> Look at this:
>
> ```sql
> CREATE TABLE enrollments (
>     student_id INTEGER,
>     course_id INTEGER,
>
>     PRIMARY KEY(student_id, course_id),
>
>     FOREIGN KEY(student_id)
>         REFERENCES students(id),
>
>     FOREIGN KEY(course_id)
>         REFERENCES courses(id)
> );
> ```
>
> Very clear:
>
> ```text
> Columns
> ↓
> Primary Key
> ↓
> Foreign Keys
> ```
>
> ##### ✅ Equivalent Inline Version
>
> You could write:
>
> ```sql
> CREATE TABLE enrollments (
>     student_id INTEGER REFERENCES students(id),
>     course_id INTEGER REFERENCES courses(id),
>
>     PRIMARY KEY(student_id, course_id)
> );
> ```
>
> This is also valid PostgreSQL.
>
> #### 🚀 Real-World Convention
>
> Most developers use:
>
> 🔹 Inline
>
> when there is a simple single foreign key:
>
> ```sql
> CREATE TABLE orders (
>     id SERIAL PRIMARY KEY,
>     user_id INTEGER REFERENCES users(id)
> );
> ```
>
> 🔹 Table-Level
>
> when there are multiple constraints:
>
> ```sql
> CREATE TABLE enrollments (
>     student_id INTEGER,
>     course_id INTEGER,
>
>     PRIMARY KEY(student_id, course_id),
>
>     FOREIGN KEY(student_id)
>         REFERENCES students(id),
>
>     FOREIGN KEY(course_id)
>         REFERENCES courses(id)
> );
> ```
>
> because it stays cleaner as complexity grows.
>
> #### 💼 Interview Question
>
> **❓ Why Can't We Directly Implement Many-to-Many With A Single Foreign Key?**
>
> Expected answer:
>
>> Because each side can have multiple related records. A single foreign key can only point to one record. Therefore we use a junction table containing foreign keys to both tables.
>>
>
> ##### ❓ What Is A Junction Table?
>
> Expected answer:
>
>> A table used to connect two tables in a many-to-many relationship. It typically contains foreign keys referencing both tables.
>>
>
> #### Why Use `PRIMARY KEY(student_id, course_id)`?
>
> Expected answer:
>
>> To prevent duplicate relationships such as enrolling the same student in the same course multiple times.
>>
>
> #### 🎯 Rule Of Thumb
>
> Whenever you hear:
>
> ```text
> Users ↔ Roles
> Students ↔ Courses
> Posts ↔ Tags
> Products ↔ Categories
> Projects ↔ Members
> ```
>
> immediately think:
>
> ```text
> Many-to-Many
> ↓
> Need Junction Table
> ↓
> Contains 2 Foreign Keys
> ↓
> Usually Uses Composite Primary Key
> ```
>
> That's the pattern you'll see in PostgreSQL, MySQL, Prisma, Sequelize, TypeORM, and almost every relational database system. 🚀

##### 🔥 Composite Primary Key

Notice:

```sql
PRIMARY KEY(
    student_id,
    course_id
)
```

Prevents:

| student_id | course_id |
| ---------- | --------- |
| 1          | 10        |
| 1          | 10        |

Duplicate enrollment.

##### 💼 Real Examples

Users ↔ Roles

```text
User can have many roles
Role can belong to many users
```

Posts ↔ Tags

```text
Post can have many tags
Tag can belong to many posts
```

Products ↔ Categories

```text
Product can be in many categories
Category contains many products
```

Users ↔ Groups

```text
Many users
Many groups
```

### 🔑 Foreign Keys

The backbone of relationships.

Example:

```sql
FOREIGN KEY(user_id)
REFERENCES users(id)
```

Meaning:

```text
user_id must exist in users table
```

**🚨 Foreign Key Violation**

Users:

| id |
| -- |
| 1  |

Try:

```sql
INSERT INTO orders(user_id)
VALUES(999);
```

Result:

❌ Error

Because user 999 doesn't exist.

##### 🔥 Cascading Deletes

Interview favorite.

**Without Cascade**

```sql
DELETE FROM users
WHERE id = 1;
```

while orders exist.

Result:

❌ Error

**With Cascade**

```sql
FOREIGN KEY(user_id)
REFERENCES users(id)
ON DELETE CASCADE
```

Now:

```sql
DELETE FROM users
WHERE id = 1;
```

Automatically deletes:

```text
User
+
Orders
```

##### ⚠️ Cascade Danger

Suppose:

```text
User
↓
Orders
↓
Order Items
↓
Payments
```

Deleting one user could delete:

```text
Thousands of rows
```

Use carefully.

### 🎯 Optional vs Mandatory Relationship

**Mandatory**

Every order must have a user.

```sql
user_id INTEGER NOT NULL
```

**Optional**

Order may not have user.

```sql
user_id INTEGER NULL
```

Usually guest checkout.

### 🔥 Circular Relationship

Bad design.

Example:

```text
Users references Profiles
Profiles references Users
```

Can cause insertion problems.

Avoid when possible.

### 🎯 Identifying Relationship Type Quickly

**One-to-One**

Ask:

```text
Can A have multiple B?
```

No.

```text
Can B have multiple A?
```

No.

➡️ One-to-One

**One-to-Many**

Ask:

```text
Can User have many Orders?
```

Yes.

```text
Can Order have many Users?
```

No.

➡️ One-to-Many

**Many-to-Many**

Ask:

```text
Can Student have many Courses?
```

Yes.

```text
Can Course have many Students?
```

Yes.

➡️ Many-to-Many

### 🧩 Prisma Mapping

**One-to-Many**

```prisma
model User {
  id     Int     @id
  orders Order[]
}

model Order {
  id      Int  @id
  userId  Int
  user    User @relation(fields: [userId], references: [id])
}
```

**One-to-One**

```prisma
model User {
  id      Int      @id
  profile Profile?
}

model Profile {
  id      Int @id
  userId  Int @unique
  user    User @relation(fields:[userId], references:[id])
}
```

**Many-to-Many**

```prisma
model Student {
  id      Int      @id
  courses Course[]
}

model Course {
  id       Int       @id
  students Student[]
}
```

Prisma can automatically create the junction table.

### 🚨 Common Interview Traps

**❓ Is Foreign Key Always Unique?**

Answer:

❌ No

One-to-Many foreign keys are usually repeated.

Example:

| order_id | user_id |
| -------- | ------- |
| 1        | 1       |
| 2        | 1       |
| 3        | 1       |

**❓ Can a Table Have Multiple Foreign Keys?**

Absolutely.

Example:

```sql
orders
```

| column     |
| ---------- |
| user_id    |
| product_id |
| payment_id |

All may be foreign keys.

**❓ Is Every Relationship Implemented Using JOIN?**

Answer:

> Relationships are stored using foreign keys. JOINs are used later to query and combine related data.

This is a very common conceptual question.

### 🎯 High-Frequency Interview Questions

**❓ Difference Between One-to-One and One-to-Many?**

Expected:

> One-to-One uses a UNIQUE foreign key. One-to-Many allows multiple child rows to reference the same parent.

**❓ How Do You Implement Many-to-Many?**

Expected:

> Using a junction table containing foreign keys to both tables.

**❓ What Is a Junction Table?**

Expected:

> A table that connects two tables in a Many-to-Many relationship.

**❓ Can a Foreign Key Be NULL?**

Expected:

> Yes, unless constrained with NOT NULL.

---

# ----Alter

### What Is ALTER?

`ALTER TABLE` modifies an existing table.

Imagine:

### Before

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

Later you realize:

```text
Need email column
```

Instead of recreating the table:

```sql
ALTER TABLE users
ADD COLUMN email TEXT;
```

### ➕ Add Column

Most common operation.

```sql
ALTER TABLE users
ADD COLUMN email TEXT;
```

Before:

| id | name |
| -- | ---- |

After:

| id | name | email |
| -- | ---- | ----- |

### ❌ Drop Column

```sql
ALTER TABLE users
DROP COLUMN email;
```

Removes the column completely.

### ✏️ Rename Column

```sql
ALTER TABLE users
RENAME COLUMN name TO full_name;
```

Before:

```text
name
```

After:

```text
full_name
```

### 🏷️ Rename Table

```sql
ALTER TABLE users
RENAME TO customers;
```

### 🔒 Add Constraint

Example:

```sql
ALTER TABLE users
ADD CONSTRAINT unique_email
UNIQUE(email);
```

Now duplicate emails are rejected.

### 🚨 Set NOT NULL

Suppose:

```sql
email TEXT
```

Later:

```sql
ALTER TABLE users
ALTER COLUMN email
SET NOT NULL;
```

Now email becomes mandatory.

### 🔓 Remove NOT NULL

```sql
ALTER TABLE users
ALTER COLUMN email
DROP NOT NULL;
```

### 🔄 Change Data Type

Example:

```sql
ALTER TABLE users
ALTER COLUMN age
TYPE BIGINT;
```

Changes:

```text
INTEGER
↓
BIGINT
```

### 🧠 How Prisma Uses ALTER

Suppose:

### Initial Schema

```prisma
model User {
  id   Int    @id @default(autoincrement())
  name String
}
```

Later:

```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  email String
}
```

Run:

```bash
npx prisma migrate dev
```

Prisma internally generates something like:

```sql
ALTER TABLE users
ADD COLUMN email TEXT;
```

You rarely write it yourself.

### 💼 Interview-Level Knowledge You Should Have

Know what these do:

### ➕

```sql
ALTER TABLE users
ADD COLUMN email TEXT;
```

### ❌

```sql
ALTER TABLE users
DROP COLUMN email;
```

### ✏️

```sql
ALTER TABLE users
RENAME COLUMN name TO full_name;
```

### 🔒

```sql
ALTER TABLE users
ALTER COLUMN email
SET NOT NULL;
```

---

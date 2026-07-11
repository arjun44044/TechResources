# 🚀 Prisma Query Practice Set

This is exactly how I'd prepare you for a follow-up interview.

**Rule:** Don't use aggregation (`groupBy`, `_sum`, `_avg`, etc.) yet.

Focus on:

* `findMany`
* `findUnique`
* `findFirst`
* `where`
* `AND`, `OR`, `NOT`
* `some`, `every`, `none`
* `is`, `isNot`
* `select`
* `include`
* `orderBy`
* `skip`
* `take`
* `create`
* `update`
* `delete`
* `upsert`
* `connect`

📋 Schema We'll Use

```prisma
model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  age       Int
  isActive  Boolean

  posts     Post[]
  profile   Profile?
}

model Post {
  id         Int      @id @default(autoincrement())
  title      String
  published  Boolean
  views      Int

  userId     Int
  user       User @relation(fields:[userId], references:[id])
}

model Profile {
  id      Int @id @default(autoincrement())
  bio     String

  userId  Int @unique
  user    User @relation(fields:[userId], references:[id])
}
```

### 🟢 Level 1 — Easy (5 Questions)

##### 1️⃣ Get all users

Expected:

```text
SELECT * FROM users
```

Write Prisma query.

```pgsql
await prisma.user.findMany()
```

##### 2️⃣ Get user whose email is

```text
john@gmail.com
```

```pgsql
await prisma.user.findUnique({
	where: {email: "john@gmail.com"}
})
```

##### 3️⃣ Get all active users

```text
isActive = true
```

```pgsql
await prisma.user.findMany({
	where: {isActive: true}
})
```

##### 4️⃣ Get users older than 25

Use:

```pgsql
await prisma.user.findMany({
	where: {age: {gt: 25}}
})
```

```

```

##### 5️⃣ Get all posts that are published

```text
published = true
```

```pgsql
await prisma.post.findMany({
	where: {published: true }
})
```

### 🟡 Level 2 — Intermediate (5 Questions)

##### 6️⃣ Get users whose age is between

```text
20 and 30
```

```pg
await prism.user.findMany({
	where: {age: {
		gt:20, lt:30
	}}
})
OR
await prisma.user.findMany({
	where: {AND: [
		{age: {gt: 20}},
		{age: {lt:30}}
	]}
})
```

##### 7️⃣ Get users whose name contains

```text
"ar"
```

Case insensitive.

```pgsql
await prisma.user.findMany({
	where: {name: {
		contains: "ar", mode: "insensitive"
	}}
})
```

##### 8️⃣ Get all posts ordered by views descending

Most viewed first.

```pgsql
await prisma.post.findMany({orderBy: {views: "desc"}})
```

##### 9️⃣ Get first 5 users

Pagination.

```pgsql
await prisma.user.findMany({take: 5})
```

##### 🔟 Get next 5 users

Assume first page already loaded.

```
await prisma.user.findMany({skip: 5, take: 5})
```

### 🟠 Level 3 — Relations (5 Questions)

##### 1️⃣1️⃣ Get all users along with their posts

Need related data.

```
await prisma.user.findMany({
	include: {posts: true}
})
```

##### 1️⃣2️⃣ Get all posts with author information

Need user details.

```
await prisma.post.findMany({
	include: {user: true}
})
```

##### 1️⃣3️⃣ Get users having at least one published post

Use:

```text
some
```

```
await prisma.user.findMany({
	posts: {some: {
		published: true
	}}
})
```

##### 1️⃣4️⃣ Get users having no published posts

Use:

```text
none
```

```
await prisma.user.findMany({
	where: {posts: {
		none: {published: true}
	}}
})
```

##### 1️⃣5️⃣ Get users where every post is published

Use:

```text
every
```

```
await prisma.user.findMany({
	where: {posts: {
		every: {published: true}
	}}
})

```

### 🔴 Level 4 — Advanced Filtering (5 Questions)

##### 1️⃣6️⃣ Get active users older than 25

Use:

```text
AND
```

```
await prisma.user.findMany({
	AND: [
	{isActive: true}, {age: {gt: 25}}
	]
})
```

##### 1️⃣7️⃣ Get users who are

```text
older than 30
OR
inactive
```

```
await prisma.user.findMany({
	OR: [
	{isActive: false}, {age: {gt: 30}}
	]
})

```

##### 1️⃣8️⃣ Get users whose name does NOT contain

```text
john
```

Use:

```text
NOT
```

```
await prisma.user.findMany({
	NOT: {name: {contains: "john"}}
})
```

##### 1️⃣9️⃣ Get users with profiles

Use:

```text
isNot
```

```

```

##### 2️⃣0️⃣ Get users without profiles

Use:

```text
is
```

### 🔥 Level 5 — Real Interview Style (5 Questions)

##### 2️⃣1️⃣ Create a new user

```text
name = Arjun
email = arjun@gmail.com
age = 24
```

```
await prima.user.create({
	data: {
		{name: "Arjun", email: "arjun@gmail.com", age: 24}
	}
})
```

##### 2️⃣2️⃣ Update user's age to 30

For:

```text
id = 1
```

```
await prisma.user.update({
	where: {id: 1},
	data: {age: 30}
})
```

##### 2️⃣3️⃣ Increase post views by 1

For:

```text
postId = 5
```

Use:

```text
increment
```

```
await prism.post.update({
	where: {id: 5},
	data: {views: {
		increment: 1
	}}
})
```

##### 2️⃣4️⃣ Delete all unpublished posts

Use:

```text
deleteMany
```



##### 2️⃣5️⃣ Upsert a user

If:

```text
john@gmail.com
```

exists → update age.

Otherwise create user.

### 🏆 Bonus (Questions Interviewers Love)

##### 2️⃣6️⃣ Get only

```text
name
email
```

for all users.

(No unnecessary fields.)

##### 2️⃣7️⃣ Get users and only titles of their posts.

Hint:

```text
include + select
```

##### 2️⃣8️⃣ Get top 3 most viewed published posts.

Combine:

```text
where
orderBy
take
```

##### 2️⃣9️⃣ Create a post and connect it to existing user id=1.

Use:

```text
connect
```

##### 3️⃣0️⃣ Get users having at least one post with more than 1000 views.

Use:

```text
some
gt
```

🎯 How We'll Do This

Reply with answers for:

**Q1 → Q5 only.**

I'll review them exactly like an interviewer:

✅ Correctness
✅ Better alternatives
✅ Prisma-specific improvements
✅ Interview score out of 10

Then we'll move to Q6 → Q10. 🚀

# ----Introduction

### 🛡️ What is Zod?

**Zod is a TypeScript-first schema validation library.**

In simple terms:

> **Zod lets you define what data should look like, and then check whether incoming data actually follows those rules.**

For example, suppose your backend expects a user registration request:

```ts
{
  name: "Arun",
  email: "arun@example.com",
  age: 33,
  password: "secret123"
}
```

You can describe the rules with Zod:

```ts
import { z } from "zod";

const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  age: z.number().int().positive(),
  password: z.string().min(8),
});
```

Then validate:

```ts
const result = userSchema.safeParse(data);
```

Zod checks things like:

* Is `name` actually a string?
* Is `email` a valid email?
* Is `age` a positive integer?
* Is `password` at least 8 characters?
* Are required fields present?

### 🧠 Why do we need Zod if we already have TypeScript?

This is the  **most important thing to understand** .

TypeScript checks your code  **during development/compile time** .

Zod checks  **actual data at runtime** .

Consider:

```ts
type User = {
  name: string;
  age: number;
};
```

You might think this guarantees:

```ts
const user: User = {
  name: "Arun",
  age: 33,
};
```

But TypeScript cannot protect you from data coming from the outside world.

For example:

```ts
const response = await fetch("/api/user");
const user: User = await response.json();
```

You are basically telling TypeScript:

> "Trust me, this response is a User."

But what if the server actually returns:

```json
{
  "name": 123,
  "age": "hello"
}
```

TypeScript doesn't magically inspect the HTTP response.

That's where Zod comes in.

```ts
const result = userSchema.safeParse(await response.json());

if (!result.success) {
  console.log(result.error);
} else {
  const user = result.data;
}
```

Now the actual runtime data is checked.

### 🔑 Think of it this way

| TypeScript                  | Zod                  |
| --------------------------- | -------------------- |
| Compile-time                | Runtime              |
| Protects your code          | Protects your data   |
| Types                       | Schemas              |
| Doesn't inspect API data    | Validates API data   |
| Doesn't validate user input | Validates user input |

So:

> **TypeScript tells you what data is supposed to look like. Zod checks whether the actual data really looks like that.**

### 🧩 Your first Zod schema

Let's start extremely simply.

```ts
import { z } from "zod";

const nameSchema = z.string();
```

This says:

> "I expect a string."

Now:

```ts
nameSchema.parse("Arun");
```

Works.

But:

```ts
nameSchema.parse(123);
```

throws a validation error.

---

# ----Zod Types

### 📦 Basic Zod types

Zod supports many common types.

**String**

```ts
z.string()
```

**Number**

```ts
z.number()
```

**Boolean**

```ts
z.boolean()
```

**Date**

```ts
z.date()
```

**Array**

```ts
z.array(z.string())
```

Meaning:

```ts
["React", "Node", "MongoDB"]
```

is valid.

But:

```ts
["React", 123]
```

isn't.

**Object**

```ts
z.object({
  name: z.string(),
  age: z.number(),
})
```

**Enum**

```ts
z.enum(["admin", "user", "manager"])
```

Only those values are accepted.

### 🏗️ Objects — where Zod becomes really useful

Suppose you're creating a registration API.

```ts
const registerSchema = z.object({
  name: z.string(),
  email: z.string(),
  password: z.string(),
});
```

You can validate:

```ts
const data = {
  name: "Arun",
  email: "arun@example.com",
  password: "hello123",
};

registerSchema.parse(data);
```

---

# ----Adding constraints

This is where Zod becomes much more powerful.

##### String length

```ts
z.string().min(3)
```

At least 3 characters.

```ts
z.string().max(50)
```

Maximum 50 characters.

You can combine them:

```ts
z.string().min(3).max(50)
```

##### 📧 Email

Instead of:

```ts
z.string()
```

you can use:

```ts
z.string().email()
```

So:

```ts
"arun@example.com"
```

passes.

But:

```ts
"hello"
```

fails.

##### 🔐 Password

```ts
const passwordSchema = z
  .string()
  .min(8)
  .max(100);
```

You could make it more strict:

```ts
const passwordSchema = z
  .string()
  .min(8)
  .regex(/[A-Z]/)
  .regex(/[0-9]/);
```

Meaning:

* minimum 8 characters
* must contain uppercase letter
* must contain a number

##### 🔢 Numbers

You can do:

```ts
z.number().positive()
```

or:

```ts
z.number().min(18)
```

or:

```ts
z.number().max(100)
```

or:

```ts
z.number().int()
```

For example:

```ts
const ageSchema = z
  .number()
  .int()
  .min(18)
  .max(100);
```

> #### ➕ `nonnegative()` vs `positive()`
>
> The difference is:
>
> ```ts
> z.number().positive()
> ```
>
> means:
>
>> Must be  **greater than 0** .
>>
>
> Valid:
>
> ```text
> 1
> 5
> 100
> ```
>
> Invalid:
>
> ```text
> 0
> -1
> ```
>
> Whereas:
>
> ```ts
> z.number().nonnegative()
> ```
>
> means:
>
>> Must be  **greater than or equal to 0** .
>>
>
> Valid:
>
> ```text
> 0
> 1
> 5
> 100
> ```
>
> Invalid:
>
> ```text
> -1
> ```

##### 📋 Arrays

Suppose a product has tags:

```ts
const tagsSchema = z.array(z.string());
```

Valid:

```ts
["gold", "diamond", "ring"]
```

Invalid:

```ts
["gold", 123, true]
```

You can also constrain the array:

```ts
const tagsSchema = z
  .array(z.string())
  .min(1)
  .max(10);
```

##### 🎯 Enums

Suppose your jewellery application has:

```ts
type ProductStatus = "available" | "sold" | "reserved";
```

Zod:

```ts
const statusSchema = z.enum([
  "available",
  "sold",
  "reserved",
]);
```

This is valid:

```ts
"available"
```

This isn't:

```ts
"deleted"
```

This is particularly useful for:

* roles
* statuses
* categories
* order states
* payment states
* permissions

### 👤 A realistic user schema

Let's combine everything.

```ts
const userSchema = z.object({
  name: z.string().min(2).max(50),

  email: z.string().email(),

  age: z.number().int().min(18),

  password: z.string().min(8),

  role: z.enum(["user", "admin"]),
});
```

Now we have a complete validation contract.

---

# ----`parse()` vs `safeParse()`

This is very important.

There are two common ways to validate.

### `parse()`

```ts
const user = userSchema.parse(data);
```

If valid:

```ts
user
```

contains the validated data.

If invalid, Zod  **throws an error** .

Example:

```ts
try {
  const user = userSchema.parse(data);
} catch (error) {
  console.log(error);
}
```

### `safeParse()`

Usually better when dealing with user/API input:

```ts
const result = userSchema.safeParse(data);
```

It doesn't throw.

Instead, you get something like:

```ts
{
  success: true,
  data: ...
}
```

or:

```ts
{
  success: false,
  error: ...
}
```

So:

```ts
const result = userSchema.safeParse(data);

if (!result.success) {
  console.log(result.error);
  return;
}

console.log(result.data);
```

### ⭐ Practical rule

For external/untrusted data:

```ts
safeParse()
```

is often convenient.

For situations where invalid data should immediately throw:

```ts
parse()
```

is useful.

---

# 🧠 The really powerful part

### ⚡Zod can generate TypeScript types

This is one of the biggest reasons developers love Zod.

Suppose you have:

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  age: z.number(),
});
```

You can generate the TypeScript type:

```ts
type User = z.infer<typeof userSchema>;
```

Now TypeScript understands:

```ts
type User = {
  name: string;
  email: string;
  age: number;
};
```

You  **don't need to write the type separately** .

Instead of:

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
});

type User = {
  name: string;
  email: string;
};
```

you can simply do:

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
});

type User = z.infer<typeof userSchema>;
```

This gives you a  **single source of truth** .

### 🔥 Schema + Type together

A very common pattern in modern TypeScript applications:

```ts
import { z } from "zod";

const createUserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  age: z.number().int().positive(),
});

type CreateUserInput = z.infer<typeof createUserSchema>;
```

Then:

```ts
function createUser(data: CreateUserInput) {
  // TypeScript knows the structure
}
```

And before calling it:

```ts
const result = createUserSchema.safeParse(input);

if (!result.success) {
  return;
}

createUser(result.data);
```

Now you have:

**Runtime validation + TypeScript typing.**

---

# ----Zod in a Next.js API route

This is where Zod becomes particularly relevant to you.

Imagine:

```text
Frontend
   ↓
POST /api/users
   ↓
Next.js API Route
   ↓
Zod validation
   ↓
Business logic
   ↓
Prisma
   ↓
PostgreSQL
```

Frontend sends:

```json
{
  "name": "Arun",
  "email": "arun@example.com",
  "age": 33
}
```

### Your API route:

```ts
import { z } from "zod";

const createUserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  age: z.number().int().positive(),
});

export async function POST(req: Request) {
  const body = await req.json();

  const result = createUserSchema.safeParse(body);

  if (!result.success) {
    return Response.json(
      {
        error: result.error.flatten(),
      },
      { status: 400 }
    );
  }

  const data = result.data;

  // Now data is validated
  // Prisma database operation here

  return Response.json(data);
}
```

That's a very typical real-world use.

### 🛡️ Why validation should happen on the backend

Suppose your frontend has:

```ts
const schema = z.object({
  email: z.string().email(),
});
```

You validate the email.

But a malicious user can completely bypass your React application and directly call:

```text
POST /api/users
```

with:

```json
{
  "email": "not-an-email"
}
```

Therefore:

> **Frontend validation improves UX. Backend validation provides security and correctness.**

You can use Zod on both.

```text
React form
   ↓
Zod
   ↓
API request
   ↓
Next.js API
   ↓
Zod again
   ↓
Database
```

---

# ----Optional, Nullable fields & Default values

### 📝 Optional fields

Suppose:

```ts
const userSchema = z.object({
  name: z.string(),
  phone: z.string().optional(),
});
```

Then both are valid:

```ts
{
  name: "Arun"
}
```

and:

```ts
{
  name: "Arun",
  phone: "9876543210"
}
```

### 🕳️ Nullable vs optional

This distinction is important.

**-- Optional**

```ts
phone: z.string().optional()
```

means:

```ts
phone
```

can be missing.

**-- Nullable**

```ts
phone: z.string().nullable()
```

means:

```ts
phone: null
```

is allowed.

These are different.

```ts
// Optional
{}

// Nullable
{
  phone: null
}
```

You can combine them:

```ts
phone: z.string().nullable().optional()
```

Then all three can be valid:

```ts
{}
```

```ts
{
  phone: null
}
```

```ts
{
  phone: "9876543210"
}
```

### 🔄 Default values

You can specify defaults:

```ts
const userSchema = z.object({
  name: z.string(),

  role: z
    .enum(["user", "admin"])
    .default("user"),
});
```

If:

```ts
{
  name: "Arun"
}
```

is passed, the parsed result can contain:

```ts
{
  name: "Arun",
  role: "user"
}
```

---

# ----Transforming data

Zod isn't only about validation.

It can also transform data.

For example:

```ts
const emailSchema = z
  .string()
  .email()
  .transform((email) => email.toLowerCase());
```

Input:

```ts
"ARUN@EXAMPLE.COM"
```

Output:

```ts
"arun@example.com"
```

Another example:

```ts
const usernameSchema = z
  .string()
  .trim()
  .toLowerCase();
```

This can turn:

```text
"  ARUN  "
```

into:

```text
"arun"
```

### 🧹 `transform()` + validation

You can combine validation and transformation:

```ts
const usernameSchema = z
  .string()
  .min(3)
  .transform((value) => value.trim().toLowerCase());
```

Input:

```text
"   ARUN   "
```

Output:

```text
"arun"
```

But:

```text
"AB"
```

fails because it doesn't satisfy `.min(3)`.

So:

```text
Input
 ↓
Validate
 ↓
Transform
 ↓
Output
```

### 🔢 A more interesting `transform()` example

Suppose an API sends:

```json
{
  "age": "33"
}
```

but your application wants:

```ts
age: number
```

You could do:

```ts
const ageSchema = z
  .string()
  .transform((value) => Number(value));
```

Then:

```ts
const age = ageSchema.parse("33");
```

gives:

```ts
33
```

Notice something important:

```ts
// Input
string

// Output
number
```

That's one reason `transform()` is powerful.

### 💰 Practical example: price

Imagine a form gives you:

```text
"4999.50"
```

as a string.

You might transform it:

```ts
const priceSchema = z
  .string()
  .transform((value) => Number(value));
```

Then:

```ts
priceSchema.parse("4999.50");
```

becomes:

```ts
4999.5
```

However, in production you'd usually want to **validate that the string is actually numeric** rather than blindly calling `Number()`.

### 🧠 `transform()` changes the inferred type

This is important.

```ts
const schema = z
  .string()
  .transform((value) => value.length);
```

Input:

```ts
string
```

Output:

```ts
number
```

So:

```ts
type Result = z.infer<typeof schema>;
```

is:

```ts
number
```

not:

```ts
string
```

This is something to keep in mind when using transformed schemas in TypeScript.

---

# ----Refine & Cross-field validation

### 🔗 Cross-field validation

Sometimes one field depends on another.

For example:

```text
password
confirmPassword
```

You want:

```text
password === confirmPassword
```

You can use `refine()`.

```ts
const registerSchema = z
  .object({
    password: z.string().min(8),

    confirmPassword: z.string(),
  })
  .refine(
    (data) => data.password === data.confirmPassword,
    {
      message: "Passwords do not match",
      path: ["confirmPassword"],
    }
  );
```

Now Zod checks the relationship between the two fields.

> #### 🎯 What does `path` mean in `refine()`?
>
> ```ts
> .refine(
>   (data) => data.password === data.confirmPassword,
>   {
>     message: "Passwords do not match",
>     path: ["confirmPassword"],
>   }
> )
> ```
>
> Here, **`path` tells Zod which field the error should be associated with.**
>
> **-- Without `path`**
>
> Suppose:
>
> ```ts
> const registerSchema = z
>   .object({
>     password: z.string().min(8),
>     confirmPassword: z.string(),
>   })
>   .refine(
>     (data) => data.password === data.confirmPassword,
>     {
>       message: "Passwords do not match",
>     }
>   );
> ```
>
> The validation concerns the  **relationship between two fields** , rather than one individual field.
>
> So Zod doesn't automatically know:
>
>> "Should I display this error under `password` or `confirmPassword`?"
>>
>
> You can tell it:
>
> ```ts
> path: ["confirmPassword"]
> ```
>
> Meaning:
>
>> Put this validation error on the `confirmPassword` field.
>>
>
> So a form library can display:
>
> ```text
> Password
> [••••••••]
>
> Confirm Password
> [•••••••]
> Passwords do not match
> ```
>
> #### Why is it an array?
>
> Because Zod paths can represent  **nested data** .
>
> For:
>
> ```ts
> {
>   user: {
>     profile: {
>       age: 15
>     }
>   }
> }
> ```
>
> the path could be:
>
> ```ts
> ["user", "profile", "age"]
> ```
>
> For an array:
>
> ```ts
> {
>   products: [
>     { name: "Ring" },
>     { name: "Necklace" }
>   ]
> }
> ```
>
> a path could look like:
>
> ```ts
> ["products", 1, "name"]
> ```
>
> So:
>
> ```ts
> path: ["confirmPassword"]
> ```
>
> just means:
>
> ```text
> root
>  └── confirmPassword
> ```

### 🧪 21. `refine()`

`refine()` is useful when built-in Zod validators aren't enough.

Example:

```ts
const usernameSchema = z
  .string()
  .min(3)
  .refine(
    (username) => !username.includes(" "),
    {
      message: "Username cannot contain spaces",
    }
  );
```

You can create custom validation logic.

---

# ----Nested objects

Real applications often have nested data.

For example:

```ts
const customerSchema = z.object({
  name: z.string(),

  address: z.object({
    street: z.string(),
    city: z.string(),
    pincode: z.string(),
  }),
});
```

Valid:

```ts
{
  name: "Arun",
  address: {
    street: "MG Road",
    city: "Thrissur",
    pincode: "680001"
  }
}
```

---

# ----Zod + Prisma + PostgreSQL, Database Constraints & React Forms

This is particularly relevant to what you've been learning.

You might have:

```text
React / Next.js
       ↓
      Zod
       ↓
API / Server Action
       ↓
     Prisma
       ↓
 PostgreSQL
```

Each has a different job.

**-- Zod**

Validates incoming data.

```ts
z.string()
z.number()
z.email()
z.enum()
```

**-- Prisma**

Communicates with PostgreSQL.

```ts
prisma.user.create(...)
prisma.product.findMany(...)
```

**-- PostgreSQL**

Actually stores the data.

```text
users
products
orders
customers
```

**--TypeScript**

Provides static type safety while you're writing the application.

So don't think:

> "Zod replaces Prisma."

It doesn't.

They solve different problems.

### 🚨 Zod does NOT replace database constraints

This is another important distinction.

You might have:

```ts
const userSchema = z.object({
  email: z.string().email(),
});
```

But your PostgreSQL database should still have appropriate constraints.

For example:

```text
email UNIQUE
```

Why?

Because validation and database integrity are different layers.

Think:

```text
          User input
              ↓
           Zod
        validation
              ↓
        Application
              ↓
           Prisma
              ↓
       PostgreSQL
              ↓
       DB constraints
```

Multiple layers protect your data.

### 🔥 Zod + React forms

Zod is extremely common with form libraries.

For example, you might eventually use:

```text
React Hook Form
        +
       Zod
```

Schema:

```ts
const loginSchema = z.object({
  email: z.string().email("Invalid email"),

  password: z.string().min(
    8,
    "Password must be at least 8 characters"
  ),
});
```

Then React Hook Form can use that schema to validate the form.

Conceptually:

```text
User types email
       ↓
React Hook Form
       ↓
Zod
       ↓
Validation error
```

For example:

```text
Invalid email
```

or:

```text
Password must be at least 8 characters
```

This is one of the most practical places you'll use Zod.

---

# ----Zod for API responses

You can also validate  **responses** .

Suppose your API is supposed to return:

```json
{
  "id": "123",
  "name": "Arun",
  "email": "arun@example.com"
}
```

Define:

```ts
const userResponseSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
});
```

Then:

```ts
const result = userResponseSchema.safeParse(response);
```

Now you're protecting against unexpected API responses too.

This becomes especially useful when consuming:

* third-party APIs
* payment APIs
* AI APIs
* external services
* microservices

---

# ----Schema composition, Reusing Schemas and Extending Schemas

### 🧬 Schema composition

You don't always need to write enormous schemas.

You can build them from smaller schemas.

For example:

```ts
const addressSchema = z.object({
  city: z.string(),
  state: z.string(),
});
```

Then:

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  address: addressSchema,
});
```

This makes large applications easier to maintain.

### ✏️ Reusing schemas

Suppose you have:

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  password: z.string(),
});
```

For an update API, password might not be required.

You can derive another schema:

```ts
const updateUserSchema = userSchema.partial();
```

Now every property becomes optional.

Conceptually:

```ts
{
  name?: string;
  email?: string;
  password?: string;
}
```

This is extremely useful for PATCH APIs.

### ➕ `.extend()`

You can extend an object schema.

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
});
```

Then:

```ts
const adminSchema = userSchema.extend({
  permissions: z.array(z.string()),
});
```

Now an admin has:

```ts
{
  name,
  email,
  permissions
}
```

---

# ----Union types & Discriminated unions

### 🔀 Union types

Suppose a field can be either a string or a number.

```ts
const idSchema = z.union([
  z.string(),
  z.number(),
]);
```

So both are valid:

```ts
"123"
```

and:

```ts
123
```

### 🎯 Discriminated unions

This is more advanced but extremely useful in TypeScript applications.

It solves this kind of problem:

> **"This object can have several different shapes, and one field tells me which shape it is."**

That special field is called the  **discriminator** .

Suppose payments can be:

```text
card
upi
cash
```

Each has different data.

```ts
const paymentSchema = z.discriminatedUnion("type", [
  z.object({
    type: z.literal("card"),
    cardNumber: z.string(),
  }),

  z.object({
    type: z.literal("upi"),
    upiId: z.string(),
  }),

  z.object({
    type: z.literal("cash"),
    amount: z.number(),
  }),
]);
```

Now Zod understands:

```ts
{
  type: "upi",
  upiId: "arun@upi"
}
```

versus:

```ts
{
  type: "card",
  cardNumber: "1234..."
}
```

This becomes valuable in complex applications.

> #### 💳 Example Explanation
>
> Imagine your application supports three payment methods:
>
> ```text
> card
> upi
> cash
> ```
>
> Each requires different information.
>
> **Card**
>
> ```ts
> {
>   type: "card",
>   cardNumber: "1234..."
> }
> ```
>
> **UPI**
>
> ```ts
> {
>   type: "upi",
>   upiId: "arun@upi"
> }
> ```
>
> **Cash**
>
> ```ts
> {
>   type: "cash",
>   amount: 500
> }
> ```
>
> These aren't the same structure.
>
> We can represent them with:
>
> ```ts
> const paymentSchema = z.discriminatedUnion("type", [
>   z.object({
>     type: z.literal("card"),
>     cardNumber: z.string(),
>   }),
>
>   z.object({
>     type: z.literal("upi"),
>     upiId: z.string(),
>   }),
>
>   z.object({
>     type: z.literal("cash"),
>     amount: z.number(),
>   }),
> ]);
> ```
>
> The discriminator is:
>
> ```ts
> "type"
> ```
>
> because `type` tells Zod which object structure to use.

##### 🔍 How does Zod interpret this?

Suppose:

```ts
{
  type: "upi",
  upiId: "arun@upi"
}
```

Zod sees:

```text
type = "upi"
```

So it knows:

> "Use the UPI schema."

It checks:

```ts
z.object({
  type: z.literal("upi"),
  upiId: z.string(),
})
```

It doesn't need to try every possible schema.

##### 🧠 Why not just use `union()`?

You could write:

```ts
const paymentSchema = z.union([
  z.object({
    type: z.literal("card"),
    cardNumber: z.string(),
  }),

  z.object({
    type: z.literal("upi"),
    upiId: z.string(),
  }),

  z.object({
    type: z.literal("cash"),
    amount: z.number(),
  }),
]);
```

This can work.

But `discriminatedUnion()` explicitly tells Zod:

> "Look at this field to determine which variant we're dealing with."

That's especially useful when your variants have a clear discriminator.

##### 🏪 A CreoGrid-style example

Imagine you're building an order system.

Orders can be:

```text
delivery
pickup
```

A delivery order requires an address:

```ts
{
  type: "delivery",
  address: "MG Road",
  pincode: "680001"
}
```

A pickup order requires a store:

```ts
{
  type: "pickup",
  storeId: "store_123"
}
```

Schema:

```ts
const orderSchema = z.discriminatedUnion("type", [
  z.object({
    type: z.literal("delivery"),
    address: z.string(),
    pincode: z.string(),
  }),

  z.object({
    type: z.literal("pickup"),
    storeId: z.string(),
  }),
]);
```

Now:

```ts
orderSchema.parse({
  type: "delivery",
  address: "MG Road",
  pincode: "680001",
});
```

works.

And:

```ts
orderSchema.parse({
  type: "pickup",
  storeId: "store_123",
});
```

works.

But:

```ts
orderSchema.parse({
  type: "delivery",
  storeId: "store_123",
});
```

fails because a delivery order needs an address and pincode.

##### 🧩 Where you'll encounter `discriminatedUnion()`

It's particularly useful for things like:

💳 Payments

```text
card
upi
cash
bank_transfer
```

📦 Orders

```text
delivery
pickup
```

👤 Users

```text
customer
employee
admin
```

🔔 Notifications

```text
email
sms
push
whatsapp
```

🤖 AI outputs

```text
success
error
needs_clarification
```

🏥 Healthcare systems

```text
appointment
walk_in
emergency
```

💎 Jewellery

For example, different stone types might require different information:

```text
diamond
gemstone
no_stone
```


---

# ----Zod errors

Suppose:

```ts
const schema = z.object({
  email: z.string().email(),
  age: z.number().min(18),
});
```

And the user sends:

```ts
{
  email: "hello",
  age: 15
}
```

Zod produces structured validation errors.

You can inspect:

```ts
result.error.issues
```

You'll get information describing things such as:

```text
email → invalid email
age → must be >= 18
```

This makes it easy to return useful errors to your frontend.

---

# ----Zod vs TypeScript

This is the distinction I want you to remember:

**-- TypeScript**

```ts
type User = {
  name: string;
  age: number;
};
```

This says:

> "While I'm writing code, treat User as having this structure."

**-- Zod**

```ts
const userSchema = z.object({
  name: z.string(),
  age: z.number(),
});
```

This says:

> "When actual data arrives, check that it really has this structure."

And:

```ts
type User = z.infer<typeof userSchema>;
```

combines the two nicely.

---

# ----Learning Recommendation

For your **Next.js + TypeScript + Prisma + PostgreSQL** path, learn Zod in this order:

**🟢 Level 1 — Essential**

```text
Schemas
z.object()
z.string()
z.number()
z.boolean()
z.array()
z.enum()
```

**🟢 Level 2 — Validation**

```text
min()
max()
email()
url()
int()
positive()
optional()
nullable()
default()
```

**🟢 Level 3 — Using it**

```text
parse()
safeParse()
error
issues
```

**🟢 Level 4 — TypeScript integration**

```ts
z.infer<typeof schema>
```

This is  **very important** .

**🟡 Level 5 — Application patterns**

```text
Zod + Next.js API routes
Zod + Server Actions
Zod + React Hook Form
Zod + Prisma
```

**🟡 Level 6 — Advanced**

```text
refine()
superRefine()
transform()
extend()
partial()
pick()
omit()
union()
discriminatedUnion()
```

---

# ----Pick, Omit and Merge

### 🧩 `pick()`

`pick()` means:

> **Take only these fields from an existing object schema.**

Suppose you have:

```ts
const userSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
  password: z.string(),
  phone: z.string(),
  role: z.enum(["user", "admin"]),
});
```

Maybe your login API only needs:

```text
email
password
```

Instead of creating another schema manually:

```ts
const loginSchema = z.object({
  email: z.string().email(),
  password: z.string(),
});
```

you can do:

```ts
const loginSchema = userSchema.pick({
  email: true,
  password: true,
});
```

Now `loginSchema` contains only:

```ts
{
  email: string;
  password: string;
}
```

**-- Where you'd use it**

Very commonly:

```text
User schema
   ↓
pick()
   ↓
Login schema
```

or:

```text
Product schema
   ↓
pick()
   ↓
Public product schema
```

### ✂️ `omit()`

`omit()` is basically the opposite of `pick()`.

It means:

> **Take the schema, but remove these fields.**

Using our user schema:

```ts
const userSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
  password: z.string(),
  phone: z.string(),
  role: z.enum(["user", "admin"]),
});
```

Suppose you want a schema representing user data that can safely be returned to the frontend.

You don't want:

```text
password
```

So:

```ts
const publicUserSchema = userSchema.omit({
  password: true,
});
```

Result:

```ts
{
  id: string;
  name: string;
  email: string;
  phone: string;
  role: "user" | "admin";
}
```

### **`pick()` vs `omit()`**

Remember:

```ts
pick()
```

> "I want  **these** ."

```ts
omit()
```

> "I want  **everything except these** ."

### 🔗 `merge()`

`merge()` allows you to combine two object schemas.

Suppose you have:

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
});
```

And:

```ts
const addressSchema = z.object({
  city: z.string(),
  state: z.string(),
  pincode: z.string(),
});
```

You can combine them:

```ts
const userWithAddressSchema =
  userSchema.merge(addressSchema);
```

Now the resulting schema expects:

```ts
{
  name: "Arun",
  email: "arun@example.com",
  city: "Thrissur",
  state: "Kerala",
  pincode: "680001"
}
```

### ⚠️ What if both schemas have the same field?

For example:

```ts
const a = z.object({
  name: z.string(),
});
```

and:

```ts
const b = z.object({
  name: z.number(),
});
```

Then:

```ts
a.merge(b)
```

results in the later schema's definition taking precedence for that overlapping key.

So be careful when merging schemas with duplicate fields.

---

# ----SuperRefine

### 🔬 1. What is `superRefine()`?

`superRefine()` is Zod's  **advanced custom validation method** .

You use it when the normal validators like:

```ts
z.string().min(3)
z.number().positive()
z.string().email()
```

or even:

```ts
.refine(...)
```

aren't enough.

The key difference is:

> **`refine()` is good for one custom condition. `superRefine()` gives you detailed control over multiple validation errors.**

### 🧠 2. First, remember what `refine()` does

Suppose we have registration:

```ts
const registerSchema = z
  .object({
    password: z.string().min(8),
    confirmPassword: z.string(),
  })
  .refine(
    (data) => data.password === data.confirmPassword,
    {
      message: "Passwords do not match",
      path: ["confirmPassword"],
    }
  );
```

This is great when you have  **one custom rule** :

```text
password === confirmPassword
```

But suppose we want to perform several custom checks and report each one separately.

That's where `superRefine()` becomes useful.

### 🔥 3. Basic `superRefine()` example

```ts
const passwordSchema = z
  .string()
  .superRefine((password, ctx) => {
    if (!/[A-Z]/.test(password)) {
      ctx.addIssue({
        code: "custom",
        message: "Password must contain an uppercase letter",
      });
    }

    if (!/[0-9]/.test(password)) {
      ctx.addIssue({
        code: "custom",
        message: "Password must contain a number",
      });
    }
  });
```

Now imagine the user enters:

```text
hello
```

`superRefine()` can report:

```text
Password must contain an uppercase letter
Password must contain a number
```

Notice that we can add  **multiple issues** .

That's the major idea.

### 🆚 4. `refine()` vs `superRefine()`

This is probably the most important distinction.

**-- `refine()`**

Think:

> **"Does this condition pass?"**

```ts
z.string().refine(
  (value) => value.length >= 8,
  {
    message: "Too short",
  }
);
```

It's simple and convenient.

**-- `superRefine()`**

Think:

> **"I'm going to inspect this value myself and potentially add several detailed errors."**

```ts
z.string().superRefine((value, ctx) => {
  if (...) {
    ctx.addIssue(...);
  }

  if (...) {
    ctx.addIssue(...);
  }

  if (...) {
    ctx.addIssue(...);
  }
});
```

So:

```text
refine()
   ↓
One custom validation condition


superRefine()
   ↓
Custom validation logic
       +
Multiple issues
       +
Precise error paths
       +
More control
```

### 🧩 5. Understanding `ctx`

This part initially looks confusing:

```ts
.superRefine((value, ctx) => {
```

There are two important things:

**-- `value`**

The value being validated.

**-- `ctx`**

The  **validation context** .

It gives you methods such as:

```ts
ctx.addIssue(...)
```

which allows you to tell Zod:

> "This data is invalid, and here's exactly why."

### 🚨 6. `ctx.addIssue()`

The basic pattern is:

```ts
ctx.addIssue({
  code: "custom",
  message: "Something is wrong",
});
```

For example:

```ts
const usernameSchema = z
  .string()
  .superRefine((username, ctx) => {
    if (username.includes(" ")) {
      ctx.addIssue({
        code: "custom",
        message: "Username cannot contain spaces",
      });
    }

    if (username.includes("@")) {
      ctx.addIssue({
        code: "custom",
        message: "Username cannot contain @",
      });
    }
  });
```

You can add as many issues as necessary.

### 🎯 7. Why `superRefine()` is particularly useful with objects

This is where it becomes much more interesting.

Suppose we have:

```ts
const registrationSchema = z.object({
  email: z.string().email(),
  password: z.string(),
  confirmPassword: z.string(),
});
```

Now suppose we want:

1. Password must contain uppercase.
2. Password must contain number.
3. Password must match confirmation.

We can do:

```ts
const registrationSchema = z
  .object({
    email: z.string().email(),
    password: z.string(),
    confirmPassword: z.string(),
  })
  .superRefine((data, ctx) => {
    if (!/[A-Z]/.test(data.password)) {
      ctx.addIssue({
        code: "custom",
        message: "Password must contain an uppercase letter",
        path: ["password"],
      });
    }

    if (!/[0-9]/.test(data.password)) {
      ctx.addIssue({
        code: "custom",
        message: "Password must contain a number",
        path: ["password"],
      });
    }

    if (data.password !== data.confirmPassword) {
      ctx.addIssue({
        code: "custom",
        message: "Passwords do not match",
        path: ["confirmPassword"],
      });
    }
  });
```

This is much more powerful than a single `refine()`.

### 📱 8. Why `path` becomes useful here

Notice:

```ts
path: ["password"]
```

and:

```ts
path: ["confirmPassword"]
```

That tells Zod  **which field should receive the error** .

So React Native + React Hook Form could display:

```text
Password
[hello123]

⚠ Password must contain an uppercase letter

Confirm Password
[hello456]

⚠ Passwords do not match
```

Instead of giving you a vague:

```text
Form is invalid
```

This is one reason `superRefine()` is useful for complex forms.

### 💰 9. A business example

Imagine an order:

```ts
const orderSchema = z.object({
  subtotal: z.number().nonnegative(),
  discount: z.number().nonnegative(),
  tax: z.number().nonnegative(),
  total: z.number().nonnegative(),
});
```

Every individual number could be valid.

But perhaps you require:

```text
total = subtotal - discount + tax
```

You can validate the relationship:

```ts
const orderSchema = z
  .object({
    subtotal: z.number().nonnegative(),
    discount: z.number().nonnegative(),
    tax: z.number().nonnegative(),
    total: z.number().nonnegative(),
  })
  .superRefine((data, ctx) => {
    const expectedTotal =
      data.subtotal - data.discount + data.tax;

    if (data.total !== expectedTotal) {
      ctx.addIssue({
        code: "custom",
        message: "Total does not match the calculated amount",
        path: ["total"],
      });
    }
  });
```

Now Zod is checking a  **business rule** , not merely a data type.

### 🔢 10. Array validation

`superRefine()` is also useful when validating relationships between items in an array.

Suppose you have product SKUs:

```ts
const productsSchema = z
  .array(
    z.object({
      sku: z.string(),
      name: z.string(),
    })
  )
  .superRefine((products, ctx) => {
    const skus = new Set<string>();

    products.forEach((product, index) => {
      if (skus.has(product.sku)) {
        ctx.addIssue({
          code: "custom",
          message: "Duplicate SKU",
          path: [index, "sku"],
        });
      }

      skus.add(product.sku);
    });
  });
```

This can catch:

```ts
[
  { sku: "R001", name: "Gold Ring" },
  { sku: "N001", name: "Gold Necklace" },
  { sku: "R001", name: "Diamond Ring" }
]
```

The third item gets an error at:

```ts
[2, "sku"]
```

That's a very useful capability.

### 🧠 11. `superRefine()` isn't only for "more validation"

Think about the levels:

**-- Basic Zod**

```ts
z.string()
```

> Is it a string?

**-- Built-in validation**

```ts
z.string().min(3)
```

> Is it at least 3 characters?

**-- `refine()`**

```ts
z.string().refine(...)
```

> Does my custom condition pass?

**-- `superRefine()`**

```ts
z.string().superRefine(...)
```

> Let me run arbitrary validation logic and add detailed issues wherever necessary.

That's the progression.

### ⚠️ 12. Don't use `superRefine()` for everything

This is important.

Don't write:

```ts
z.string().superRefine((value, ctx) => {
  if (value.length < 3) {
    ctx.addIssue(...);
  }
});
```

when you could simply write:

```ts
z.string().min(3)
```

Likewise, don't use `superRefine()` when:

```ts
z.string().email()
```

already solves the problem.

**Prefer:**

```text
Built-in Zod validator
       ↓
     refine()
       ↓
  superRefine()
```

Use the simplest tool that expresses the rule.

### 🧠 One particularly useful pattern

Suppose a customer can provide either:

```text
phone
```

or:

```text
email
```

but at least one must exist.

Schema:

```ts
const customerSchema = z
  .object({
    name: z.string(),
    email: z.string().email().optional(),
    phone: z.string().optional(),
  })
  .superRefine((data, ctx) => {
    if (!data.email && !data.phone) {
      ctx.addIssue({
        code: "custom",
        message: "Provide either email or phone",
        path: ["email"],
      });
    }
  });
```

This is a perfect example of something that isn't naturally expressed by:

```ts
z.string()
z.number()
z.optional()
```

alone.

---

# ----------------------------------------------------------------------

# ZOD

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

---

# ----------------------------------------------------------------------

# React Hook Form

# ----Introduction

### 🪝 1. What is React Hook Form?

**React Hook Form (RHF)** is a library for managing forms in React and React Native.

Without RHF, a form can become something like:

```text
Input 1 → useState
Input 2 → useState
Input 3 → useState
Input 4 → useState
Input 5 → useState
       ↓
validation
       ↓
errors
       ↓
submission
       ↓
loading
       ↓
resetting
```

RHF gives you a structured system for handling all of this.

The basic idea is:

```text
                React Hook Form
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Values       Errors      Submission
        │            │            │
        └────────────┼────────────┘
                     ↓
                  Your API
```

And when combined with Zod:

```text
React / React Native
        ↓
React Hook Form
        ↓
       Zod
        ↓
Validation
        ↓
API
```

For the **Next.js + React Native + TypeScript** stack you're learning, RHF is definitely worth knowing.

### 🧠 2. Why not just use `useState()`?

You absolutely  *can* .

For a tiny form:

```tsx
const [email, setEmail] = useState("");
const [password, setPassword] = useState("");
```

Then:

```tsx
<input
  value={email}
  onChange={(e) => setEmail(e.target.value)}
/>

<input
  value={password}
  onChange={(e) => setPassword(e.target.value)}
/>
```

Then you manually handle:

```text
values
validation
errors
submission
reset
touched fields
dirty fields
loading
etc.
```

With a larger form, this becomes tedious.

Imagine a jewellery admin form with:

```text
Product name
SKU
Category
Gross weight
Net weight
Purity
Stone type
Stone weight
Making charge
Wastage
HUID
Description
Images
```

Managing every field manually can get messy.

RHF centralizes the form management.

### 🏗️ 3. Your first React Hook Form

Install:

```bash
npm install react-hook-form
```

Basic example:

```tsx
import { useForm } from "react-hook-form";

type LoginForm = {
  email: string;
  password: string;
};

export default function LoginForm() {
  const {
    register,
    handleSubmit,
  } = useForm<LoginForm>();

  const onSubmit = (data: LoginForm) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("email")} />

      <input
        type="password"
        {...register("password")}
      />

      <button type="submit">
        Login
      </button>
    </form>
  );
}
```

This is the basic RHF pattern.

### 🔑 4. `useForm()`

The main hook is:

```tsx
const form = useForm();
```

Usually you'll destructure what you need:

```tsx
const {
  register,
  handleSubmit,
  formState,
  reset,
  watch,
  setValue,
  getValues,
} = useForm();
```

Don't worry about all of those yet.

The important ones initially are:

```text
register
handleSubmit
formState
reset
```

### 📝 5. `register()`

This is one of the most important RHF concepts.

You write:

```tsx
<input {...register("email")} />
```

You're essentially telling React Hook Form:

> "This input represents the `email` field in my form."

For:

```tsx
<input {...register("name")} />
```

RHF knows:

```text
name → this input
```

For:

```tsx
<input {...register("email")} />
```

it knows:

```text
email → this input
```

And so on.

### 📦 6. What does `{...register("email")}` actually do?

This:

```tsx
<input {...register("email")} />
```

looks strange at first.

`register("email")` returns several properties that RHF needs to connect the input to the form.

Conceptually:

```tsx
<input
  name="email"
  onChange={...}
  onBlur={...}
  ref={...}
/>
```

You normally don't need to manually write those.

RHF handles the connection.

### 🚀 7. `handleSubmit()`

This handles form submission.

You write:

```tsx
<form onSubmit={handleSubmit(onSubmit)}>
```

When the user submits:

```text
User clicks Submit
       ↓
handleSubmit()
       ↓
RHF collects values
       ↓
Validation
       ↓
onSubmit(data)
```

If everything is valid:

```tsx
const onSubmit = (data: LoginForm) => {
  console.log(data);
};
```

You receive:

```ts
{
  email: "arun@example.com",
  password: "secret123"
}
```

### 🧠 8. Why TypeScript generics matter

You saw:

```tsx
useForm<LoginForm>()
```

Suppose:

```ts
type LoginForm = {
  email: string;
  password: string;
};
```

Now TypeScript knows that the form contains:

```text
email
password
```

So:

```tsx
register("email")
```

is valid.

But:

```tsx
register("username")
```

would produce a TypeScript error because `username` isn't part of `LoginForm`.

This is one of the reasons RHF works very nicely with TypeScript.

### 🛡️ 9. Adding validation

RHF itself supports basic validation.

For example:

```tsx
<input
  {...register("email", {
    required: "Email is required",
  })}
/>
```

And:

```tsx
<input
  {...register("password", {
    required: "Password is required",
    minLength: {
      value: 8,
      message: "Password must be at least 8 characters",
    },
  })}
/>
```

Now RHF can validate these fields.

### 🚨 10. `formState.errors`

Where do validation errors go?

Here:

```tsx
formState.errors
```

Example:

```tsx
const {
  register,
  handleSubmit,
  formState: { errors },
} = useForm<LoginForm>();
```

Then:

```tsx
<input
  {...register("email", {
    required: "Email is required",
  })}
/>

{errors.email && (
  <p>{errors.email.message}</p>
)}
```

If the user doesn't enter an email:

```text
Email is required
```

appears.

---

# ----React Hook Forms and Zod Together

### 🧩 RHF + Zod

This is where the combination gets really good.

Install:

```bash
npm install zod @hookform/resolvers
```

Create your schema:

```ts
import { z } from "zod";

const loginSchema = z.object({
  email: z.string().email("Invalid email"),

  password: z
    .string()
    .min(8, "Password must be at least 8 characters"),
});
```

Generate the TypeScript type:

```ts
type LoginForm = z.infer<typeof loginSchema>;
```

Then:

```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

const {
  register,
  handleSubmit,
  formState: { errors },
} = useForm<LoginForm>({
  resolver: zodResolver(loginSchema),
});
```

Now:

```text
TypeScript
     +
React Hook Form
     +
Zod
```

work together.

### 🔥 What exactly happens with RHF + Zod?

Suppose the user enters:

```text
Email: hello
Password: 123
```

Then:

```text
User submits
      ↓
React Hook Form
      ↓
zodResolver
      ↓
Zod
      ↓
┌─────────────────────┐
│ email ❌            │
│ password ❌         │
└─────────────────────┘
      ↓
errors
      ↓
UI displays messages
```

You don't have to manually call:

```ts
loginSchema.safeParse(...)
```

for every form submission.

The resolver connects RHF and Zod.

---

# ----RHF with React Native

### 📱 React Native is slightly different

This is particularly important for you.

On the web:

```tsx
<input {...register("email")} />
```

is easy because RHF's `register()` works naturally with HTML inputs.

In  **React Native** , you generally use:

```tsx
<TextInput />
```

and React Native's `TextInput` isn't an HTML input.

Therefore, for React Native, you'll commonly use  **`Controller`** .

### 🎮  `Controller` in React Native

The pattern looks like:

```tsx
import {
  View,
  TextInput,
  Button,
} from "react-native";

import {
  useForm,
  Controller,
} from "react-hook-form";
```

Then:

```tsx
const {
  control,
  handleSubmit,
} = useForm<LoginForm>();
```

And:

```tsx
<Controller
  control={control}
  name="email"
  render={({ field }) => (
    <TextInput
      value={field.value}
      onChangeText={field.onChange}
      onBlur={field.onBlur}
    />
  )}
/>
```

This is an important React Native-specific pattern.

### 🧠 Why `Controller`?

Think about the difference.

**-- Web**

```text
<input>
   ↓
register()
   ↓
RHF
```

**-- React Native**

```text
<TextInput>
   ↓
Controller
   ↓
RHF
```

`Controller` acts as the bridge between RHF and controlled/custom components.

### 📱 Complete React Native + Zod example

Here's a realistic Expo example.

```tsx
import {
  View,
  Text,
  TextInput,
  Button,
  Text as RNText,
} from "react-native";

import { useForm, Controller } from "react-hook-form";

import { z } from "zod";
import { zodResolver } from "@hookform/resolvers/zod";

const loginSchema = z.object({
  email: z.string().email("Please enter a valid email"),

  password: z
    .string()
    .min(8, "Password must be at least 8 characters"),
});

type LoginForm = z.infer<typeof loginSchema>;

export default function LoginScreen() {
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm<LoginForm>({
    resolver: zodResolver(loginSchema),
    defaultValues: {
      email: "",
      password: "",
    },
  });

  const onSubmit = (data: LoginForm) => {
    console.log(data);
  };

  return (
    <View>
      <Controller
        control={control}
        name="email"
        render={({ field }) => (
          <TextInput
            placeholder="Email"
            value={field.value}
            onChangeText={field.onChange}
            onBlur={field.onBlur}
            autoCapitalize="none"
            keyboardType="email-address"
          />
        )}
      />

      {errors.email && (
        <RNText>
          {errors.email.message}
        </RNText>
      )}

      <Controller
        control={control}
        name="password"
        render={({ field }) => (
          <TextInput
            placeholder="Password"
            value={field.value}
            onChangeText={field.onChange}
            onBlur={field.onBlur}
            secureTextEntry
          />
        )}
      />

      {errors.password && (
        <RNText>
          {errors.password.message}
        </RNText>
      )}

      <Button
        title="Login"
        onPress={handleSubmit(onSubmit)}
      />
    </View>
  );
}
```

That's a pattern you'll want to become comfortable with.

### 🔍 Understanding `field`

This:

```tsx
render={({ field }) => (
```

gives you information/functions related to that particular field.

The important ones are:

```ts
field.value
field.onChange
field.onBlur
field.ref
```

For React Native:

```tsx
<TextInput
  value={field.value}
  onChangeText={field.onChange}
  onBlur={field.onBlur}
/>
```

So:

```text
User types
    ↓
onChangeText
    ↓
field.onChange
    ↓
RHF updates form value
```

### 🧩 `register()` vs `Controller()`

For you, this is probably the most important React Native distinction.

**-- Web native inputs**

```tsx
<input {...register("email")} />
```

**-- React Native**

```tsx
<Controller
  control={control}
  name="email"
  render={({ field }) => (
    <TextInput
      value={field.value}
      onChangeText={field.onChange}
    />
  )}
/>
```

**-- Why?**

Because React Native components aren't HTML form elements.

Also, many third-party/custom components behave similarly:

```text
DatePicker
Dropdown
CustomSelect
Slider
PhoneInput
```

These often work nicely with `Controller`.

---

# ----Default values, watch(), setValues() and getValues

### 🎛️ `defaultValues`

You can specify initial values:

```tsx
const form = useForm<LoginForm>({
  defaultValues: {
    email: "",
    password: "",
  },
});
```

For an edit form:

```tsx
defaultValues: {
  email: "arun@example.com",
  password: "",
}
```

This is especially useful for:

```text
Create form
Edit form
Profile form
Settings form
```

### 👀 `watch()`

`watch()` lets you observe form values.

```tsx
const {
  watch,
} = useForm<LoginForm>();

const email = watch("email");
```

Now:

```text
User types
    ↓
email changes
    ↓
watch("email")
    ↓
new value
```

You can also:

```tsx
const password = watch("password");
```

**-- Practical example**

Suppose a registration screen has:

```text
Account type:
○ Individual
○ Business
```

If:

```text
Business
```

is selected, you want to show:

```text
Company name
GST number
```

You can watch:

```tsx
const accountType = watch("accountType");
```

Then:

```tsx
{accountType === "business" && (
  <>
    {/* Company fields */}
  </>
)}
```

### ✏️  `setValue()`

Sometimes you need to programmatically change a field.

```tsx
setValue("email", "arun@example.com");
```

For example, you might have:

```text
Use my profile email
```

button.

Clicking it:

```tsx
setValue("email", user.email);
```

updates the form.

### 🔎 `getValues()`

`getValues()` retrieves current form values.

```tsx
const values = getValues();
```

You can also get one field:

```tsx
const email = getValues("email");
```

Unlike `watch()`, `getValues()` doesn't subscribe your component to changes.

Think:

```text
watch()
→ continuously observe


getValues()
→ give me the current value now
```

### 🔄 `reset()`

Very useful after successful submission.

```tsx
reset();
```

This returns the form to its default state.

Or:

```tsx
reset({
  email: "",
  password: "",
});
```

For example:

```tsx
const onSubmit = async (data: LoginForm) => {
  await login(data);

  reset();
};
```

### 🧹 `reset()` for editing data

Suppose your screen initially loads a customer:

```ts
const customer = {
  name: "Arun",
  email: "arun@example.com",
};
```

After fetching it:

```tsx
reset(customer);
```

Now the form fields are populated.

This is extremely common in edit forms.

---

# ----`formState`

`formState` contains information about the form.

Important properties include:

```text
errors
isSubmitting
isValid
isDirty
isValidating
touchedFields
dirtyFields
```

You don't need to memorize all of these immediately.

### ⏳ `isSubmitting`

Extremely useful.

```tsx
const {
  handleSubmit,
  formState: { isSubmitting },
} = useForm();
```

Then:

```tsx
<Button
  title={isSubmitting ? "Logging in..." : "Login"}
  disabled={isSubmitting}
  onPress={handleSubmit(onSubmit)}
/>
```

This prevents the user from repeatedly pressing the button while the API request is running.

### 🧹 `isDirty`

`isDirty` tells you whether the user has changed something from the initial value.

For example:

```tsx
const {
  formState: { isDirty },
} = useForm({
  defaultValues: {
    name: "Arun",
  },
});
```

Initially:

```text
isDirty = false
```

User changes:

```text
Arun → Arun S
```

Now:

```text
isDirty = true
```

Useful for:

```text
Save button
Unsaved changes warning
Profile editing
Settings
```

### 👆  `touchedFields`

RHF tracks fields the user has interacted with.

For example:

```text
email
password
```

User touches only email.

You might have:

```ts
touchedFields.email
```

but not:

```ts
touchedFields.password
```

This can help decide when to display certain validation messages.

---

# ----RHF & architecture

### 🧠 RHF is NOT a validation library

This distinction is important.

React Hook Form primarily handles:

```text
Form state
Field registration
Submission
Errors
Form status
```

Zod handles:

```text
Validation rules
Data shape
Business validation
```

So:

```text
              React Hook Form
             /       |       \
            /        |        \
       values      errors    submit
                         │
                         ▼
                        Zod
                         │
                         ▼
                     validation
```

They're complementary.

### 🏗️ A realistic architecture for your stack

For an Expo/React Native app:

```text
┌───────────────────────────────┐
│        React Native UI        │
│                               │
│ TextInput / Select / Button   │
└───────────────┬───────────────┘
                │
                ▼
       ┌──────────────────┐
       │ React Hook Form  │
       │                  │
       │ values           │
       │ errors           │
       │ submission       │
       └────────┬─────────┘
                │
                ▼
          ┌───────────┐
          │   Zod     │
          │ validation│
          └─────┬─────┘
                │
                ▼
             API call
                │
                ▼
          Your backend
                │
                ▼
              Prisma
                │
                ▼
           PostgreSQL
```

That's a very sensible stack.

### 🎯 The mental model I want you to have

Think of  **React Hook Form as the manager of your form** .

```text
                    FORM
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
     Values        Errors        Status
       │             │             │
       │             │             ├─ isSubmitting
       │             │             ├─ isDirty
       │             │             └─ isValid
       │             │
       │             └──── Zod
       │                  validation
       │
       └──── handleSubmit()
                    │
                    ▼
                 API call
```

And  **Zod is the rulebook** :

```text
RHF = manages the form
Zod = validates the form data
```

That's the key distinction.

---

# ----RHF in Next application

### 🌐 What about your Next.js apps?

The same idea works there.

For a web application:

```text
React
 ↓
React Hook Form
 ↓
Zod
 ↓
Server Action / API Route
 ↓
Zod again
 ↓
Prisma
 ↓
PostgreSQL
```

Notice the  **second Zod** .

Frontend validation:

> Good user experience.

Backend validation:

> Protects your backend from untrusted input.

### 🔐 A very important rule

Don't do this:

```text
React Hook Form
     ↓
Zod
     ↓
Prisma
```

and assume the backend is protected.

Instead:

```text
                    FRONTEND
React Hook Form → Zod
                     ↓
                   API
                     ↓
                    BACKEND
                     ↓
                    Zod
                     ↓
                  Prisma
                     ↓
                PostgreSQL
```

The client is never trustworthy.

---

# ----Controlled vs uncontrolled

React forms can be managed in two broad ways.

### Controlled

React state holds the value:

```tsx
const [email, setEmail] = useState("");
```

```tsx
<TextInput
  value={email}
  onChangeText={setEmail}
/>
```

### React Hook Form's preferred approach

RHF generally tries to avoid unnecessary controlled state and uses an  **uncontrolled approach where possible** .

On web:

```tsx
<input {...register("email")} />
```

This is one reason RHF can be performant.

For React Native, because components like `TextInput` commonly need controlled behavior, `Controller` is frequently used.

You don't need to obsess over the internal implementation right now. Just understand the distinction.

---

# ----Learning Recommedations

Don't try to learn the entire library.

**🟢 Essential**

```ts
useForm()
register()
handleSubmit()
formState.errors
```

**🟢 React Native**

```ts
Controller
control
```

**🟢 Very useful**

```ts
defaultValues
reset()
watch()
setValue()
getValues()
```

**🟢 Form status**

```ts
isSubmitting
isDirty
isValid
touchedFields
dirtyFields
```

**🟡 Later**

```text
useFieldArray
useWatch
useFormContext
FormProvider
useController
setError
clearErrors
trigger
```

### 🔥 For YOUR React Native learning path

Since you're using  **Expo + React Native + TypeScript** , I would learn RHF in this order:

```text
1. useForm()
       ↓
2. register()             ← mainly understand for web
       ↓
3. handleSubmit()
       ↓
4. formState.errors
       ↓
5. Controller             ← VERY important for RN
       ↓
6. defaultValues
       ↓
7. reset()
       ↓
8. watch()
       ↓
9. setValue()
       ↓
10. getValues()
       ↓
11. isSubmitting / isDirty / isValid
       ↓
12. useFieldArray()
       ↓
13. FormProvider / useFormContext
```

Then combine it with what you've just learned:

```text
                 Zod
                  │
             validation
                  │
                  ▼
React Hook Form ←────→ React Native
                  │
             form state
                  │
                  ▼
                API
```

### ⭐ The three things I'd especially focus on right now:

**1. `useForm()`** — creates/manages the form.

**2. `Controller`** — connects React Native inputs to RHF.

**3. `zodResolver()`** — connects RHF to Zod.

Once those three click, the whole **React Native forms + Zod** ecosystem becomes much easier to understand.

---

# ----`useFieldArray()` — worth knowing exists

This is for dynamic arrays of fields.

Imagine an invoice:

```text
Product 1
Quantity 1

Product 2
Quantity 2

Product 3
Quantity 3

[+ Add Product]
```

Instead of manually managing the array, RHF has:

```tsx
useFieldArray()
```

Conceptually:

```text
Form
 │
 └── products[]
       ├── product 1
       ├── product 2
       └── product 3
```

This becomes very useful for things like:

* invoices
* order items
* jewellery stones
* family members
* phone numbers
* multiple addresses

You don't need to master it yet, but you'll probably use it eventually.

**-- EXAMPLE**

Imagine you're creating a jewellery invoice.

You don't know beforehand how many items the customer will add:

```text
Item 1: Gold Ring
Item 2: Gold Chain
Item 3: Diamond Necklace
Item 4: Earrings
...
```

Or an order:

```text
Product 1
Product 2
Product 3
...
```

You  **cannot conveniently create a fixed field for every possible item** .

That's exactly what `useFieldArray()` is for.

### Basic idea

```text
useForm()
   ↓
useFieldArray()
   ↓
items[]
   ├── item 1
   ├── item 2
   ├── item 3
   └── ...
```

### 📝 Basic `useFieldArray()` example

Suppose our form has:

```ts
type OrderForm = {
  customerName: string;
  items: {
    product: string;
    quantity: number;
  }[];
};
```

Our form might look like:

```text
Customer name: Arun

Items

Product        Quantity
-----------------------
Gold Ring      1       [Remove]
Gold Chain     2       [Remove]

[+ Add Item]

[Submit]
```

We can implement this with:

```tsx
import { useForm, useFieldArray } from "react-hook-form";

type OrderForm = {
  customerName: string;
  items: {
    product: string;
    quantity: number;
  }[];
};

export default function OrderForm() {
  const {
    control,
    register,
    handleSubmit,
  } = useForm<OrderForm>({
    defaultValues: {
      customerName: "",
      items: [],
    },
  });

  const { fields, append, remove } = useFieldArray({
    control,
    name: "items",
  });

  const onSubmit = (data: OrderForm) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>

      <input
        {...register("customerName")}
        placeholder="Customer name"
      />

      {fields.map((field, index) => (
        <div key={field.id}>

          <input
            {...register(`items.${index}.product`)}
            placeholder="Product"
          />

          <input
            type="number"
            {...register(`items.${index}.quantity`, {
              valueAsNumber: true,
            })}
          />

          <button
            type="button"
            onClick={() => remove(index)}
          >
            Remove
          </button>

        </div>
      ))}

      <button
        type="button"
        onClick={() =>
          append({
            product: "",
            quantity: 1,
          })
        }
      >
        Add Item
      </button>

      <button type="submit">
        Submit
      </button>

    </form>
  );
}
```

### 🔍 3. Understanding `fields`

This is the most important part:

```ts
const { fields, append, remove } = useFieldArray({
  control,
  name: "items",
});
```

`fields` represents the current items in your array.

Initially:

```ts
items: []
```

After:

```ts
append({
  product: "",
  quantity: 1,
});
```

you get something conceptually like:

```ts
fields = [
  {
    id: "abc123",
    product: "",
    quantity: 1
  }
]
```

Add another:

```ts
append({
  product: "",
  quantity: 1,
});
```

Now:

```ts
fields = [
  {
    id: "abc123",
    product: "",
    quantity: 1
  },
  {
    id: "xyz789",
    product: "",
    quantity: 1
  }
]
```

### 🔑 Why `field.id`?

You'll frequently see:

```tsx
{fields.map((field, index) => (
  <div key={field.id}>
```

instead of:

```tsx
<div key={index}>
```

RHF gives every field-array item a unique `id`.

Use:

```tsx
key={field.id}
```

This is important because React needs a stable key when items are added, removed, or reordered.

❌ Avoid

```tsx
key={index}
```

✅ Prefer

```tsx
key={field.id}
```

for `useFieldArray()`.

### ➕ `append()`

Adds an item.

```ts
append({
  product: "",
  quantity: 1,
});
```

For example:

```text
Before:

items
 ├── Ring
 └── Chain

After append():

items
 ├── Ring
 ├── Chain
 └── Necklace
```

### 🗑️ `remove()`

Removes an item by index.

```ts
remove(index);
```

For example:

```ts
remove(1);
```

removes the second item.

### 🔄 Other useful `useFieldArray()` methods

You'll also encounter:

```ts
append()
prepend()
insert()
remove()
swap()
move()
update()
replace()
```

##### `prepend()`

Adds at beginning:

```ts
prepend({
  product: "",
  quantity: 1,
});
```

##### `insert()`

Insert at a particular position:

```ts
insert(1, {
  product: "",
  quantity: 1,
});
```

##### `swap()`

Swap two items:

```ts
swap(0, 1);
```

##### `move()`

Move an item:

```ts
move(2, 0);
```

##### `update()`

Update an existing item:

```ts
update(0, {
  product: "Gold Ring",
  quantity: 2,
});
```

##### `replace()`

Replace the entire array:

```ts
replace([
  {
    product: "Ring",
    quantity: 1,
  },
  {
    product: "Chain",
    quantity: 2,
  },
]);
```

You won't need all of these every day.

For most applications:

```text
append()
remove()
```

are the ones you'll use constantly.

### 💎 A more realistic jewellery example

This is where `useFieldArray()` becomes especially useful for the kind of business software you're interested in.

Suppose a jewellery product has multiple stones:

```ts
type JewelleryForm = {
  name: string;

  stones: {
    type: string;
    weight: number;
    count: number;
  }[];
};
```

The UI:

```text
Product: Diamond Necklace

Stones

Type        Weight       Count
--------------------------------
Diamond     1.20 ct      18      [Remove]
Ruby        0.50 ct       2      [Remove]

[+ Add Stone]
```

You could have:

```tsx
const { fields, append, remove } = useFieldArray({
  control,
  name: "stones",
});
```

Then:

```tsx
{fields.map((field, index) => (
  <div key={field.id}>

    <input
      {...register(`stones.${index}.type`)}
    />

    <input
      type="number"
      {...register(`stones.${index}.weight`, {
        valueAsNumber: true,
      })}
    />

    <input
      type="number"
      {...register(`stones.${index}.count`, {
        valueAsNumber: true,
      })}
    />

    <button
      type="button"
      onClick={() => remove(index)}
    >
      Remove
    </button>

  </div>
))}
```

And:

```tsx
<button
  type="button"
  onClick={() =>
    append({
      type: "",
      weight: 0,
      count: 1,
    })
  }
>
  Add Stone
</button>
```

That's a **real-world use case** for `useFieldArray()`.

---

# ----`useWatch()` — Watch form values

Now let's move to `useWatch()`.

Suppose you have:

```text
Account Type:

○ Individual
○ Business
```

If the user chooses:

```text
Business
```

you want to display:

```text
Company Name
GST Number
Business Address
```

If they choose:

```text
Individual
```

you don't want those fields.

You could use `watch()`:

```ts
const accountType = watch("accountType");
```

But RHF also provides:

```ts
useWatch()
```

which is particularly useful when you want to watch form values  **inside components** .

### 🧠 `watch()` vs `useWatch()`

This distinction is important.

**-- `watch()`**

comes from:

```ts
const { watch } = useForm();
```

Example:

```ts
const accountType = watch("accountType");
```

**-- `useWatch()`**

is a separate hook:

```ts
import { useWatch } from "react-hook-form";
```

Example:

```ts
const accountType = useWatch({
  control,
  name: "accountType",
});
```

> **IT SAYS: "I want to react to a value changing."**

Conceptually:

```text
watch()
 ↓
watch values from the form

useWatch()
 ↓
watch values from the form
but allows more targeted subscriptions
```

This becomes particularly useful in  **large forms** .

**EXAMPLE:**

```typescript
function PaymentSection() {

  const { control, register } =
    useFormContext<OrderForm>();

  const paymentMethod = useWatch({
    control,
    name: "paymentMethod",
  });

  return (
    <section>

      <h2>Payment</h2>

      <select {...register("paymentMethod")}>
        <option value="cash">
          Cash
        </option>

        <option value="upi">
          UPI
        </option>

        <option value="card">
          Card
        </option>
      </select>

      {paymentMethod === "upi" && (
        <input
          placeholder="UPI ID"
        />
      )}

      {paymentMethod === "card" && (
        <input
          placeholder="Card holder name"
        />
      )}

    </section>
  );
}

```

### 🎯 Why `useWatch()` is useful

Imagine this huge form:

```text
Customer Information
        ↓
Billing Information
        ↓
Shipping Information
        ↓
Payment Information
        ↓
Products
        ↓
Discount
        ↓
Notes
```

Suppose only the payment component cares about:

```ts
paymentMethod
```

Instead of having the entire form component react to every value change, you can make a small component watch only:

```ts
paymentMethod
```

Example:

```tsx
function PaymentSection({ control }: { control: Control<OrderForm> }) {

  const paymentMethod = useWatch({
    control,
    name: "paymentMethod",
  });

  return (
    <>
      {paymentMethod === "card" && (
        <CardPaymentFields />
      )}

      {paymentMethod === "upi" && (
        <UPIPaymentFields />
      )}
    </>
  );
}
```

That's a very useful pattern.

### 🔀 Watching multiple values

You can watch multiple fields:

```ts
const [quantity, price] = useWatch({
  control,
  name: ["quantity", "price"],
});
```

Then:

```ts
const total = quantity * price;
```

For example:

```text
Quantity: 3
Price: ₹500

Total: ₹1500
```

### 🧮 Real-world example: invoice total

Suppose:

```ts
type InvoiceForm = {
  quantity: number;
  price: number;
  discount: number;
};
```

You could do:

```tsx
function InvoiceSummary({ control }: { control: Control<InvoiceForm> }) {
  const [quantity, price, discount] = useWatch({
    control,
    name: ["quantity", "price", "discount"],
  });

  const subtotal = quantity * price;
  const total = subtotal - discount;

  return (
    <Text>
      Total: ₹{total}
    </Text>
  );
}
```

Now the summary automatically updates whenever:

```text
quantity changes
      ↓
useWatch detects it
      ↓
component re-renders
      ↓
new total
```

This is one of the nicest uses of `useWatch()`.

---

# ----FormProvider — Share the form with children

Now imagine your form becomes large.

Instead of one component:

```tsx
<OrderForm />
```

you have:

```text
OrderForm
│
├── CustomerSection
│
├── ProductSection
│
├── PaymentSection
│
└── NotesSection
```

You don't want to constantly pass:

```tsx
control
register
setValue
watch
errors
...
```

through every component.

For example:

```tsx
<OrderForm
  control={control}
  register={register}
  errors={errors}
  setValue={setValue}
  watch={watch}
/>
```

Then:

```tsx
<CustomerSection
  control={control}
  register={register}
  errors={errors}
/>
```

Then deeper:

```tsx
<CustomerAddress
  control={control}
  register={register}
/>
```

This is called  **prop drilling** .

`FormProvider` solves this.

### 🌳 `FormProvider` mental model

Think of it as putting your entire RHF form into a React Context.

```text
FormProvider
│
├── CustomerSection
│    └── CustomerAddress
│
├── ProductSection
│    └── ProductList
│
└── PaymentSection
```

Every descendant can access the form.

### 🧩 Basic `FormProvider` example

First:

```tsx
const methods = useForm<OrderForm>();
```

Then:

```tsx
<FormProvider {...methods}>
  <form onSubmit={methods.handleSubmit(onSubmit)}>
    <CustomerSection />
    <ProductSection />
    <PaymentSection />

    <button type="submit">
      Submit
    </button>
  </form>
</FormProvider>
```

Notice:

```tsx
<FormProvider {...methods}>
```

You're essentially making the RHF methods available to descendants.

### 🔌 `useFormContext()` — Access the form from children

Now inside:

```tsx
<CustomerSection />
```

you can do:

```tsx
import { useFormContext } from "react-hook-form";

function CustomerSection() {
  const {
    register,
    formState: { errors },
  } = useFormContext<OrderForm>();

  return (
    <>
      <input
        {...register("customerName")}
      />

      {errors.customerName && (
        <p>{errors.customerName.message}</p>
      )}
    </>
  );
}
```

You didn't pass:

```tsx
register
errors
```

as props.

The component gets them from the `FormProvider`.

### 🔗 The relationship between the two

This is extremely important:

**-- `FormProvider`**

**provides** the form.

**-- `useFormContext()`**

**consumes** the form.

Think:

```text
useForm()
   ↓
FormProvider
   ↓
useFormContext()
   ↓
Child component
```

Or:

```text
Provider = put the form into the context

Context = retrieve the form from the context
```

### 🧱 Complete example

Let's build:

```text
OrderForm
│
├── CustomerSection
│
├── ItemsSection
│
└── PaymentSection
```

**Parent**

```tsx
import {
  useForm,
  FormProvider,
} from "react-hook-form";

type OrderForm = {
  customerName: string;

  items: {
    product: string;
    quantity: number;
  }[];

  paymentMethod: "cash" | "upi" | "card";
};

export default function OrderForm() {

  const methods = useForm<OrderForm>({
    defaultValues: {
      customerName: "",
      items: [],
      paymentMethod: "cash",
    },
  });

  const onSubmit = (data: OrderForm) => {
    console.log(data);
  };

  return (
    <FormProvider {...methods}>

      <form onSubmit={methods.handleSubmit(onSubmit)}>

        <CustomerSection />

        <ItemsSection />

        <PaymentSection />

        <button type="submit">
          Submit
        </button>

      </form>

    </FormProvider>
  );
}
```

##### 👤 CustomerSection

```tsx
function CustomerSection() {

  const {
    register,
    formState: { errors },
  } = useFormContext<OrderForm>();

  return (
    <section>

      <h2>Customer</h2>

      <input
        {...register("customerName", {
          required: "Customer name is required",
        })}
      />

      {errors.customerName && (
        <p>
          {errors.customerName.message}
        </p>
      )}

    </section>
  );
}
```

No props needed.

##### 📦  ItemsSection + `useFieldArray()`

```tsx
function ItemsSection() {

  const { control, register } =
    useFormContext<OrderForm>();

  const {
    fields,
    append,
    remove,
  } = useFieldArray({
    control,
    name: "items",
  });

  return (
    <section>

      <h2>Items</h2>

      {fields.map((field, index) => (

        <div key={field.id}>

          <input
            {...register(
              `items.${index}.product`
            )}
            placeholder="Product"
          />

          <input
            type="number"
            {...register(
              `items.${index}.quantity`,
              {
                valueAsNumber: true,
              }
            )}
          />

          <button
            type="button"
            onClick={() => remove(index)}
          >
            Remove
          </button>

        </div>

      ))}

      <button
        type="button"
        onClick={() =>
          append({
            product: "",
            quantity: 1,
          })
        }
      >
        Add Item
      </button>

    </section>
  );
}
```

Notice the combination:

```text
FormProvider
     ↓
useFormContext()
     ↓
control
     ↓
useFieldArray()
```

That's a very common architecture.

### 💳 PaymentSection + `useWatch()`

Now:

```tsx
function PaymentSection() {

  const { control, register } =
    useFormContext<OrderForm>();

  const paymentMethod = useWatch({
    control,
    name: "paymentMethod",
  });

  return (
    <section>

      <h2>Payment</h2>

      <select {...register("paymentMethod")}>
        <option value="cash">
          Cash
        </option>

        <option value="upi">
          UPI
        </option>

        <option value="card">
          Card
        </option>
      </select>

      {paymentMethod === "upi" && (
        <input
          placeholder="UPI ID"
        />
      )}

      {paymentMethod === "card" && (
        <input
          placeholder="Card holder name"
        />
      )}

    </section>
  );
}
```

Now we have all four concepts working together.

### 🧠 The complete mental model

Remember this diagram:

```text
                     useForm()
                        │
                        ▼
                 ┌──────────────┐
                 │ FormProvider │
                 └──────┬───────┘
                        │
          ┌─────────────┼──────────────┐
          ▼             ▼              ▼
     Customer       Products        Payment
          │             │              │
          │             │              │
 useFormContext()  useFormContext()  useFormContext()
                        │
                        ▼
                 useFieldArray()
                        │
                        ▼
                   items[]
                        │
                        ▼
                     fields

Payment
   │
   ▼
useWatch()
   │
   ▼
paymentMethod
   │
   ▼
conditional UI
```

### 📱What changes in React Native?

The concepts remain the same.

The major difference is that RN normally uses:

```tsx
Controller
```

rather than web:

```tsx
register()
```

For example:

```tsx
function CustomerSection() {

  const { control } =
    useFormContext<OrderForm>();

  return (
    <Controller
      control={control}
      name="customerName"
      render={({ field }) => (
        <TextInput
          value={field.value}
          onChangeText={field.onChange}
          onBlur={field.onBlur}
        />
      )}
    />
  );
}
```

And you can still combine:

```text
FormProvider
     ↓
useFormContext()
     ↓
Controller
```

or:

```text
FormProvider
     ↓
useFormContext()
     ↓
useFieldArray()
```

or:

```text
FormProvider
     ↓
useFormContext()
     ↓
useWatch()
```

All of these work together in React Native.

---

# ----`setError() `— Manually set a form error

Normally, errors are generated automatically by:

```text
Zod
  ↓
resolver
  ↓
React Hook Form
  ↓
formState.errors
```

But sometimes  **you discover an error yourself** , especially after an API request.

For example, the user enters:

```text
Email: arun@example.com
```

Your frontend validation says it's valid.

You send it to the backend.

Backend responds:

```json
{
  "message": "Email already exists"
}
```

RHF doesn't automatically know that.

You can manually create the error:

```ts
setError("email", {
  type: "server",
  message: "This email is already registered",
});
```

Now:

```ts
formState.errors.email
```

contains the error.

### 🔥 Basic `setError()` example

```tsx
const {
  register,
  setError,
  formState: { errors },
} = useForm<LoginForm>();
```

Then:

```ts
setError("email", {
  type: "manual",
  message: "This email is already registered",
});
```

Display it:

```tsx
{errors.email && (
  <Text>
    {errors.email.message}
  </Text>
)}
```

So:

```text
setError()
    ↓
errors.email
    ↓
UI displays message
```

### 🌐 The most important use: server errors

This is probably the most useful real-world use of `setError()`.

Suppose:

```tsx
const onSubmit = async (data: LoginForm) => {
  try {
    await login(data);
  } catch (error) {

    setError("email", {
      type: "server",
      message: "Email or password is incorrect",
    });

  }
};
```

Now your UI can show:

```text
Email
[ arun@example.com ]

❌ Email or password is incorrect
```

This is especially useful for:

```text
Email already exists
Username already taken
Invalid coupon
Incorrect password
Invalid OTP
Insufficient balance
Product unavailable
```

### 🎯  `setError()` doesn't have to correspond to Zod

Suppose your Zod schema says:

```ts
const schema = z.object({
  email: z.string().email(),
});
```

Zod may say:

```text
email = valid
```

But your backend may say:

```text
email = already registered
```

That's not necessarily a  **schema validation problem** .

It's a  **business/server validation problem** .

So:

```text
Zod
 ↓
"Is this structurally valid?"

Server
 ↓
"Can this operation actually happen?"
```

`setError()` lets you bring the server's answer back into RHF's error system.

### 🧠 `setError()` with a nested field

Suppose:

```ts
type Form = {
  user: {
    email: string;
  };
};
```

You can:

```ts
setError("user.email", {
  type: "server",
  message: "Email already exists",
});
```

### 🚨 `setError("root...")` — Form-level errors

Sometimes the error doesn't belong to one particular field.

For example:

```text
❌ Payment failed. Please try again.
```

You can use a root error:

```ts
setError("root.server", {
  type: "server",
  message: "Payment failed. Please try again.",
});
```

Then:

```tsx
{errors.root?.server && (
  <Text>
    {errors.root.server.message}
  </Text>
)}
```

This is useful for:

```text
Login failed
Payment failed
Something went wrong
Unable to save
Server unavailable
```

Think:

```text
setError("email")
       ↓
field-specific error

setError("root.server")
       ↓
form-level error
```

---

# ----`clearErrors()` — Remove errors

If `setError()` adds an error:

```text
setError()
   ↓
ADD error
```

then:

```ts
clearErrors()
```

removes errors.

### 🧹 Basic `clearErrors()`

```ts
clearErrors("email");
```

This removes the email error.

For example:

```ts
setError("email", {
  type: "server",
  message: "Email already exists",
});
```

Later:

```ts
clearErrors("email");
```

Now:

```text
errors.email
    ↓
removed
```

### 🧹 Clear multiple errors

You can specify multiple fields:

```ts
clearErrors(["email", "password"]);
```

### 🧹 Clear all errors

```ts
clearErrors();
```

This clears all current errors.

Be careful with this in large forms because you may unintentionally remove errors the user still needs to see.

### 🔄 `setError()` + `clearErrors()` together

A common pattern:

```tsx
const checkUsername = async (username: string) => {

  const available = await checkUsernameAvailability(username);

  if (!available) {
    setError("username", {
      type: "server",
      message: "Username is already taken",
    });
  } else {
    clearErrors("username");
  }
};
```

Conceptually:

```text
Check username
      ↓
Available?
 ┌────┴────┐
Yes        No
 ↓          ↓
clearError setError
```

---

# ----`trigger()` — Manually run validation

Normally validation happens when RHF decides it should:

```text
submit
blur
change
```

depending on your:

```ts
mode
```

But sometimes you want to say:

> "Validate this field RIGHT NOW."

That's:

```ts
trigger()
```

### 🎯 Basic `trigger()`

Suppose:

```ts
const {
  trigger,
} = useForm<FormValues>();
```

You can:

```ts
await trigger("email");
```

This manually validates the email field.

It returns:

```ts
true
```

if valid, or:

```ts
false
```

if invalid.

Example:

```ts
const isValid = await trigger("email");

if (isValid) {
  console.log("Email is valid");
}
```

### 🔍 Validate multiple fields

```ts
const isValid = await trigger([
  "email",
  "password",
]);
```

Now RHF validates both.

### 🔍 Validate the entire form

```ts
const isValid = await trigger();
```

This triggers validation for the entire form.

For example:

```ts
const handleNext = async () => {

  const valid = await trigger();

  if (!valid) {
    return;
  }

  // Move to next screen
};
```

This is particularly useful for  **multi-step forms** .

### 📱 Real-world React Native multi-step form

Imagine:

```text
Step 1
Personal Information
    ↓
Step 2
Address
    ↓
Step 3
Payment
```

You don't want to submit the whole form when moving from Step 1 to Step 2.

Instead:

```ts
const nextStep = async () => {

  const valid = await trigger([
    "firstName",
    "lastName",
    "phone",
  ]);

  if (!valid) {
    return;
  }

  setStep(2);
};
```

Now:

```text
User presses Next
        ↓
trigger()
        ↓
Validate Step 1
        ↓
   ┌────┴────┐
 Invalid    Valid
    ↓          ↓
 Show errors  Step 2
```

This is an  **excellent real-world use of `trigger()`** .

### 🔄 `trigger()` + `useWatch()`

These can work together.

Suppose:

```text
Country = India
State = Kerala
```

You change country.

You may want to revalidate:

```ts
await trigger("state");
```

For example:

```ts
const country = useWatch({
  control,
  name: "country",
});
```

Then:

```ts
useEffect(() => {
  trigger("state");
}, [country, trigger]);
```

Now when country changes, the dependent field can be revalidated.

### 🧠 `trigger()` vs `handleSubmit()`

This is another important distinction.

**-- `trigger()`**

> "Validate the form, but don't submit."

```ts
const valid = await trigger();
```

**-- `handleSubmit()`**

> "Validate and, if valid, execute my submit handler."

```tsx
<form onSubmit={handleSubmit(onSubmit)}>
```

So:

```text
trigger()
   ↓
validation only
```

while:

```text
handleSubmit()
   ↓
validation
   ↓
if valid
   ↓
onSubmit(data)
```

---

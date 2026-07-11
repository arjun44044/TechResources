---
# ----Aggregation Pipeline Operator: `$hour`, `$group and 2 $project in one aggregation`  with Example--Project[Fitlab]

###### getHourlySalesOfDay controller---

```javascript

const mongoose = require('mongoose');
const Order = require('../models/orderModel');

const getHourlySalesOfDay = async (req, res) => {
  try {
    const { date } = req.query;

    if (!date) {
      return res.status(400).json({ message: 'Please provide a date in YYYY-MM-DD format.' });
    }

    const start = new Date(date);
    const end = new Date(date);
    end.setDate(end.getDate() + 1);

    const salesData = await Order.aggregate([
      {
        $match: {
          orderDate: { $gte: start, $lt: end },
          orderStatus: { $nin: ['cancelled', 'refunded'] }
        }
      },
      {
        $project: {
          hour: { $hour: '$orderDate' },
          total: '$absoluteTotalWithTaxes'
        }
      },
      {
        $group: {
          _id: '$hour',
          totalSales: { $sum: '$total' }
        }
      },
      {
        $project: {
          _id: 0,
          hour: '$_id',
          totalSales: 1
        }
      }
    ]);

    // Fill in missing hours with zero sales
    const fullDay = Array.from({ length: 24 }, (_, i) => {
      const found = salesData.find(s => s.hour === i);
      return {
        hour: `${i.toString().padStart(2, '0')}:00`,
        totalSales: found ? found.totalSales : 0
      };
    });

    res.status(200).json(fullDay);
  } catch (err) {
    console.error('Hourly sales error:', err);
    res.status(500).json({ message: 'Server error while fetching hourly sales' });
  }
};

module.exports = { getHourlySalesOfDay };

```

Here
1️⃣ `$project` stage:

```javascript
{
  $project: {
    hour: { $hour: '$orderDate' },
    total: '$absoluteTotalWithTaxes'
  }
}

```

**What it does:**

* Extracts the **hour (0–23)** from each order's `orderDate` using `$hour`.
* Picks the `absoluteTotalWithTaxes` (which is the full revenue of an order) and stores it as `total`.

> ` $hour` is a **MongoDB aggregation operator** used to **extract the hour (0–23)** from a date field.
>
> It pulls just the **hour** from a full `Date` object.
>
> SYNTAX ----- `{ $hour: `<dateExpression>````````` }
>
> ***It returns a number between `0` (midnight) and `23` (11 PM***
>
> If let's say the orderDate is -- 2025-04-29T13:45:00Z
>
> then, `{ $project: { hour: { $hour: "$orderDate" } } }`
>
> The output will be - `{ hour: 13 }`
>
> ### 🔧 Other similar operators:
>
> * `$minute`: Gets minute (0–59)
> * `$second`: Gets second (0–59)
> * `$dayOfMonth`, `$month`, `$year`, `$dayOfWeek`: Extract respective parts from a date

🔸 After this stage, each document looks like--

`{ hour: 13, total: 2450 }`

2️⃣ `$group` stage:

```javascript
{
  $group: {
    _id: '$hour',
    totalSales: { $sum: '$total' }
  }
}

```

**What it does:**

* Groups documents by the `hour` field.
* Sums up all `total` values (order revenue) for each hour.
* The result gives total sales for each hour of the day.

🔸 After this stage, documents look like:

`{ _id: 13, totalSales: 8340 }`

3️⃣ `$project` stage:

```javascript
{
  $project: {
    _id: 0,
    hour: '$_id',
    totalSales: 1
  }
}

```

**What it does:**

* Renames `_id` (which holds the hour) to `hour`.
* Keeps `totalSales` as-is.
* Removes `_id` (makes the output cleaner).

🔸 After this stage, the final output looks like:

`{ hour: 13, totalSales: 8340 }`

###### 🧠 Final Output:

After all stages, you get an array like this (partial):

```javascript
[
  { hour: 0, totalSales: 1200 },
  { hour: 1, totalSales: 900 },
  { hour: 13, totalSales: 8340 },
  ...
]

```

### `$dayOfMonth` , `$dayOfWeek`, `$month `, `$year` Aggregation Operators

###### 📘 Scenario:

You have a MongoDB collection named `orders`, and each document has a field `createdAt` which is a `Date`.

```javascript
// Example document in 'orders' collection
{
  _id: ObjectId("..."),
  customerName: "John Doe",
  totalAmount: 99.99,
  createdAt: ISODate("2025-04-28T10:00:00Z")
}

```

EXAMPLE CODE--

```javascript
db.orders.aggregate([
  {
    $project: {
      customerName: 1,
      totalAmount: 1,
      createdAt: 1,
      day: { $dayOfMonth: "$createdAt" },
      month: { $month: "$createdAt" },
      year: { $year: "$createdAt" },
      weekday: { $dayOfWeek: "$createdAt" }
    }
  }
]);

```

🧾 Output Example:

```javascript
{
  _id: ObjectId("..."),
  customerName: "John Doe",
  totalAmount: 99.99,
  createdAt: ISODate("2025-04-28T10:00:00Z"),
  day: 28,
  month: 4,
  year: 2025,
  weekday: 2 // (1 = Sunday, 7 = Saturday)
}

```

###### 💡 Explanation:

* `$dayOfMonth`: Extracts the day of the month (1–31)
* `$month`: Extracts the month (1–12)
* `$year`: Extracts the year (e.g., 2025)
* `$dayOfWeek`: Extracts the day of the week (1 = Sunday, 7 = Saturday)
---
# ----Aggregation- `$unwind- $group- $project` with Example --Project[Fitlab]

Below is getRevenueByMainCategory() controller

```javascript
const getRevenueByMainCategory = async (req, res, next) => {
  try {
    console.log("Inside getRevenueByMainCategory")

    const categoryDatas = await Order.aggregate([
      {
        $match: {
          orderStatus: 'delivered'
        }
      },
      {
        $unwind: '$products'
      },
      {
        $unwind: '$products.category'
      },
      {
        $group: {
          _id: '$products.category',
          revenue: { $sum: '$products.total' }
        }
      },
      {
        $project: {
          _id: 0,
          name: '$_id',
          revenue: 1
        }
      }
    ]);

    console.log("categoryDatas--->", categoryDatas)

    res.status(200).json({categoryDatas})
  }
  catch(error){
    console.error('Error fetching revenue by category:', error.message)
    next(error)
  }
}

```

with Example--Project[Fitlab]

##### 🧠 Explanation

1. **$match** : Filters orders to include only those with an `orderStatus` of `'delivered'`.
2. **$unwind: '$products'** : Deconstructs the `products` array in each order document, creating a separate document for each product.
3. **$unwind: '$products.category'** : Further deconstructs the `category` array within each product, allowing aggregation by individual categories.
4. **$group** : Groups the documents by category name (`$products.category `) and calculates the total revenue for each category by summing the `total` field of the products.
5. **$project** : Formats the output to include `name` (category name) and `revenue`, excluding the `_id` field.

##### 📊 Sample Output

```javascript
[
  { "name": "Cardio Equipment", "revenue": 35000 },
  { "name": "Supplements", "revenue": 20000 },
  { "name": "Accessories", "revenue": 15000 }
]

```

---

# ----Aggregation- `$facet- $count- $project- $addFields- $cond- $round- $multiply` with Example --Project[Fitlab]

**getOrderStats() Controller that return total orders, pending orders and fulfillment rate**

```javascript
const Order = require('../models/orderModel');

const getOrderStats = async (req, res) => {
  try {
    const stats = await Order.aggregate([
      {
        $facet: {
          totalOrders: [{ $count: "count" }],
          deliveredOrders: [
            { $match: { orderStatus: "delivered" } },
            { $count: "count" }
          ],
          pendingOrders: [
            { $match: { orderStatus: "processing" } },
            { $count: "count" }
          ]
        }
      },
      {
        $project: {
          total: { $ifNull: [{ $arrayElemAt: ["$totalOrders.count", 0] }, 0] },
          delivered: { $ifNull: [{ $arrayElemAt: ["$deliveredOrders.count", 0] }, 0] },
          pending: { $ifNull: [{ $arrayElemAt: ["$pendingOrders.count", 0] }, 0] }
        }
      },
      {
        $addFields: {
          fulfillmentRate: {
            $cond: [
              { $eq: ["$total", 0] },
              0,
              {
                $round: [
                  { $multiply: [{ $divide: ["$delivered", "$total"] }, 100] },
                  2
                ]
              }
            ]
          }
        }
      }
    ]);

    res.status(200).json(stats[0]);
  } catch (error) {
    console.error("Error fetching order stats:", error);
    res.status(500).json({ error: "Internal server error" });
  }
};

```

🔍 Output Sample

```javascript
{
  "total": 200,
  "delivered": 150,
  "pending": 30,
  "fulfillmentRate": 75
}

```

* Let's take the first pipeline-- that of `$facet`

The `$facet` stage allows  **multiple sub-pipelines to run in parallel on the same input set of documents** . It returns a **single document** with each key representing a sub-pipeline.

`{ $count: "count" }`

This aggregation stage:

* **Counts** the number of documents in the current pipeline (i.e. how many matched the previous filters).
* **Stores** that number in a field called `"count"`.
* If 150 documents reach this stage, you'll get: `[{ count: 150 }]`

`totalOrders: [{ $count: "count" }] `

* This simply **counts all orders** in the collection.
* Output: `{ totalOrders: [ { count: 500 } ] }`

```javascript
deliveredOrders: [
  { $match: { orderStatus: "delivered" } },
  { $count: "count" }
]

```

* Filters only orders where `orderStatus` is `"delivered"`.
* Then counts how many matched.
* Output: `{ deliveredOrders: [ { count: 300 } ] }`

SIMILARLY---- Output for pending orders --- ``{ pendingOrders: [ { count: 50 } ] }` `` `

🧠 What `$facet` returns-- It returns an object like this:

```javascript
{
  "totalOrders": [{ "count": 500 }],
  "deliveredOrders": [{ "count": 300 }],
  "pendingOrders": [{ "count": 50 }]
}

```

## ✅ Why use `$facet`?

Because it allows you to:

* Run **multiple groupings or filters at once**
* Avoid multiple queries
* Get  **total** ,  **delivered** , and **pending** stats in **one go**

> **But since each of these values is wrapped inside an array, you need to "unwrap" them using `$arrayElemAt` in the next step of the pipeline.**

**Now to the next pipeline---**

```javascript
{
  $project: {
    total: {
      $ifNull: [{ $arrayElemAt: ["$totalOrders.count", 0] }, 0]
    },
    delivered: {
      $ifNull: [{ $arrayElemAt: ["$deliveredOrders.count", 0] }, 0]
    },
    pending: {
      $ifNull: [{ $arrayElemAt: ["$pendingOrders.count", 0] }, 0]
    }
  }
}

```

Since `totalOrders`, deliveredOrders, pedingOrders is an array, to extract just the number `from count`, we use:

`{ $arrayElemAt: ["$totalOrders.count", 0] }  // gives 500`

## 🟧 2. `$ifNull: [ ..., 0 ]`

* In case the array is empty (i.e. there were no matching documents), `$arrayElemAt` will return `null` or `undefined`.
* To handle that safely, we wrap it in `$ifNull`.

SYNTAX--  `{ $ifNull: [ <expression>``, 0 ] }`

This means:

→ "If the expression (like `$arrayElemAt`) gives null, return `0` instead."

So if no orders matched `orderStatus: 'delivered'`, you won’t get an error or null — you’ll just get: `     delivered: 0`

## **Now to the next pipeline-----**

```javascript
{
  $addFields: {
    fulfillmentRate: {
      $cond: [
        { $eq: ["$total", 0] },
        0,
        {
          $round: [
            {
              $multiply: [
                { $divide: ["$delivered", "$total"] },
                100
              ]
            },
            2
          ]
        }
      ]
    }
  }
}

```

###### ❓ What is this trying to do?

It calculates the **fulfillment rate** as a percentage:

> **Fulfillment Rate** = (Delivered Orders ÷ Total Orders) × 100
>
> …rounded to 2 decimal places

But with a safety check to avoid division by zero.

`$cond`: a conditional (like `if`)

SYNTAX---- `$cond: [ <if condition>``, <then>``, <else>`` ]`

So here--

```javascript
$cond: [
  { $eq: ["$total", 0] },
  0,
  <calculate fulfillment>
]

```

Means:

* If `total` is 0 → set `fulfillmentRate` to `0`
* Else → calculate it normally

#### ➗ 2. `$divide: ["$delivered", "$total"]`

This does:

> `delivered ÷ total`
>
> For example: `80 ÷ 120 = 0.666...`

#### ✖️ 3. `$multiply: [ result, 100 ]`

Multiplies the result by 100 to get a percentage:

> `0.666... × 100 = 66.67`

#### 🔄 4. `$round: [ number, 2 ]`

Rounds the number to  **2 decimal places** :

> `66.666... → 66.67`

This will result in an added field like:

`fulfillmentRate: 66.67`

## ✅ **Why is `$addFields` used here?**

`$addFields` is a stage in the MongoDB aggregation pipeline that **adds new fields** to the documents  **without removing the existing ones** .

```javascript
{
  $addFields: {
    fulfillmentRate: { ... }
  }
}

```

This tells MongoDB:

> "Take each document you're processing, and  **add a new field called `fulfillmentRate`** , whose value is calculated from other fields (like `delivered` and `total`)."

### 💡 Why not just use `$project`?

`$project` **replaces** the document fields — so you'd need to manually re-specify all fields you want to keep.

But with `$addFields`:

* You **keep all existing fields** (like `total`, `pending`, `delivered`)
* And you  **add one more** : `fulfillmentRate`

---

# ----Aggregation- `date, $match- $cond- if then else $project- $group- $count `with Example --Project[Fitlab]

**-- getMonthlyOrderStats() controller that generates the monthly breakdown of order statuses (`delivered`, `pending`, `cancelled`, `shipped`, `refunded`) for the current year**

```javascript
const Order = require('../models/orderModel');

const getMonthlyOrderStats = async (req, res) => {
  try {
    const now = new Date();
    const startOfYear = new Date(now.getFullYear(), 0, 1);  // Jan 1st, 00:00:00
    const endOfYear = new Date(now.getFullYear(), 11, 31, 23, 59, 59, 999);  // Dec 31st, 23:59:59.999

    const rawStats = await Order.aggregate([
      {
        $match: {
          createdAt: {
            $gte: startOfYear,
            $lte: endOfYear
          }
        }
      },
      {
        $project: {
          month: { $month: "$createdAt" },
          status: {
            $cond: {
              if: { $in: ["$orderStatus", ["processing", "confirmed"]] },
              then: "pending",
              else: "$orderStatus"
            }
          }
        }
      },
      {
        $group: {
          _id: {
            month: "$month",
            status: "$status"
          },
          count: { $sum: 1 }
        }
      }
    ]);

    const months = [
      "Jan", "Feb", "Mar", "Apr", "May", "Jun",
      "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    ];

    // Pre-fill 12 months with zero counts
    const finalStats = months.map((name) => ({
      name,
      delivered: 0,
      pending: 0,
      cancelled: 0,
      shipped: 0,
      refunded: 0
    }));

    rawStats.forEach(({ _id, count }) => {
      const monthIndex = _id.month - 1;
      const status = _id.status;
      if (finalStats[monthIndex][status] !== undefined) {
        finalStats[monthIndex][status] = count;
      }
    });

    res.status(200).json(finalStats);
  } catch (error) {
    console.error("Error fetching monthly order stats:", error);
    res.status(500).json({ message: "Internal server error" });
  }
};

module.exports = { getMonthlyOrderStats };
```

✅ Controller Logic Outline

1. Filter orders from the current year.
2. Group them by month.
3. Count how many orders fall under each status.
4. Return an array of 12 months with those counts.

SAMPLE OUTPUT----

```javascript
[
  { name: "Jan", completed: 145, pending: 23, canceled: 12 },
  { name: "Feb", completed: 158, pending: 25, canceled: 10 },
  { name: "Mar", completed: 162, pending: 28, canceled: 14 },
  { name: "Apr", completed: 175, pending: 30, canceled: 15 },
  .................................
]
```

🧠 Notes

* The aggregation groups by **month** and  **status** , and sums the counts.
* We map over all 12 months to ensure  **every month exists** , even if count is 0.

---

# ----Aggregation- `date, $unwind- $sort- $limit- $group- $project `with Example --Project[Fitlab]

-- getTopFiveProductByOrders() is a  controller to get the **5 most purchased products** based on how many **orders** they appeared in —  **not just quantities** , but **unique orders** that included the product.

```javascript
const getTopFiveProductsByOrders = async (req, res, next) => {
  try {
    console.log("Inside getTopFiveProductsByOrders")

    const result = await Order.aggregate([
      { $unwind: "$products" }, 
      {
        $group: {
          _id: {
            productId: "$products.productId",
            title: "$products.title"
          },
          orderCount: { $sum: 1 } 
        }
      },
      {
        $sort: { orderCount: -1 }
      },
      {
        $limit: 5
      },
      {
        $project: {
          _id: 0,
          product: "$_id.title",
          orders: "$orderCount"
        }
      }
    ])

    res.status(200).json({topProductDatas: result});
  }
  catch(error){
    console.error("Error getting top 5 products:", error.message)
    next(error)
  }
}
```

### 🔍 Explanation

* `unwind`: flattens the `products` array so each product is treated individually.
* `group`: groups by `productId` and `title`, then counts how many times it appears.
* `sort`: orders by count descending.
* `limit`: takes top 5.
* `project`: formats the final result like:

---

# ----Aggregation- `$lookup, $limit- $sort- $unwind $group $project- $match- $count `with Example --Project[Fitlab]

--getTopVIPCustomers()  is a controller to get top 5 VIP customers

```javascript
const getTopVIPCustomers = async (req, res) => {
  try {
    console.log("Inside getTopVIPCustomers")

    const vipThreshold = 300000; 

    const vipCustomerDatas = await Order.aggregate([
      {
        $match: {
          orderStatus: { $in: ['delivered', 'confirmed', 'shipped'] },
        }
      },
      {
        $group: {
          _id: '$userId',
          orders: { $sum: 1 },
          spent: { $sum: '$absoluteTotalWithTaxes' }
        }
      },
      {
        $match: {
          spent: { $gte: vipThreshold }
        }
      },
      {
        $sort: { spent: -1 }
      },
      {
        $limit: 5
      },
      {
        $lookup: {
          from: 'users', 
          localField: '_id',
          foreignField: '_id',
          as: 'user'
        }
      },
      {
        $unwind: '$user'
      },
      {
        $match: {
          'user.isAdmin': false 
        }
      },
      {
        $project: {
          _id: 0,
          name: '$user.username',
          orders: 1,
          spent: 1
        }
      }
    ])
    console.log("vipCustomerDatas---->", vipCustomerDatas)

    res.status(200).json({vipCustomerDatas});
  }
  catch(error){
    console.error('Error fetching VIP customers:', error.message)
    next(error)
  }
}
```

Now let's take the lookup part---

```javascript
{
  $lookup: {
    from: 'users',
    localField: '_id',
    foreignField: '_id',
    as: 'user'
  }
},
{ $unwind: '$user' },
{
  $match: {
    'user.isAdmin': false // ✅ Exclude admins
  }
},
{
  $project: {
    _id: 0,
    name: '$user.username',
    orders: 1,
    spent: 1
  }
}

```

Here--

```javascript
{
  $lookup: {
    from: 'users',
    localField: '_id',
    foreignField: '_id',
    as: 'user'
  }
}

```

* This **joins the `users` collection** with the current document (which most likely comes from an aggregation on `orders`).
* It matches on `_id` — typically assuming each document in this pipeline has an `_id` that corresponds to a user's `_id`.
* The result is a new field `"user"` which will be an **array** of matching users (usually just one).

`{ $unwind: '$user' }`

* Since `$lookup` gives an array (even if it's one user), `$unwind` turns the array into a flat object.
* After this, `user` is no longer an array — it's a normal subdocument like:

```javascript
user: {
  _id: ...,
  username: 'JohnDoe',
  isAdmin: false
}

```

* In the match part it **filters out any users who are admins** , i.e., where `isAdmin: true`.
* Only regular customers are kept at this point.

Now in the match part,

* This  **shapes the output** :

  * Removes `_id` from the output (`_id: 0`).
  * Adds a new field `name` by projecting `user.username`.
  * Keeps `orders` and `spent` fields from previous stages in the pipeline.
* Final output per user looks like:

```javascript
{
  name: "JohnDoe",
  orders: 5,
  spent: 7200
}

```

---

# ----`.lean()` in Mongoose

##### 🔧 What `.lean()` Does:

It **tells Mongoose to skip creating full Mongoose documents** and instead return  **plain JavaScript objects** .

Normally, Mongoose returns  **Mongoose Documents** , which have methods and getters attached (like `.save()`, `.populate()`, virtuals, etc.).

With `.lean()`, Mongoose skips all that and gives you  **POJOs (plain old JavaScript objects)** .

##### ✅ Why Use `.lean()`?

* **Performance Boost** : It's  **faster and lighter** , especially for read-only operations like `.find()`, because it avoids the overhead of creating Mongoose document instances.
* **Less Memory Usage** : Because it skips all the extras.
* **Safe for Display/Send** : If you're just sending this data to a client or using it in logic (without modifying and saving it), `lean()` is ideal.

Example---
`const allMainCategories = await Category.find({ parentCategory: null }, { name: 1 }).lean();`

Becuase of tihs, with lean remember this--

```javascript
const category = await Category.findOne({ _id: '123' }).lean();
console.log(typeof category); // object (plain JS)
category.save(); // ❌ error: save is not a function

```

---

# `----Aggregation- `$switch, branches- case- then- default- $unwind- $push- k v -$addFields- $group- $project-  `with Example --Project[Fitlab]

getStockStatsPerMainCategory()  is a controller function that will return the number of  **inStock** ,  **lowStock** , and **outOfStock** products for each **main category** (i.e., categories whose `parentCategory` is `null`)

```javascript
const Category = require('../models/categoryModel');
const Product = require('../models/productModel');

const getStockStatsPerMainCategory = async (req, res) => {
  try {
    // Step 1: Get all main categories
    const mainCategories = await Category.find({ parentCategory: null }).select("name");

    const mainCategoryNames = mainCategories.map(cat => cat.name);

    // Step 2: Aggregate stock data from products
    const stockStats = await Product.aggregate([
      {
        $match: {
          category: { $in: mainCategoryNames }
        }
      },
      {
        $project: {
          category: 1,
          stockStatus: {
            $switch: {
              branches: [
                { case: { $eq: ["$stock", 0] }, then: "outOfStock" },
                { case: { $lt: ["$stock", 10] }, then: "lowStock" }
              ],
              default: "inStock"
            }
          }
        }
      },
      { $unwind: "$category" },
      {
        $match: {
          category: { $in: mainCategoryNames }
        }
      },
      {
        $group: {
          _id: { category: "$category", stockStatus: "$stockStatus" },
          count: { $sum: 1 }
        }
      },
      {
        $group: {
          _id: "$_id.category",
          stats: {
            $push: {
              k: "$_id.stockStatus",
              v: "$count"
            }
          }
        }
      },
      {
        $project: {
          _id: 0,
          name: "$_id",
          stats: {
            $arrayToObject: "$stats"
          }
        }
      },
      {
        $addFields: {
          inStock: { $ifNull: ["$stats.inStock", 0] },
          lowStock: { $ifNull: ["$stats.lowStock", 0] },
          outOfStock: { $ifNull: ["$stats.outOfStock", 0] },
        }
      },
      {
        $project: {
          name: 1,
          inStock: 1,
          lowStock: 1,
          outOfStock: 1
        }
      }
    ]);

    return res.status(200).json(stockStats);
  } catch (error) {
    console.error("Aggregation error:", error);
    return res.status(500).json({ message: "Server Error" });
  }
};

module.exports = { getStockStatsPerMainCategory };

```

`$switch` operator in  **MongoDB's Aggregation Framework** , which works similarly to a `switch-case` statement in traditional programming. It's used to **conditionally assign values** based on multiple conditions.

SYNTAX---

```javascript
{
  $switch: {
    branches: [
      { case: <expression>, then: <value> },
      { case: <expression>, then: <value> },
      ...
    ],
    default: <default-value>
  }
}

```

#### 🔍 What This Does:

This code is checking the value of the `stock` field for each document in the pipeline and assigning a **new string value** based on its value:

`branches`:

A list of **conditions** to evaluate in order:

1. **First case** :

   `{ $eq: ["$stock", 0] }`

   → If `stock` is exactly 0, then `"outOfStock"` is returned.
2. **Second case** :

   `{ $lt: ["$stock", 10] }`

   → If `stock` is less than 10 (but not 0, because first match wins), then `"lowStock"` is returned.

#### `default`:

If **none** of the `case` conditions match, then `"inStock"` is returned.

| Term         | Meaning                                          |
| ------------ | ------------------------------------------------ |
| `$switch`  | Main conditional operator (like `switch`in JS) |
| `branches` | Array of conditions to check                     |
| `case`     | A condition to evaluate                          |
| `then`     | Value to return if the `case`is true           |
| `default`  | Value to return if no cases match                |

**So, Example Input document:**

`{ category: "Accessories", stock: 5 }`

Output:

`{ category: "Accessories", stockStatus: "lowStock" }`

Now,

```javascript
{ $unwind: "$category" },
{ $match: { category: { $in: mainCategoryNames } } },

```

1. `$unwind: "$category"`

* This operator **deconstructs an array field** from the input documents and outputs a separate document for **each element** of the array.
* In this case, if a document has:

```javascript
{
  name: "Protein Bar",
  category: ["Snacks", "Supplements"]
}

```

After unwinding:

```javascript
{ name: "Protein Bar", category: "Snacks" }
{ name: "Protein Bar", category: "Supplements" }

```

* Why? So you can **treat each category separately** in further stages.

2. `$match: { category: { $in: mainCategoryNames } }`

* This stage **filters** the unwound documents.
* Only keeps documents where `category` is **included** in the `mainCategoryNames` array.
* Example:

`mainCategoryNames = ["Supplements", "Cardio"]`

Only the second document (`category: "Supplements"`) would pass the match filter.

Now the pipeline--

```javascript
{ 
  $group: { 
    _id: { category: "$category", stockStatus: "$stockStatus" }, 
    count: { $sum: 1 } 
  } 
},
{ 
  $group: { 
    _id: "$_id.category", 
    stats: { 
      $push: { k: "$_id.stockStatus", v: "$count" } 
    } 
  } 
},
{ 
  $project: { 
    _id: 0, 
    name: "$_id", 
    stats: { $arrayToObject: "$stats" } 
  } 
}

```

##### 🧠 Goal

This pipeline answers the question:

> **For each category, how many products are in each stock status?**
>
> (For example:  *InStock* ,  *OutOfStock* , etc.)

It transforms data into this shape:

`{   "name": "Supplements",   "stats": {     "InStock": 42,     "OutOfStock": 17,     "LowStock": 6   } }`

✅ Stage 1: `$group` by `category` and `stockStatus`

```javascript
{ category: "Supplements", stockStatus: "InStock" }
{ category: "Supplements", stockStatus: "OutOfStock" }
{ category: "Accessories", stockStatus: "InStock" }

```

into counts:

```
{ _id: { category: "Supplements", stockStatus: "InStock" }, count: 42 }
{ _id: { category: "Supplements", stockStatus: "OutOfStock" }, count: 17 }
{ _id: { category: "Accessories", stockStatus: "InStock" }, count: 22 }

```

✅ Stage 2: `$group` again by `category` only

This groups the stockStatus-count pairs  **into an array per category** :

```javascript
{
  _id: "Supplements",
  stats: [
    { k: "InStock", v: 42 },
    { k: "OutOfStock", v: 17 }
  ]
}

```

Then,

```javascript
{
  $project: {
    _id: 0,
    name: "$_id",
    stats: { $arrayToObject: "$stats" }
  }
}

```

This:

* Renames `_id` to `name`
* Converts the `stats` array into an object using `$arrayToObject`

Final output:

```javascript
{
  name: "Supplements",
  stats: {
    "InStock": 42,
    "OutOfStock": 17
  }
}

```

So after the other pipelines, the final output wlll be

```javascript
 [
  {
    name: "Cardio Equipment",
    inStock: 45,
    lowStock: 5,
    outOfStock: 2,
  },
  {
    name: "Strength Training",
    inStock: 38,
    lowStock: 8,
    outOfStock: 4,
  },
  {
    name: "Supplements",
    inStock: 65,
    lowStock: 12,
    outOfStock: 3,
  }
]
```

---

# ----Aggregation- `countDocuments, $lookup- $unwind -$group -$project -$match `with Example --Project[Fitlab]

getCouponStats() is a controller to calculate the active coupons and coupon redemptions

```javascript
const getCouponStats = async(req, res, next)=> {
  console.log("Inside getCouponStats")
  const now = new Date()

  const activeCouponCount = await Coupon.countDocuments({
    status: "active",
    startDate: { $lte: now },
    endDate: { $gte: now },
  })
  console.log("activeCouponCount--->", activeCouponCount)

  const redemptions = await Order.aggregate([
    {
      $match: {
        couponUsed: { $ne: null }
      }
    },
    {
      $group: {
        _id: "$couponUsed",
        count: { $sum: 1 }
      }
    },
    {
      $lookup: {
        from: "coupons",
        localField: "_id",
        foreignField: "_id",
        as: "coupon"
      }
    },
    {
      $unwind: "$coupon"
    },
    {
      $project: {
        couponId: "$coupon._id",
        code: "$coupon.code",
        count: 1
      }
    }
  ])
   console.log("activeCouponCount--->", activeCouponCount)
   console.log("redemptions--->", redemptions)

   const totalRedemptions = redemptions.reduce((counter, coupon)=> counter += coupon.count, 0)
   console.log("totalRedemptions---->", totalRedemptions)

  res.status(200).json({activeCouponCount, couponRedemptions: totalRedemptions});
}
```

# ----Aggregation- `$sum, $add- $ifNull -$sort -$nin -$match `with Example --Project[Fitlab]

getDiscountImpactDatas() is a controller that calculates the **monthly revenue** (grouped by month) **with and without coupon discounts** using the `Order` model

```javascript
const getDiscountImpactDatas = async (req, res) => {
  try {
    console.log("Inside getDiscountImpactDatas")
    const revenue = await Order.aggregate([
      {
        $match: {
          orderStatus: { $nin: ["cancelled", "returning", "refunded"] },
        },
      },
      {
        $group: {
          _id: {
            year: { $year: "$orderDate" },
            month: { $month: "$orderDate" },
          },
          withDiscount: { $sum: "$absoluteTotalWithTaxes" },
          withoutDiscount: {
            $sum: {
              $add: ["$absoluteTotalWithTaxes", { $ifNull: ["$couponDiscount", 0] }],
            },
          },
        },
      },
      {
        $sort: {
          "_id.year": 1,
          "_id.month": 1,
        },
      },
    ])

    // Convert numeric month to abbreviated name and format result
    const monthNames = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", 
                        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]

    const discountImpactDatas = revenue.map((item)=> ({
      name: monthNames[item._id.month - 1],
      withDiscount: item.withDiscount,
      withoutDiscount: item.withoutDiscount,
    }))
    console.log("discountImpactDatas---->", discountImpactDatas)

    res.status(200).json(discountImpactDatas);
  }
  catch(error){
    console.error("Error in getDiscountImpactDatas:", error.message)
    next(error)
  }
}
```

# ----Aggregation- `$sum, $add- $ifNull -$sort -$nin -$match `with Example --Project[Fitlab]

---

# ----Journaling

👉 **No, it’s NOT for logs.**

👉 It’s for **journaling (durability), not logging.**

### 🔹 What `j` actually means

```js
writeConcern: { j: true }
```

👉 `j` = **journal acknowledgment**

It tells MongoDB:

> “Don’t confirm this write until it is safely written to the journal on disk.”

### 🔹 What is the  *journal* ?

MongoDB maintains a **write-ahead log** (called the  *journal* ) 🧾

* Before writing to actual data files
* MongoDB first writes to the **journal file on disk**

👉 Why?
To recover data if the server crashes 💥

### 🔹 So what happens with `j: true`?

Without `j`

* Write goes to memory
* MongoDB may acknowledge quickly ⚡
* Risk: crash = data loss ❌

With `j: true`

* Write is **flushed to journal on disk**
* Then MongoDB acknowledges ✅
* Much safer, slightly slower 🐢

### 🔹 Example

```js
collection.insertOne(doc, {
  writeConcern: {
    w: 1,
    j: true
  }
})
```

👉 Meaning:

* Wait for primary node (`w:1`)
* AND ensure it's written to disk journal

### 🔹 Is it related to logs?

🚫 No — **not application logs**
🚫 Not debugging logs

✅ It’s a **low-level durability mechanism**

Think of it like:

* `comment` → for logs/debugging 🧠
* `j` → for crash safety 💾

### 🔥 Interview-level insight

* `j: true` is often used with:

  ```js
  { w: "majority", j: true }
  ```

  👉 ensures:

  * replicated to majority
  * AND persisted to disk
* If journaling is disabled in MongoDB config, `j` is ignored

### 🧠 Simple analogy

* `j: false` → “I wrote it in RAM, trust me” 😅
* `j: true` → “It’s written on disk, even if I crash, it survives” 💪

---

# ----Options of `.insertOne() & .insertMany()`

In MongoDB, both `.insertOne()` and `.insertMany()` accept an **options object** as their second parameter. These options control behavior like validation, ordering, and write acknowledgment.

Let’s break them down clearly 👇

### 🔹 1. `.insertOne(document, options)`

✅ Common Options

##### **1. `writeConcern`**

Controls how MongoDB acknowledges the write.

```js
collection.insertOne(doc, {
  writeConcern: { w: "majority", j: true, wtimeout: 5000 }
})
```

* `w` → number of nodes that must acknowledge (e.g., `1`, `"majority"`)
* `j` → wait for write to be committed to journal
* `wtimeout` → timeout in ms

👉 Used in production when durability matters.

##### **2. `bypassDocumentValidation`**

Skips schema validation rules.

```js
collection.insertOne(doc, {
  bypassDocumentValidation: true
})
```

👉 Useful when:

* You have strict validators
* You want to force insert anyway

##### **3. `session`**

Attach the operation to a transaction/session.

```js
collection.insertOne(doc, { session })
```

👉 Required when using transactions.

##### **4. `comment`**

Adds a comment for debugging/logging.

```js
collection.insertOne(doc, {
  comment: "Insert user during signup"
})
```

👉 Visible in logs / profiler.

### 🔹 2. `.insertMany(documents, options)`

All options from `insertOne()` apply  **+ some extra important ones** :

##### 🔥 1. **`ordered` (VERY IMPORTANT)**

Controls execution order & failure behavior.

```js
collection.insertMany(docs, { ordered: true })
```

👉 Behavior:

* `true` (default):
  * Stops at first error ❌
  * Inserts sequentially
* `false`:
  * Continues even if some fail ✅
  * Inserts in parallel (faster ⚡)

##### 🔥 2. **`writeConcern`**

Same as above.

##### 🔥 3. **`bypassDocumentValidation`**

Same as above.

##### 🔥 4. **`session`**

Same as above.

##### 🔥 5. **`comment`**

Same as above.

### 🔥 Example (Real-world)

```js
await collection.insertMany(
  [
    { name: "A" },
    { name: "B" },
    { name: "A" } // duplicate maybe
  ],
  {
    ordered: false,
    writeConcern: { w: "majority" },
    bypassDocumentValidation: false
  }
)
```

👉 Even if one fails (duplicate), others will still insert.

### ⚠️ Key Differences Summary

| Feature                | insertOne | insertMany |
| ---------------------- | --------- | ---------- |
| Inserts multiple docs  | ❌        | ✅         |
| `ordered`option      | ❌        | ✅         |
| Performance tuning     | ❌        | ✅         |
| Error handling control | Basic     | Advanced   |

### 🧠 Important Interview Points

* `ordered: false` → improves performance in bulk inserts
* `writeConcern` → critical for **data durability**
* `bypassDocumentValidation` → bypass schema rules
* `session` → required for **transactions**
* MongoDB automatically adds `_id` if not provided

### 🔹 Do these options only matter with replica sets?

👉 **No — they work even on a single MongoDB instance.**

**Without replica set (standalone server):**

* `writeConcern: { w: 1 }` → default, just confirms write in memory
* `j: true` → ensures it’s written to disk journal 💾
* `bypassDocumentValidation`, `comment` → still work normally

👉 Only limitation:

* You **can’t use `w: "majority"`** (because there are no replicas)

**With replica set:**

Now `writeConcern` becomes more powerful:

```js
writeConcern: {
  w: "majority",
  j: true
}
```

👉 Means:

* Majority of nodes have the data
* AND it's written to disk journal

#### 🔥 So why journaling if we already have replica sets?

This is the **core concept** 👇

**👉 Replica set ≠ guaranteed no data loss**

**👉 Journaling ≠ replication**

They solve  **different problems** .

#### 🔹 1. What replica sets protect against

👉 **Node failure (machine crash, server down)**

Example:

* Primary crashes 💥
* Secondary becomes new primary
* Data is preserved (if replicated)

#### 🔹 2. What journaling protects against

👉 **Sudden crash BEFORE data is safely written to disk**

Example:

1. Write happens
2. MongoDB stores it in memory (not yet journaled)
3. Server crashes 💥

👉 Result:

* Data is LOST ❌
* Even replication may not have happened yet

#### ⚠️ Important Insight

Even in replica sets:

👉 Writes are first in  **memory** , then:

* replicated
* journaled

So crashes can still cause loss if not journaled.

#### 🔥 Key difference (very important)

| Feature     | Protects Against              |
| ----------- | ----------------------------- |
| Replica Set | Node/server failure           |
| Journaling  | Crash before disk persistence |

#### 🔹 Why use BOTH?

👉 Best practice:

```js
writeConcern: {
  w: "majority",
  j: true
}
```

This ensures:

✅ Replicated to majority
✅ Written to disk journal

👉 Maximum safety 🔒

---

# ----Updating document- `.updateOne() vs .findOneAndUpdate()`

This is a **very important MongoDB distinction** — both update a single document, but their **purpose and behavior differ a lot** 👇

### 🔥 Core Difference (one-line)

* `updateOne()` → **just updates** (no document returned)
* `findOneAndUpdate()` → **finds + updates + returns the document**

### 🔹 1. `updateOne()`

✅ What it does

* Updates the **first matching document**
* Returns a  **write result** , NOT the document

```js
await collection.updateOne(
  { email: "a@gmail.com" },
  { $set: { name: "Arjun" } }
)
```

📦 Return value

```js
{
  acknowledged: true,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedId: null
}
```

👉 You only know:

* Was it found?
* Was it modified?

👉 You **don’t get the updated document**

**🧠 Use when:**

* You **don’t need the updated data**
* Just want to update efficiently ⚡

### 🔹 2. `findOneAndUpdate()`

✅ What it does

* Finds a document
* Updates it
* Returns the document (before or after update)

```js
await collection.findOneAndUpdate(
  { email: "a@gmail.com" },
  { $set: { name: "Arjun" } },
  { returnDocument: "after" }
)
```

📦 Return value

```js
{
  value: { _id: ..., email: "a@gmail.com", name: "Arjun" },
  ok: 1
}
```

##### 🔥 Important Option

**`returnDocument`**

```js
{ returnDocument: "before" } // default
{ returnDocument: "after" }
```

👉 Controls what you get back:

* `"before"` → old document
* `"after"` → updated document

🧠 Use when:

* You **need the updated document immediately**
* Common in APIs (send updated data back)

### 🔥 Key Differences Table

| Feature          | updateOne()        | findOneAndUpdate() |
| ---------------- | ------------------ | ------------------ |
| Returns document | ❌                 | ✅                 |
| Returns metadata | ✅                 | ✅ (limited)       |
| Atomic operation | ✅                 | ✅                 |
| Performance      | Faster ⚡          | Slightly slower    |
| Common use       | Background updates | API responses      |

### ⚠️ Important Concept: Atomicity

👉 Both are **atomic at document level**

But:

### 🔸 `findOneAndUpdate()`

* Combines **find + update in one atomic step**
* Prevents race conditions 🛡️

### 🔥 Real-world Example

### ❌ Using `updateOne()` (not ideal)

```js
await collection.updateOne({ _id }, { $inc: { views: 1 } })
const doc = await collection.findOne({ _id })
```

👉 Problem:

* Another update could happen in between 😬

✅ Using `findOneAndUpdate()`

```js
const doc = await collection.findOneAndUpdate(
  { _id },
  { $inc: { views: 1 } },
  { returnDocument: "after" }
)
```

👉 Safe + clean ✅

### 🔥 Bonus Options (important)

**For both:**

* `upsert: true` → create if not exists

**Only useful in `findOneAndUpdate()`:**

`projection`

```js
{ projection: { name: 1 } }
```

`sort`

```js
{ sort: { createdAt: -1 } }
```

👉 Useful when multiple docs match

---

# ----Updating document- `findOneAndUpdate() vs findByIdAndUpdate()`

Now we’re stepping into **MongoDB vs Mongoose territory** ⚡

This is a very common interview trap, so let’s make it crystal clear.

### 🔥 Core Difference (one-line)

* `findOneAndUpdate()` → **MongoDB method** (also available in Mongoose)
* `findByIdAndUpdate()` → **Mongoose convenience method only**

👉 Under the hood, `findByIdAndUpdate()` just uses `findOneAndUpdate()`.

### 🔹 1. `findOneAndUpdate()`

✅ What it does

* Finds a document using **any filter**
* Updates it
* Returns the document

```js
await User.findOneAndUpdate(
  { email: "a@gmail.com" },
  { $set: { name: "Arjun" } },
  { new: true } //CAN USE 'new' since using with Mongoose instead of native MongoDB driver (in which case it should be 'returnDocument')
)
```

**🧠 Use when:**

* You need **flexible queries**
* Example:
  * email
  * username
  * multiple conditions

### 🔹 2. `findByIdAndUpdate()`

✅ What it does

* Finds document **only by `_id`**
* Updates it
* Returns the document

```js
await User.findByIdAndUpdate(
  "661abc123...",
  { $set: { name: "Arjun" } },
  { new: true }
)
```

**🧠 Use when:**

* You already have `_id`
* Cleaner and shorter ✨

### 🔥 Under the hood (VERY IMPORTANT)

```js
findByIdAndUpdate(id, update, options)
```

👉 is equivalent to:

```js
findOneAndUpdate({ _id: id }, update, options)
```

### 🔥 Key Differences Table

| Feature           | findOneAndUpdate() | findByIdAndUpdate()   |
| ----------------- | ------------------ | --------------------- |
| Query flexibility | ✅ Any filter      | ❌ Only `_id`       |
| Syntax            | Slightly longer    | Shorter ✨            |
| MongoDB native    | ✅ Yes             | ❌ No (Mongoose only) |
| Under the hood    | —                 | Uses findOneAndUpdate |

### 🔥 Options (Mongoose-specific)

**⚠️ Important: `new: true`**

```js
{ new: true }
```

👉 Returns **updated document**

* `false` (default) → old doc
* `true` → updated doc ✅

**⚠️ Also common:**

```js
{
  runValidators: true,
  upsert: true
}
```

* `runValidators` → enforce schema validation
* `upsert` → create if not found

### 🔥 Real-world usage

✅ When you have ID (most APIs)

```js
app.put("/user/:id", async (req, res) => {
  const user = await User.findByIdAndUpdate(
    req.params.id,
    req.body,
    { new: true }
  )
  res.json(user)
})
```

✅ When using other fields

```js
await User.findOneAndUpdate(
  { email: req.body.email },
  { $set: req.body },
  { new: true }
)
```

---

# ----Schema validators(Mongoose) and `runValidators`

👉 **No — validators do NOT run by default in `findOneAndUpdate()` / `findByIdAndUpdate()`** ❌

👉 You must explicitly set `runValidators: true` ✅

### 🔥 Why this happens

In  **Mongoose** , there are two different flows:

##### 🔹 1. `.save()` (Document method)

```js
const user = await User.findById(id)
user.name = "Arjun"
await user.save()
```

✅ Validators run automatically
✅ Middleware (`pre('save')`) runs

##### 🔹 2. `findOneAndUpdate()` / `findByIdAndUpdate()`

```js
await User.findByIdAndUpdate(id, {
  name: "Arjun"
})
```

❌ Validators DO NOT run by default
❌ `save()` middleware does NOT run

### 🔹 So how to enable validation?

```js
await User.findByIdAndUpdate(id, 
  { name: "Arjun" },
  { runValidators: true }
)
```

👉 Now schema validation WILL run ✅

### 🔥 Important nuance (INTERVIEW GOLD)

Even with `runValidators: true`:

👉 Validation is **limited** compared to `.save()`

**⚠️ Example**

Schema:

```js
name: {
  type: String,
  required: true,
  minlength: 5
}
```

**❌ Without `runValidators`**

```js
await User.findByIdAndUpdate(id, { name: "A" })
```

👉 Invalid data gets saved 😬

**✅ With `runValidators: true`**

```js
await User.findByIdAndUpdate(id, { name: "A" }, { runValidators: true })
```

👉 Throws validation error ✅

### ⚠️ Another subtle limitation

Validators only run on  **updated fields** , not entire document.

Example:

```js
await User.findByIdAndUpdate(id, {
  age: 10
}, { runValidators: true })
```

👉 If `name` is required but not included →
❗ No error (because it's not being updated)

### 🔥 Best Practice

👉 Use this combo:

```js
{
  new: true,
  runValidators: true
}
```

### 🔥 When to prefer `.save()` instead

Use `.save()` when you need:

* Full validation 🛡️
* Middleware (`pre/post save`) ⚙️
* Complex logic before saving

---

# ----Syntax systems- Object based and Array based

### 🔥Example 1

```javascript
db.users.aggregate([
  {
    $match: {
      $expr: { $gt: ["$age", 18] }
    }
  }
])

//OR

{
  $lookup: {
    from: "users",
    let: { userId: "$userId" },
    pipeline: [
      {
        $match: {
          $expr: { $eq: ["$_id", "$$userId"] }
        }
      }
    ],
    as: "user"
  }
}
```

**As you can see you CANNOT write for example:**

```js
{ $gte: { "$age", 18 } }
```

👉 ✅ You MUST write:

```js
{ $gte: ["$age", 18] }
```

**🧠 Why is that?**

Because:

👉 **Aggregation operators use ARRAY syntax, not object syntax**

### 🔹 Aggregation Expression Syntax (your `$cond`, `$expr`, etc.)

Operators like:

* `$gte`
* `$eq`
* `$add`
* `$multiply`

👉 Always follow this format:

```js
{ $operator: [arg1, arg2, arg3...] }
```

**✅ Correct:**

```js
{ $gte: ["$age", 18] }
```

👉 Meaning:

```
age >= 18
```

**❌ Incorrect:**

```js
{ $gte: { "$age", 18 } }
```

👉 This is invalid JavaScript AND invalid MongoDB syntax

### 🔥 Why arrays instead of objects?

Because MongoDB expressions are like  **function calls** :

```js
$gte(a, b)
```

👉 Represented as:

```js
{ $gte: [a, b] }
```

### 🔹 But in `find()`

✅ Correct query syntax:

```js
db.users.find({ age: { $gte: 18 } })
```

Because the context is queryying

### 🔥 Key Difference (VERY IMPORTANT)

| Context                           | Syntax Style              |
| --------------------------------- | ------------------------- |
| `find()`query                   | Object-based              |
| Aggregation (`$expr`,`$cond`) | Array-based (expressions) |

### 🔹 Compare side-by-side

**✅ Query (find)**

```js
{ age: { $gte: 18 } }
```

👉 field → condition

**✅ Aggregation**

```js
{ $gte: ["$age", 18] }
```

👉 expression → arguments

### 🔥 The Truth: MongoDB has **TWO DIFFERENT SYNTAX SYSTEMS**

🔹 1. Query Syntax (Object-based) 📦

🔹 2. Expression Syntax (Array-based) ⚙️

##### 🔹 1. Query Syntax

```js
{ age: { $gt: 18} }
```

👉 This is **query syntax**
👉 Used in:

* `find()`
* `$match` (without `$expr`)

**🧠 Meaning:**

```js
age > 18
```

👉 This is:

* **SUPPORTS- field vs constant**
* Simple filtering

##### 🔹 2. Expression Syntax

```js
{ $gt: ["$age", 18] }
```

👉 This is **aggregation expression syntax**

Used in:

* `$project`
* `$addFields`
* `$expr`
* `$cond`

**🧠 Meaning:**

```js
price > 1000
```

👉 But more powerful as it SUPPORTS:

* field vs field ✅
* field vs variable ✅
* computed values ✅

### 🔥 Where you MUST use array syntax

* `$cond`
* `$expr`
* `$project`
* `$addFields`
* `$lookup.pipeline`

### 👉 Example 2

```javascript
db.products.aggregate([
  {
    $facet: {
      expensive: [
        { $match: { price: { $gt: 1000 } } }
      ],
      cheap: [
        { $match: { price: { $lte: 1000 } } }
      ]
    }
  }
])
```

**You saw:**

```js
{ $match: { price: { $gt: 1000 } } }
```

And you're thinking:

> “Wait… didn’t you say expressions use array syntax like `$gt: ["$price", 1000]`?” 🤔

##### 🔥 Why `$match` uses object syntax here?

Because:

👉 `$match` behaves like a **find query**

So this:

```js
{ $match: { price: { $gt: 1000 } } }
```

👉 is basically:

```js
db.products.find({ price: { $gt: 1000 } })
```

##### 🔥 When DOES `$match` use array syntax?

👉 When you use `$expr`

Example:

```js
{
  $match: {
    $expr: { $gt: ["$price", "$discountPrice"] }
  }
}
```

👉 Now it switches to:

* expression mode ⚙️

##### 🔥 Now let's see inside $match with an without expr operator

👉 Inside `$match`:

✅ Without `$expr`

```js
{ field: { $gt: value } }   // object syntax
```

✅ With `$expr`

```js
{ $gt: ["$field", value] }  // array syntax
```

##### 🔥 Side-by-side comparison

| Case                          | Syntax | Type              |
| ----------------------------- | ------ | ----------------- |
| `{ price: { $gt: 1000 } }`  | Object | Query syntax      |
| `{ $gt: ["$price", 1000] }` | Array  | Expression syntax |

### 🔥 Why MongoDB has 2 systems

**🔹 Query syntax**

* Simple
* Fast
* Limited (field vs value)

**🔹 Expression syntax**

* Flexible
* Powerful
* Needed for:
  * `$cond`
  * `$lookup`
  * `$project`

### 👉 Example 3

```js
{
  $group: {
    _id: "$category",
    total: { $sum: "$price" }
  }
}
```

👉 This whole thing is:

* **object structure (configuration)**
* NOT expression itself

**⚠️ Important Distinction**

👉 Inside this:

```js
total: { $sum: "$price" }
```

* `{ $sum: "$price" }` → expression/accumulator ⚙️
* But the outer structure → object config 📦

### 🔥 KEY INSIGHT (this is the real answer)

👉 **It’s NOT about “stage vs operator”**
👉 It’s about **CONTEXT**

##### 🔹 Context 1: Query context → object syntax

Used in:

* `$match` (without `$expr`)

```js
{ age: { $gte: 18 } }
```

##### 🔹 Context 2: Expression context → array syntax

Used in:

* `$project`
* `$addFields`
* `$expr`
* `$cond`

```js
{ $gte: ["$age", 18] }
```

##### 🔥 Special Case: `$expr`

👉 `$expr` acts like a **bridge**

```js
{
  $match: {
    $expr: { $gte: ["$age", 18] }
  }
}
```

👉 Inside `$expr`:

* switches to expression syntax ⚙️

### 🔥 So important statement :

**Query-style object syntax appears mainly in `$match` (without `$expr`), while expression operators use array syntax. But many stages themselves are defined using object configuration.**

### 🔥 Clean mental model

**🧱 Stage level → always object config**

```js
{ $group: { ... } }
{ $project: { ... } }
```

**⚙️ Expression level → array syntax**

```js
{ $add: ["$a", "$b"] }
```

**📦 Query level → object syntax**

```js
{ age: { $gte: 18 } }
```

##### 🔥 Visual breakdown

```js
{
  $group: {                 // 🧱 stage config (object)
    _id: "$category",   
    total: {                // 📦 field config
      $sum: "$price"        // ⚙️ accumulator/expression
    }
  }
}
```

### 🔚 One-line takeaway

👉 **Only query-style conditions (mainly `$match`) use object syntax; expression operators always use array syntax, while stages themselves are just object configurations**

---

# Aggregation- `$group` in detail

`$group` is **one of the most important (and most asked) aggregation stages** 🔥

If you really understand this, a lot of MongoDB becomes easy.

### 🔥 What is `$group`?

👉 **Aggregation stage** 🧱
👉 Used to **group documents by a key and perform aggregations**

**🧠 Simple intuition**

> “Like SQL `GROUP BY` — combine documents and compute values”

**🔹 Basic Syntax**

```js
{
  $group: {
    _id: <grouping key>,
    field1: { <accumulator>: <expression> },
    field2: { <accumulator>: <expression> }
  }
}
```

### 🔥 Key parts

| Part         | Meaning                        |
| ------------ | ------------------------------ |
| `_id`      | Grouping key                   |
| fields       | Computed values                |
| accumulators | Operations like sum, avg, etc. |

### 🔥 Example 1: Basic grouping

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$category",
      totalOrders: { $sum: 1 }
    }
  }
])
```

🧠 Input:

```js
{ category: "electronics" }
{ category: "electronics" }
{ category: "clothing" }
```

Output:

```js
[
  { _id: "electronics", totalOrders: 2 },
  { _id: "clothing", totalOrders: 1 }
]
```

👉 Groups by category
👉 Counts documents

### 🔥 Example 2: Sum values

```js
{
  $group: {
    _id: "$category",
    totalRevenue: { $sum: "$price" }
  }
}
```

👉 Adds prices per category 💰

### 🔥 Example 3: Average

```js
{
  $group: {
    _id: "$category",
    avgPrice: { $avg: "$price" }
  }
}
```

### 🔥 Example 4: Min & Max

```js
{
  $group: {
    _id: "$category",
    minPrice: { $min: "$price" },
    maxPrice: { $max: "$price" }
  }
}
```

### 🔥 Example 5: Collect values (`$push`)

```js
{
  $group: {
    _id: "$category",
    prices: { $push: "$price" }
  }
}
```

Output:

```js
{
  _id: "electronics",
  prices: [1000, 2000, 3000]
}
```

### 🔥 Example 6: Unique values (`$addToSet`)

```js
{
  $group: {
    _id: "$category",
    uniquePrices: { $addToSet: "$price" }
  }
}
```

👉 Removes duplicates

### 🔥 Example 7: Full documents (`$$ROOT`) 🔥

```js
{
  $group: {
    _id: "$category",
    docs: { $push: "$$ROOT" }
  }
}
```

👉 Groups entire documents inside array

### 🔥 Example 8: First & Last (IMPORTANT)

```js
db.orders.aggregate([
  { $sort: { date: 1 } },
  {
    $group: {
      _id: "$userId",
      firstOrder: { $first: "$$ROOT" },
      lastOrder: { $last: "$$ROOT" }
    }
  }
])
```

👉 Gets:

* earliest order
* latest order

### 🔥 Example 9: Multiple grouping fields

```js
{
  $group: {
    _id: {
      category: "$category",
      status: "$status"
    },
    count: { $sum: 1 }
  }
}
```

Output:

```js
{
  _id: { category: "electronics", status: "delivered" },
  count: 5
}
```

### 🔥 Example 10: Global aggregation (no grouping)

```js
{
  $group: {
    _id: null,
    totalRevenue: { $sum: "$price" }
  }
}
```

👉 Returns single document

### 🔥 Common Accumulators

| Operator      | Purpose                        |
| ------------- | ------------------------------ |
| `$sum`      | Total                          |
| `$avg`      | Average                        |
| `$min`      | Minimum                        |
| `$max`      | Maximum                        |
| `$push`     | Collect values                 |
| `$addToSet` | Unique values                  |
| `$first`    | First value                    |
| `$last`     | Last value                     |
| `$count`    | Count (shortcut in some cases) |
| s             |                                |

### 🔥 Important Behavior ⚠️

**❗ 1. `_id` is mandatory**

👉 Without `_id` → error

**❗ 2. Output fields must use accumulators**

❌ Wrong:

```js
name: "$name"
```

✅ Correct:

```js
name: { $first: "$name" }
```

**❗ 3. Order matters for `$first` / `$last`**

👉 Always use `$sort` before `$group`

**❗ 4. `$group` changes structure completely**

👉 Original documents are gone

### 🔥 Real-world Example 🚀

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  {
    $group: {
      _id: "$userId",
      totalSpent: { $sum: "$amount" },
      avgSpent: { $avg: "$amount" },
      orders: { $sum: 1 }
    }
  }
])
```

👉 Output:

```js
{
  _id: 101,
  totalSpent: 5000,
  avgSpent: 1000,
  orders: 5
}
```

---

# ----Aggregation- `$count vs {sum: 1}`

### 🧠 The Truth (No ambiguity)

👉 **`$count` exists in TWO different forms in MongoDB**

##### 🔥 1. `$count` (Aggregation Stage) 🧱

```js
{ $count: "total" }
```

👉 This is a **pipeline stage**
👉 Counts total documents passing through

✅ Example

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $count: "totalOrders" }
])
```

##### 🔥 2. `$count` (Accumulator) ⚙️

👉 Yes — there **is also an accumulator version** (newer addition)

```js
{
  $group: {
    _id: "$category",
    count: { $count: {} }
  }
}
```

**✅ This is equivalent to:**

```js
count: { $sum: 1 }
```

### 🧠 Why this causes confusion

Because:

| Name                                           | Type | Syntax |
| ---------------------------------------------- | ---- | ------ |
| `$count` | Stage       | `{ $count: "x" }` |      |        |
| `$count` | Accumulator | `{ $count: {} }`  |      |        |

👉 Same name, different roles 😬

### 🔥 Why people still use `{ $sum: 1 }`

Even though accumulator `$count` exists:

**✅ 1. Older MongoDB compatibility**

👉 `$sum: 1` works everywhere
👉 `$count` accumulator is newer

**✅ 2. More flexible**

You can do conditional counts:

```js
count: {
  $sum: {
    $cond: [{ $gte: ["$price", 1000] }, 1, 0]
  }
}
```

👉 Not possible directly with `$count`

**✅ 3. Habit / convention**

👉 `{ $sum: 1 }` is the “classic” way

### 🔥 Side-by-side

**Using `$sum`**

```js
{
  $group: {
    _id: "$category",
    count: { $sum: 1 }
  }
}
```

**Using `$count` accumulator**

```js
{
  $group: {
    _id: "$category",
    count: { $count: {} }
  }
}
```

**Using `$count` stage**

```js
{ $count: "total" }
```

### 🧠 Final mental model

* `$count` (stage) → counts documents in pipeline 🧱
* `$count` (accumulator) → counts inside `$group` ⚙️
* `$sum: 1` → classic + more flexible alternative

---

# ----Aggregation- ` $match` and `project`

`$match` and `$project` are **the two most fundamental aggregation stages** 🔥

If you master these, 70% of pipelines become easy.

Let’s go deep but clean.

### 🔥 1. `$match` 🧱

**What it is**

👉 **Aggregation stage**
👉 Filters documents (like `.find()`)

**🧠 Simple intuition**

> “Only let matching documents pass forward”

🔹 Syntax

```js
{
  $match: {
    field: condition
  }
}
```

##### 🔥 Example 1: Basic filtering

```js
db.users.aggregate([
  {
    $match: {
      age: { $gte: 18 }
    }
  }
])
```

🧠 Meaning

👉 Only users with `age >= 18`

##### 🔥 Example 2: Multiple conditions

```js
{
  $match: {
    age: { $gte: 18 },
    city: "Kochi"
  }
}
```

👉 Equivalent to:

```js
age >= 18 AND city = "Kochi"
```

##### 🔥 Example 3: Using `$and`, `$or`

```js
{
  $match: {
    $or: [
      { age: { $lt: 18 } },
      { age: { $gt: 60 } }
    ]
  }
}
```

##### 🔥 Example 4: Using `$expr` (IMPORTANT)

```js
{
  $match: {
    $expr: {
      $gt: ["$price", "$discountPrice"]
    }
  }
}
```

👉 Now using **expression syntax**
👉 Comparing two fields

##### 🔥 Performance Tip (VERY IMPORTANT)

👉 Always put `$match` **as early as possible**

```js
[
  { $match: { status: "active" } },  // ✅ early filter
  { $group: {...} }
]
```

👉 Reduces data → faster pipeline 🚀

### 🔥 2. `$project` 🧾

**What it is**

👉 **Aggregation stage**
👉 Controls:

* Which fields to include/exclude
* How to transform fields

**🧠 Simple intuition**

> “Shape the output document”

**🔹 Syntax**

```js
{
  $project: {
    field1: 1,
    field2: 0,
    newField: expression
  }
}
```

##### 🔥 Example 1: Include fields

```js
{
  $project: {
    name: 1,
    age: 1
  }
}
```

👉 Output:

```js
{ name: "Arjun", age: 22 }
```

##### 🔥 Example 2: Exclude fields

```js
{
  $project: {
    password: 0
  }
}
```

👉 Removes sensitive field 🔐

##### ⚠️ Rule

👉 Cannot mix include & exclude ❌
👉 Except `_id`

Valid:

```js
{ name: 1, age: 1 }
```

```js
{ password: 0 }
```

Invalid:

```js
{ name: 1, password: 0 } ❌
```

##### 🔥 Example 3: Rename field

```js
{
  $project: {
    username: "$name"
  }
}
```

👉 Output:

```js
{ username: "Arjun" }
```

##### 🔥 Example 4: Computed field

```js
{
  $project: {
    total: { $add: ["$price", "$tax"] }
  }
}
```

👉 Adds values 💰

##### 🔥 Example 5: Conditional field

```js
{
  $project: {
    status: {
      $cond: {
        if: { $gte: ["$age", 18] },
        then: "Adult",
        else: "Minor"
      }
    }
  }
}
```

##### 🔥 Example 6: Remove field using `$$REMOVE`

```js
{
  $project: {
    age: {
      $cond: {
        if: { $lt: ["$age", 18] },
        then: "$$REMOVE",
        else: "$age"
      }
    }
  }
}
```

##### 🔥 Example 7: Nested fields

```js
{
  $project: {
    name: 1,
    "address.city": 1
  }
}
```

👉 Includes only specific nested field

### 🔥 Combined Example (REALISTIC)

```js
db.orders.aggregate([
  {
    $match: {
      status: "completed"
    }
  },
  {
    $project: {
      userId: 1,
      total: { $add: ["$price", "$tax"] },
      createdAt: 1
    }
  }
])
```

**🧠 What happens**

1. Filter completed orders
2. Return only selected + computed fields

# ----Aggregation- `$lookup` in detail

**`$lookup` is one of the most important (and interview-heavy) MongoDB stages** 🔥

Think of it as MongoDB’s version of a  **JOIN** .

### 🔥 What is `$lookup`?

👉 **`$lookup` is an aggregation pipeline stage** 🧱
👉 Used to **join documents from another collection**

### 🧠 Simple intuition

👉 SQL:

```sql
SELECT * FROM orders
JOIN users ON orders.userId = users._id
```

👉 MongoDB:

```js
$lookup
```

### 🔹 Basic Syntax (Most Common)

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",         // collection to join
      localField: "userId",  // field in current collection
      foreignField: "_id",   // field in other collection
      as: "userDetails"      // output array field
    }
  }
])
```

### 🔥 Output Behavior

👉 Adds a new field (`userDetails`) as an **array**

```js
{
  _id: 1,
  userId: 101,
  userDetails: [
    { _id: 101, name: "Arjun" }
  ]
}
```

**⚠️ Important**

👉 Even if only one match → still an **array**
👉 No match → empty array `[]`

### 🔹 Field Breakdown

| Field            | Meaning                    |
| ---------------- | -------------------------- |
| `from`         | Target collection          |
| `localField`   | Field in current docs      |
| `foreignField` | Field in joined collection |
| `as`           | Output array field         |

### 🔥 Real Example

**Orders Collection**

```js
{ _id: 1, userId: 101, total: 500 }
```

**Users Collection**

```js
{ _id: 101, name: "Arjun" }
```

**Query:**

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "user"
    }
  }
])
```

**Result:**

```js
{
  _id: 1,
  userId: 101,
  total: 500,
  user: [{ _id: 101, name: "Arjun" }]
}
```

### 🔥 Flattening the result (VERY COMMON)

Since result is array, we often use:

**👉 `$unwind`**

```js
{
  $unwind: "$user"
}
```

👉 Now:

```js
{
  _id: 1,
  userId: 101,
  total: 500,
  user: { _id: 101, name: "Arjun" }
}
```

### 🔥 Advanced `$lookup` (Pipeline form) 🚀

Used when:

* You need conditions
* Multiple joins
* Filtering

Syntax:

```js
{
  $lookup: {
    from: "users",
    let: { userId: "$userId" },
    pipeline: [
      {
        $match: {
          $expr: { $eq: ["$_id", "$$userId"] }
        }
      }
    ],
    as: "user"
  }
}
```

### 🔹 Key Concepts Here

| Keyword                                   | Meaning          |
| ----------------------------------------- | ---------------- |
| `let`                                   | Pass variables   |
| `$$`                                    | Access variables |
| `$expr` | Use expressions in `$match` |                  |

### 🔥 Why use pipeline `$lookup`?

👉 Enables:

* Complex conditions
* Filtering joined data
* Multiple conditions

**Example: Only active users**

```js
pipeline: [
  {
    $match: {
      $expr: {
        $and: [
          { $eq: ["$_id", "$$userId"] },
          { $eq: ["$status", "active"] }
        ]
      }
    }
  }
]
```

### 🔥 Types of JOIN supported

| SQL Join Type | MongoDB `$lookup`                |
| ------------- | ---------------------------------- |
| LEFT JOIN     | ✅ Default                         |
| INNER JOIN    | ❌ (but simulate with `$unwind`) |
| RIGHT JOIN    | ❌                                 |
| FULL JOIN     | ❌                                 |

### Simulating INNER JOIN

```js
{ $unwind: "$user" }
```

👉 Removes documents with empty arrays

### 🔥 Performance Considerations ⚠️

👉 `$lookup` can be expensive

**Best practices:**

* Index `foreignField` 🔥
* Keep collections small if possible
* Avoid large unfiltered joins
* Use pipeline `$lookup` for filtering early

### 🔥 When NOT to use `$lookup`

👉 MongoDB prefers **denormalization**

Instead of:

```js
order → userId → lookup
```

Prefer:

```js
order → embed user data
```

👉 Use `$lookup` only when:

* Data changes frequently
* Cannot duplicate data

### 🧩  `$lookup` WITHOUT `localField` / `foreignField`

**❓ Question**

> “How does `$lookup` work without `localField` and `foreignField`? Are they optional?”

**✅ Short Answer**

👉 **Yes — they are optional**
👉 BUT only when you use the **pipeline form of `$lookup`**

##### 🔥 Two forms of `$lookup`

**🔹 1. Simple form (uses `localField` & `foreignField`)**

```js
{
  $lookup: {
    from: "users",
    localField: "userId",
    foreignField: "_id",
    as: "user"
  }
}
```

👉 MongoDB internally does:

```js
user._id === order.userId
```

👉 Simple equality join ✅

**🔹 2. Pipeline form (your case) 🔥**

```js
{
  $lookup: {
    from: "users",
    let: { userId: "$userId" },
    pipeline: [
      {
        $match: {
          $expr: { $eq: ["$_id", "$$userId"] }
        }
      }
    ],
    as: "user"
  }
}
```

##### 🔥 Why use pipeline form?

Because you can:

**✅ 1. Use complex conditions**

```js
$expr: {
  $and: [
    { $eq: ["$_id", "$$userId"] },
    { $gt: ["$age", 18] }
  ]
}
```

**✅ 2. Use multiple fields**

**✅ 3. Apply transformations inside lookup**

**✅ 4. Use `$project`, `$group`, etc.**

---

# ----Aggregation- `let vs $let`

### 🔹 1. `let` (in `$lookup` stage)

👉 **`let` is used to define variables inside `$lookup`**
👉 These variables can be used inside the `pipeline`

✅ Example

```js
{
  $lookup: {
    from: "users",
    let: { userId: "$userId" },
    pipeline: [
      {
        $match: {
          $expr: { $eq: ["$_id", "$$userId"] }
        }
      }
    ],
    as: "user"
  }
}
```

### 🔥 What’s happening here?

**Step 1: `let`**

```js
let: { userId: "$userId" }
```

👉 For each document:

* Take `userId` from current document
* Store it in a variable called `userId`

**Step 2: Use inside pipeline**

```js
"$$userId"
```

👉 `$$` → means **access a variable**

So:

* `$userId` → field
* `$$userId` → variable

**Step 3: `$expr`**

```js
$expr: { $eq: ["$_id", "$$userId"] }
```

👉 Needed because:

* Normal `$match` can't compare fields/variables
* `$expr` allows expressions

### 🧠 Mental model

👉 `let` = “pass values from outer document into inner pipeline”

### 🔥 Important rules

* Variables defined in `let`:
  * Access using `$$`
* Can only be used inside:
  * `$lookup.pipeline`
  * `$expr`

> #### 🔥 Why can’t you just straight-away use `"$userId"` without uisng `let`?
>
> Inside `$lookup.pipeline`, MongoDB runs a  **separate pipeline on the *foreign collection*** .
>
> 👉 So inside this:
>
> ```js
> pipeline: [
>   {
>     $match: {
>       $expr: { $eq: ["$_id", "$userId"] }
>     }
>   }
> ]
> ```
>
> 👉 Both:
>
> * `$_id` ✅ → refers to **users collection**
> * `$userId` ❌ → MongoDB will ALSO look for this in **users collection**
>
> ⚠️ It does **NOT** refer to the outer document (orders)
>
> #### 🔥 That’s the core issue
>
> 👉 Inside `$lookup.pipeline`:
>
> * You **lose access** to outer document fields
> * Unless you explicitly pass them
>
> #### 🔹 How `let` solves this
>
> ```js
> let: { userId: "$userId" }
> ```
>
> 👉 Now:
>
> * Take `userId` from **outer document**
> * Pass it into pipeline as variable
>
> 🔹 Then use it like:
>
> ```js
> $expr: { $eq: ["$_id", "$$userId"] }
> ```
>
> 👉 Meaning:
>
> * `$_id` → from users collection
> * `$$userId` → from outer document
>
> 🧠 Key rule (VERY IMPORTANT)
>
> | Syntax         | Meaning                               |
> | -------------- | ------------------------------------- |
> | `$field`     | Field in current pipeline document    |
> | `$$variable` | Variable (from `let`or system vars) |
>
> #### 🔥 Why `$expr` is needed here
>
> Because you're doing:
>
> ```js
> "_id" === $$userId
> ```
>
> 👉 This is **field vs variable comparison**
> 👉 Normal `$match` can’t do that
>
> So you must use:
>
> ```js
> $expr
> ```
>
> #### 🔥 When CAN you avoid `let`?
>
> 👉 Only in **simple `$lookup` (non-pipeline)**
>
> ```js
> {
>   $lookup: {
>     from: "users",
>     localField: "userId",
>     foreignField: "_id",
>     as: "user"
>   }
> }
> ```
>
> 👉 Here MongoDB handles it internally — no need for `let`
>
> #### 🔥 So when do you NEED `let`?
>
> 👉 When using:
>
> * `pipeline` inside `$lookup`
> * `$expr`
> * Complex conditions

### 🔹 2. `$let` (Aggregation Expression)

👉 **`$let` is completely different** ⚠️
👉 It’s an  **expression operator** , not a stage option

✅ Syntax

```js
{
  $let: {
    vars: { x: 10, y: 20 },
    in: { $add: ["$$x", "$$y"] }
  }
}
```

**🔥 What it does**

👉 Creates **temporary variables inside an expression**

Example:

```js
db.users.aggregate([
  {
    $project: {
      total: {
        $let: {
          vars: {
            a: "$price",
            b: "$tax"
          },
          in: { $add: ["$$a", "$$b"] }
        }
      }
    }
  }
])
```

### 🔥 Common confusion (IMPORTANT)

❌ These are NOT same:

```js
let: { userId: "$userId" }   // stage option
$let: { vars: {...} }        // expression
```

### 🧠 Final intuition

* `let` → “pass data into lookup pipeline” 🔗
* `$let` → “create variables inside an expression” ⚙️

> #### 🧠 Think of it like JavaScript
>
> ```js
> let x = 10;
> let y = 20;
> return x + y;
> ```
>
> 👉 MongoDB equivalent:
>
> ```js
> $let → vars + in
> ```
>
> #### 🔹 Structure Breakdown
>
> ```js
> $let: {
>   vars: { ... },   // define variables
>   in: { ... }      // use them
> }
> ```
>
> #### 🔥 Step-by-step execution
>
> **✅ Step 1: Define variables**
>
> ```js
> vars: { x: 10, y: 20 }
> ```
>
> 👉 MongoDB creates:
>
> * `x = 10`
> * `y = 20`
>
> ⚠️ These are:
>
> * Temporary
> * Only available inside this `$let`
>
> **✅ Step 2: Use variables**
>
> ```js
> in: { $add: ["$$x", "$$y"] }
> ```
>
> 👉 Important:
>
> * `$$x` → variable access
> * `$$y` → variable access
>
> 👉 Result:
>
> ```
> 10 + 20 = 30
> ```
>
> ##### 🔥 Final Output
>
> ```js
> 30
> ```
>
> #### 🧠 CRITICAL RULES (must understand)
>
> **1. `$$` is mandatory**
>
> | Syntax  | Meaning           |
> | ------- | ----------------- |
> | `$x`  | field in document |
> | `$$x` | variable          |
>
> 👉 So:
>
> ```js
> "$x"   ❌ WRONG (looks for field)
> "$$x"  ✅ CORRECT (uses variable)
> ```
>
> **2. Scope (VERY IMPORTANT)**
>
> 👉 Variables exist **only inside `in`**
>
> ```js
> $let: {
>   vars: { x: 10 },
>   in: "$$x"   // ✅ works
> }
> ```
>
> Outside → ❌ not accessible
>
> **3. Variables can use document fields**
>
> ```js
> $let: {
>   vars: {
>     price: "$price",
>     tax: "$tax"
>   },
>   in: { $add: ["$$price", "$$tax"] }
> }
> ```
>
> 👉 Now it's dynamic per document
>
> #### 🔥 Real-world example
>
> ```js
> db.products.aggregate([
>   {
>     $project: {
>       totalPrice: {
>         $let: {
>           vars: {
>             base: "$price",
>             taxAmount: { $multiply: ["$price", 0.18] }
>           },
>           in: { $add: ["$$base", "$$taxAmount"] }
>         }
>       }
>     }
>   }
> ])
> ```
>
> 🧠 What’s happening?
>
> For each document:
>
> 1. `base = price`
> 2. `taxAmount = price * 0.18`
> 3. return `base + taxAmount`
>
> #### 🔥 Why use `$let`?
>
> **✅ 1. Avoid repetition**
>
> Without `$let`:
>
> ```js
> $add: [
>   "$price",
>   { $multiply: ["$price", 0.18] }
> ]
> ```
>
> 👉 Repeating `$price` multiple times 😬
>
> With `$let`:
>
> ```js
> vars: { base: "$price" }
> ```
>
> 👉 Cleaner + reusable ✅
>
> **✅ 2. Improves readability**
>
> 👉 Complex expressions become manageable
>
> ✅ 3. Small performance benefit
>
> 👉 Avoid recomputing same expression multiple times
>
> #### 🔥 Advanced Example (Nested)
>
> ```js
> $let: {
>   vars: {
>     a: 5,
>     b: {
>       $let: {
>         vars: { x: 2 },
>         in: { $multiply: ["$$x", 3] } // 6
>       }
>     }
>   },
>   in: { $add: ["$$a", "$$b"] } // 5 + 6 = 11
> }
> ```

---

# ----Aggregation- `pipeline` in detail

Let's see this:

```js
{
  $lookup: {
    from: "users",
    let: { userId: "$userId" },
    pipeline: [
      {
        $match: {
          $expr: { $eq: ["$_id", "$$userId"] }
        }
      }
    ],
    as: "user"
  }
}
```

### 🧠 First: What is `pipeline`?

**🔹 Simple meaning**

👉 `pipeline` = **a mini aggregation pipeline that runs on the foreign collection**

**🔥 Think like this:**

You have:

* `orders` collection (outer)
* `users` collection (inner)

👉 `$lookup.pipeline` means:

> “For each order, run this mini query on users”

### 🔹 Without pipeline (simple lookup)

```js
$lookup: {
  from: "users",
  localField: "userId",
  foreignField: "_id",
  as: "user"
}
```

👉 MongoDB internally does:

* match `userId === _id`

### 🔹 With pipeline

```js
pipeline: [ ... ]
```

👉 Now YOU control:

* filtering
* conditions
* transformations

**🔥 Example (important)**

```js
pipeline: [
  {
    $match: { status: "active" }
  }
]
```

👉 Meaning:

> “Only join active users”

### 🔥 Now the REAL problem

You want:

```js
orders.userId === users._id
```

BUT:

👉 Inside pipeline:

* You are **inside users collection**
* You CANNOT directly access `orders.userId`

**🧠 That’s why we need `let`**

```js
let: { userId: "$userId" }
```

👉 This means:

> “Take `userId` from outer (orders) and pass it into pipeline”

### 🔥 Why do we NEED `pipeline`?

**✅ 1. Complex conditions**

```js
pipeline: [
  {
    $match: {
      $expr: {
        $and: [
          { $eq: ["$_id", "$$userId"] },
          { $eq: ["$status", "active"] }
        ]
      }
    }
  }
]
```

👉 Join only active users

**✅ 2. Filtering BEFORE joining**

👉 Better performance ⚡

**✅ 3. Transform joined data**

```js
pipeline: [
  {
    $project: { name: 1 }
  }
]
```

# ----Aggregation- `$expr` in detail

### 🔹 What is `$expr`?

👉 `$expr` allows **expressions inside `$match`**

**❌ Without `$expr`**

```js
$match: {
  _id: "$$userId"  // ❌ NOT allowed
}
```

👉 MongoDB thinks:

* `"$$userId"` is a string (wrong)
* No expression evaluation

**✅ With `$expr`**

```js
$match: {
  $expr: { $eq: ["$_id", "$$userId"] }
}
```

👉 Now MongoDB understands:

* Compare two values dynamically

### 🔥 Why `$expr` is REQUIRED

Normal `$match` only supports:

```js
{ field: value }
```

👉 Example:

```js
{ status: "active" }
```

But here you need:

```js
field === variable
```

👉 That’s NOT allowed normally ❌
👉 So you use `$expr` ✅

### 🔥 Full Flow (Step-by-step)

Suppose:

**Orders**

```js
{ _id: 1, userId: 101 }
```

**Users**

```js
{ _id: 101, name: "Arjun" }
```

### Execution:

**Step 1: `$lookup` starts**

👉 Take order:

```js
{ userId: 101 }
```

**Step 2: `let`**

```js
let: { userId: "$userId" }
```

👉 Creates:

```js
userId = 101
```

**Step 3: Run pipeline on users**

```js
pipeline: [
  {
    $match: {
      $expr: { $eq: ["$_id", "$$userId"] }
    }
  }
]
```

👉 For each user:

Check:

```js
user._id === 101
```

**Step 4: Match found**

```js
{ _id: 101, name: "Arjun" }
```

**Step 5: Attach result**

```js
{
  _id: 1,
  userId: 101,
  user: [{ _id: 101, name: "Arjun" }]
}
```

### 🔥 Why do we NEED `$expr`?

Because:

👉 You want:

```
field vs field
field vs variable
```

👉 NOT:

```
field vs constant
```

### 🔥 Quick Comparison

| Case                      | Works? |
| ------------------------- | ------ |
| `{ _id: 101 }`          | ✅     |
| `{ _id: "$userId" }`    | ❌     |
| `$expr: { $eq: [...] }` | ✅     |

### 🔥 Where `$expr` is MOST used

##### **✅ 1. Inside `$match` (most common)**

```js
db.users.aggregate([
  {
    $match: {
      $expr: { $gt: ["$age", 18] }
    }
  }
])
```

👉 Normally `$match` only supports:

```js
{ age: 18 }
```

👉 `$expr` enables:

```js
age > 18
```

**🔥 Why needed here?**

Because:

👉 You are comparing:

* field vs value dynamically
* field vs field
* field vs variable

##### 🔹 2. Inside `$lookup.pipeline` (VERY COMMON)

```js
$match: {
  $expr: { $eq: ["$_id", "$$userId"] }
}
```

👉 Used when comparing:

* inner field (`$_id`)
* outer variable (`$$userId`)

##### 🔹 3. In normal queries (outside aggregation) ⚡

Yes — you can use `$expr` in **find()** too:

```js
db.users.find({
  $expr: { $gt: ["$salary", "$bonus"] }
})
```

👉 Compare two fields directly!

##### 🔹 4. Inside `$redact` (less common)

```js
{
  $redact: {
    $cond: {
      if: { $gt: ["$level", 5] },
      then: "$$KEEP",
      else: "$$PRUNE"
    }
  }
}
```

👉 `$expr`-like logic used internally

### 🔥 Where `$expr` is NOT needed

If you're doing simple matching:

```js
{ age: 18 }
```

👉 No `$expr` needed ✅

### 🧠 Final Mental Model

* `pipeline` → “Run custom query on joined collection” 🔍
* `let` → “Pass outer data inside” 🔗
* `$expr` → “Allow dynamic comparisons” ⚙️

---

# ----Aggregation- `$cond, if, then else` in detail

`$cond` is MongoDB’s **if–else logic inside aggregation** 🔥

Once you get this, a lot of complex pipelines become easy.

### 🔥 What is `$cond`?

👉 **`$cond` is an aggregation expression operator**
👉 It works like:

```js
if (condition) return trueValue
else return falseValue
```

### 🔹 Syntax (2 forms)

**✅ 1. Object form (most readable)**

```js
{
  $cond: {
    if: <condition>,
    then: <value_if_true>,
    else: <value_if_false>
  }
}
```

**✅ 2. Array form (short form)**

```js
{
  $cond: [ <condition>, <true>, <false> ]
}
```

👉 Same thing, just shorter

### 🔥 Example 1: Simple condition

```js
db.users.aggregate([
  {
    $project: {
      status: {
        $cond: {
          if: { $gte: ["$age", 18] },
          then: "Adult",
          else: "Minor"
        }
      }
    }
  }
])
```

🧠 What happens?

For each user:

* If `age >= 18` → `"Adult"`
* Else → `"Minor"`

### 🔥 Example 2: Using array syntax

```js
{
  $cond: [
    { $gte: ["$age", 18] },
    "Adult",
    "Minor"
  ]
}
```

👉 Same result, just shorter

### 🔥 Example 3: Field vs field (with `$expr` logic)

```js
db.products.aggregate([
  {
    $project: {
      priceStatus: {
        $cond: {
          if: { $gt: ["$price", "$discountPrice"] },
          then: "Discounted",
          else: "Normal"
        }
      }
    }
  }
])
```

### 🔥 Example 5: Nested `$cond` (multiple conditions)

```js
db.users.aggregate([
  {
    $project: {
      category: {
        $cond: {
          if: { $gte: ["$age", 60] },
          then: "Senior",
          else: {
            $cond: {
              if: { $gte: ["$age", 18] },
              then: "Adult",
              else: "Minor"
            }
          }
        }
      }
    }
  }
])
```

🧠 Output:

| age | category |
| --- | -------- |
| 65  | Senior   |
| 25  | Adult    |
| 15  | Minor    |

### 🔥 When do we use `$cond`?

**✅ 1. Conditional fields**

* status labels
* categories

**✅ 2. Conditional calculations**

* discounts
* taxes

**✅ 3. Data transformation**

* flagging values
* formatting output

### 🔥 `$cond` vs `$switch` (important)

👉 `$cond` → simple if–else
👉 `$switch` → multiple conditions (cleaner than nested `$cond`)

### 🧠 Key Rules

**1. Condition must be expression**

```js
{ $gt: ["$age", 18] }
```

**2. Always provide `else`**

👉 Required ❗

**3. Can return ANY value**

* string
* number
* object
* expression

---

# ----Aggregation- `$switch` in detail

`$switch` is basically the **clean, scalable version of multiple `$cond` statements** 🔥

If `$cond` = `if-else`, then `$switch` = `if → else if → else` chain.

### 🔥 What is `$switch`?

👉 **Aggregation expression operator** ⚙️
👉 Used for **multiple conditional branches**

**🧠 Simple intuition**

> “Check multiple conditions in order, return the first match”

### 🔹 Syntax

```js
{
  $switch: {
    branches: [
      { case: <condition1>, then: <result1> },
      { case: <condition2>, then: <result2> },
      ...
    ],
    default: <result_if_no_match>
  }
}
```

### 🔥 How it works

1. Evaluate `case1`
2. If true → return `then1` ✅
3. Else → check next case
4. If none match → return `default`

### 🔥 Example 1: Grade classification

```js
db.students.aggregate([
  {
    $project: {
      grade: {
        $switch: {
          branches: [
            { case: { $gte: ["$marks", 90] }, then: "A" },
            { case: { $gte: ["$marks", 75] }, then: "B" },
            { case: { $gte: ["$marks", 50] }, then: "C" }
          ],
          default: "Fail"
        }
      }
    }
  }
])
```

🧠 Meaning:

| Marks | Grade |
| ----- | ----- |
| ≥90  | A     |
| ≥75  | B     |
| ≥50  | C     |
| else  | Fail  |

### 🔥 Example 2: Age category

```js
db.users.aggregate([
  {
    $addFields: {
      category: {
        $switch: {
          branches: [
            { case: { $lt: ["$age", 18] }, then: "Minor" },
            { case: { $lt: ["$age", 60] }, then: "Adult" }
          ],
          default: "Senior"
        }
      }
    }
  }
])
```

👉 Order matters:

* First matching case is used

### 🔥 Example 3: Returning expressions

```js
db.orders.aggregate([
  {
    $project: {
      discount: {
        $switch: {
          branches: [
            {
              case: { $gt: ["$total", 5000] },
              then: { $multiply: ["$total", 0.2] }
            },
            {
              case: { $gt: ["$total", 1000] },
              then: { $multiply: ["$total", 0.1] }
            }
          ],
          default: 0
        }
      }
    }
  }
])
```

👉 Returns calculated values 💰

### 🔥 Example 4: Using `$and`

```js
{
  $switch: {
    branches: [
      {
        case: {
          $and: [
            { $gte: ["$age", 18] },
            { $lte: ["$age", 25] }
          ]
        },
        then: "Young Adult"
      }
    ],
    default: "Other"
  }
}
```

### 🔥 `$switch` vs `$cond`

| Feature     | `$cond`🔀 | `$switch`🔥 |                   |
| ----------- | --------------------------- | ----------------- |
| Conditions  | 1                           | Multiple          |
| Readability | Poor (nested)               | Clean             |
| Use case    | Simple logic                | Complex branching |

### 🔥 Equivalent using `$cond` (messy 😬)

```js
$cond: {
  if: cond1,
  then: result1,
  else: {
    $cond: {
      if: cond2,
      then: result2,
      else: result3
    }
  }
}
```

### 🔥 Important Rules ⚠️

**1. Order matters**

👉 First true condition wins

**2. `default` is optional but recommended**

👉 If missing and no case matches → error ❌

**3. Each branch must have:**

```js
{ case: ..., then: ... }
```

### 🧠 Real-world use cases

* Grade systems 🎓
* Pricing categories 💰
* User segmentation 👥
* Discount rules 🛒

---

# ----Aggregation stage operators, Accumulators, Expression operators

### 🧠 Big Picture

MongoDB aggregation has  **3 main categories** :

🔥 1. Pipeline Stages (Stage operators) 🧱

🔥 2. Expression Operators ⚙️

🔥 3. Accumulators 📊

(Plus a few sub-types we’ll also cover)

### 🔥 1. Pipeline Stages (Stage operators) 🧱

👉 These are the **steps** in the aggregation pipeline

```js
db.users.aggregate([
  { $match: {...} },
  { $group: {...} },
  { $project: {...} }
])
```

**✅ Common Stage Operators**

* `$match` → filter documents
* `$group` → group documents
* `$project` → shape output
* `$addFields` → add fields
* `$lookup` → join collections
* `$unwind` → flatten arrays
* `$sort` → sort documents
* `$limit` → limit results
* `$skip` → skip documents
* `$count` → count docs
* `$facet` → multiple pipelines
* `$bucket` → grouping ranges
* `$set` → alias of `$addFields`
* `$unset` → remove fields

### 🔥 2. Aggregation Expression Operators ⚙️

👉 These are **used inside stages**
👉 They perform calculations, logic, transformations

#### 🔹 Types of Expression Operators

**✅ A. Arithmetic Operators ➕**

* `$add`
* `$subtract`
* `$multiply`
* `$divide`
* `$mod`

```js
{ $add: ["$price", "$tax"] }
```

**✅ B. Comparison Operators ⚖️**

* `$eq`
* `$ne`
* `$gt`
* `$gte`
* `$lt`
* `$lte`

```js
{ $gte: ["$age", 18] }
```

**✅ C. Logical Operators 🧠**

* `$and`
* `$or`
* `$not`

```js
{ $and: [ { $gt: ["$age", 18] }, { $lt: ["$age", 60] } ] }
```

**✅ D. Conditional Operators 🔀**

* `$cond` 🔥
* `$switch`
* `$ifNull`

```js
{ $cond: [ { $gte: ["$age", 18] }, "Adult", "Minor" ] }
```

**✅ E. Array Operators 📦**

* `$size`
* `$filter`
* `$map`
* `$in`
* `$arrayElemAt`
* `$concatArrays`

```js
{ $size: "$hobbies" }
```

**✅ F. String Operators 🔤**

* `$concat`
* `$substr`
* `$toUpper`
* `$toLower`
* `$trim`

```js
{ $concat: ["$firstName", " ", "$lastName"] }
```

**✅ G. Date Operators 📅**

* `$year`
* `$month`
* `$dayOfMonth`
* `$dateToString`
* `$now`

**✅ H. Type Operators 🔍**

* `$type`
* `$convert`
* `$toInt`
* `$toString`

**✅ I. Variable Operators 🧵**

* `$let` 🔥
* `$$ROOT`
* `$$CURRENT`

### 🔥 3. Accumulators 📊

👉 Used mainly in `$group`
👉 Combine multiple documents into one value

**✅ Common Accumulators**

* `$sum`
* `$avg`
* `$min`
* `$max`
* `$push`
* `$addToSet` 🔥
* `$first`
* `$last`
* `$count`

```js
$group: {
  _id: "$category",
  total: { $sum: "$price" }
}
```

### 🔥 Special Case: Dual-role Operators

Some operators work as:

| Operator      | Role 1      | Role 2          |
| ------------- | ----------- | --------------- |
| `$sum`      | accumulator | expression      |
| `$avg`      | accumulator | expression      |
| `$addToSet` | accumulator | update operator |

### 🔥 4. Query Operators (Different category ⚠️)

Used in `.find()`:

* `$in`
* `$exists`
* `$regex`
* `$elemMatch`

👉 Not aggregation-specific

### 🔥 5. Update Operators ⚙️

Used in `updateOne()`:

* `$set`
* `$inc`
* `$push`
* `$addToSet`

### 🧠 How everything fits together

Example:

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$category",
      total: { $sum: "$price" },  // accumulator
      avg: { $avg: "$price" }     // accumulator
    }
  },
  {
    $project: {
      totalWithTax: {
        $multiply: ["$total", 1.18]  // expression
      }
    }
  }
])
```

---

# ----Aggregation- `addFields` in detail

### 👉 What it is

👉 **Pipeline stage**
👉 Used to **add or modify fields in each document**

### 🔹 Basic Example

```js
db.users.aggregate([
  {
    $addFields: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] }
    }
  }
])
```

🧠 What happens:

Input:

```js
{ firstName: "Arjun", lastName: "Suresh" }
```

Output:

```js
{
  firstName: "Arjun",
  lastName: "Suresh",
  fullName: "Arjun Suresh"
}
```

### 🔹 Example 2: Modify existing field

```js
db.products.aggregate([
  {
    $addFields: {
      price: { $multiply: ["$price", 1.18] }
    }
  }
])
```

👉 Updates `price` with tax included

### 🔹 Example 3: Add nested field

```js
db.users.aggregate([
  {
    $addFields: {
      address: {
        city: "Kochi",
        country: "India"
      }
    }
  }
])
```

### 🔹 Example 4: Using `$cond`

```js
db.users.aggregate([
  {
    $addFields: {
      status: {
        $cond: [
          { $gte: ["$age", 18] },
          "Adult",
          "Minor"
        ]
      }
    }
  }
])
```

### 🔥 Key Points

* Works on **each document individually**
* Uses **expression operators inside**
* Alias of `$set`

### `$project` vs `$addFields`

Y**ou absolutely can do the same using `$project`** ✅

But there’s a **subtle and important difference** you need to understand 🔥

##### 🔥 Example

**Using `$addFields`**

```js
db.users.aggregate([
  {
    $addFields: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] }
    }
  }
])
```

**🔥 Equivalent using `$project`**

```js
db.users.aggregate([
  {
    $project: {
      firstName: 1,
      lastName: 1,
      fullName: { $concat: ["$firstName", " ", "$lastName"] }
    }
  }
])
```

##### 🧠 So what’s the difference?

**🔹 `$addFields`**

👉 Adds new field **WITHOUT removing existing fields**

```js
Input:
{ firstName: "Arjun", lastName: "Suresh", age: 22 }

Output:
{ firstName: "Arjun", lastName: "Suresh", age: 22, fullName: "Arjun Suresh" }
```

**🔹 `$project`**

👉 Controls **which fields to include/exclude**

```js
Output:
{ firstName: "Arjun", lastName: "Suresh", fullName: "Arjun Suresh" }
```

👉 Notice:

* `age` is gone ❌ (because not included)

##### 🔥 Key Difference

| Feature             | `$addFields`🧱 | `$project`📦 |                          |
| ------------------- | --------------------------------- | ------------------------ |
| Keeps all fields    | ✅ Yes                            | ❌ Only specified fields |
| Main purpose        | Add/modify fields                 | Shape/filter output      |
| Risk of losing data | ❌ No                             | ⚠️ Yes                 |

##### 🔥 Important Insight

👉 `$addFields` is basically:

```js
$project + include everything automatically
```

---

# ---Aggregation- `$addToSet` in detail

### 👉 What it is

👉 Two roles:

1. **Accumulator (in aggregation)**
2. **Update operator (in update queries)**

### 🔹 A. `$addToSet` as ACCUMULATOR

👉 Used inside `$group`
👉 Creates array of **unique values**

##### Example 1: Unique users per category

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$category",
      users: { $addToSet: "$userId" }
    }
  }
])
```

🧠 Input:

```js
{ category: "electronics", userId: 1 }
{ category: "electronics", userId: 1 }
{ category: "electronics", userId: 2 }
```

Output:

```js
{
  _id: "electronics",
  users: [1, 2]  // duplicates removed
}
```

##### 🔹 Example 2: Unique tags

```js
db.posts.aggregate([
  {
    $group: {
      _id: null,
      allTags: { $addToSet: "$tag" }
    }
  }
])
```

### 🔹 B. `$addToSet` as UPDATE OPERATOR

👉 Adds value to array **only if not already present**

##### Example 3:

```js
db.users.updateOne(
  { _id: 1 },
  {
    $addToSet: { hobbies: "cricket" }
  }
)
```

##### 🧠 Behavior:

| Before       | After                  |
| ------------ | ---------------------- |
| ["cricket"]  | ["cricket"]            |
| ["football"] | ["football","cricket"] |

##### 🔥 Difference from `$push`

```js
$push → allows duplicates
$addToSet → prevents duplicates
```

### 🔥 Example (POWERFUL)

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$category",
      users: { $addToSet: "$userId" }
    }
  },
  {
    $addFields: {
      userCount: { $size: "$users" }
    }
  }
])
```

🧠 Output:

```js
{
  _id: "electronics",
  users: [1, 2],
  userCount: 2
}
```

---

# ----Aggregation- `$facet` in detail

`$facet` is one of the most **powerful (and often misunderstood) aggregation stages** 🔥

Let’s break it down cleanly and deeply so it sticks 🧠✨

### 🔥 What is `$facet`?

👉 **`$facet` is a pipeline stage** 🧱
👉 It lets you run **multiple aggregation pipelines in parallel on the same input**

**🧠 Simple intuition**

👉 Think of it like:

> “Take the same data, process it in different ways at the same time”

### 🔥 Syntax

```js
{
  $facet: {
    pipeline1Name: [ ...pipeline stages... ],
    pipeline2Name: [ ...pipeline stages... ],
    ...
  }
}
```

**🔹 Output structure**

👉 Output is a **single document** with multiple fields:

```js
{
  pipeline1Name: [ ...results ],
  pipeline2Name: [ ...results ]
}
```

### 🔥 Example 1: Basic understanding

```js
db.products.aggregate([
  {
    $facet: {
      expensive: [
        { $match: { price: { $gt: 1000 } } }
      ],
      cheap: [
        { $match: { price: { $lte: 1000 } } }
      ]
    }
  }
])
```

🧠 Output:

```js
{
  expensive: [ ...products > 1000 ],
  cheap: [ ...products <= 1000 ]
}
```

👉 Same input → split into two result sets

### 🔥 Example 2: Count + Data together (VERY COMMON)

```js
db.products.aggregate([
  {
    $facet: {
      totalCount: [
        { $count: "count" }
      ],
      products: [
        { $sort: { price: -1 } },
        { $limit: 5 }
      ]
    }
  }
])
```

🧠 Output:

```js
{
  totalCount: [{ count: 100 }],
  products: [ top 5 expensive products ]
}
```

👉 Used in:

* pagination APIs 🔥

### 🔥 Example 3: Grouping + Stats

```js
db.orders.aggregate([
  {
    $facet: {
      totalRevenue: [
        {
          $group: {
            _id: null,
            total: { $sum: "$amount" }
          }
        }
      ],
      avgOrder: [
        {
          $group: {
            _id: null,
            avg: { $avg: "$amount" }
          }
        }
      ]
    }
  }
])
```

🧠 Output:

```js
{
  totalRevenue: [{ total: 50000 }],
  avgOrder: [{ avg: 250 }]
}
```

### 🔥 Example 4: Pagination (REAL-WORLD)

```javascript
db.users.aggregate([
  {
    $facet: {
      metadata: [
        { $count: "total" }
      ],
      data: [
        { $skip: 10 },
        { $limit: 5 }
      ]
    }
  }
])
```

### 🔥 Key Characteristics

**✅ 1. Parallel pipelines**

👉 Each pipeline runs independently

**✅ 2. Same input**

👉 All pipelines receive **same documents**

**✅ 3. Single output document**

👉 Results grouped together

### 🔥 When to use `$facet`?

**✅ 1. Pagination + total count**

(VERY common interview use-case)

**✅ 2. Dashboard stats**

* total
* average
* categories

**✅ 3. Multi-filter data**

* different categories at once

### 🔥 Performance insight ⚠️

👉 `$facet` runs ALL pipelines on full input

So:

* Can be expensive ❗

### 🔹 Best practice

👉 Filter BEFORE `$facet`

```js
[
  { $match: { status: "active" } },
  { $facet: { ... } }
]
```

### 🔥 Important limitation

👉 Pipelines inside `$facet`:

* Cannot share data between each other ❌
* Are completely independent

### 🔥 Common mistake ❌

Trying to use output of one pipeline inside another:

👉 Not possible

---

# ----Aggregation- `$replaceRoot` in detail

### 👉 What it is

👉 **Aggregation stage**
👉 Replaces the **entire document** with another document

**🧠 Simple intuition**

> “Throw away the current document and replace it with something else”

### 🔹 Basic Syntax

```js
{
  $replaceRoot: {
    newRoot: <expression>
  }
}
```

### 🔥 Example 1: Replace with nested object

Input:

```js
{
  _id: 1,
  name: "Arjun",
  address: {
    city: "Kochi",
    country: "India"
  }
}
```

Query:

```js
db.users.aggregate([
  {
    $replaceRoot: {
      newRoot: "$address"
    }
  }
])
```

Output:

```js
{
  city: "Kochi",
  country: "India"
}
```

👉 Entire document replaced with `address`

### 🔥 Example 2: After `$lookup` + `$unwind`

```js
db.orders.aggregate([
  { $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "user"
  }},
  { $unwind: "$user" },
  {
    $replaceRoot: {
      newRoot: "$user"
    }
  }
])
```

👉 Output becomes only  **user documents** , not orders

### 🔥 Example 3: Merge fields (using `$mergeObjects`)

```js
{
  $replaceRoot: {
    newRoot: {
      $mergeObjects: ["$$ROOT", "$address"]
    }
  }
}
```

👉 Combines root + nested object

> #### 🧠 What is the above code doing (in one line)?
>
> 👉 **It merges the entire document with the `address` object and makes that the new document**
>
> #### 🔹 Step-by-step breakdown
>
> **🔹 1. `$$ROOT`**
>
> 👉 Special variable
> 👉 Represents the **entire current document**
>
> Example input:
>
> ```js
> {
>   _id: 1,
>   name: "Arjun",
>   age: 22,
>   address: {
>     city: "Kochi",
>     country: "India"
>   }
> }
> ```
>
> 👉 Here:
>
> ```js
> $$ROOT =
> {
>   _id: 1,
>   name: "Arjun",
>   age: 22,
>   address: { city: "Kochi", country: "India" }
> }
> ```
>
> **🔹 2. `$address`**
>
> ```js
> "$address"
> ```
>
> 👉 Refers to:
>
> ```js
> {
>   city: "Kochi",
>   country: "India"
> }
> ```
>
> **🔹 3. `$mergeObjects`**
>
> ```js
> $mergeObjects: ["$$ROOT", "$address"]
> ```
>
> 👉 This merges both objects into one
>
> Result:
>
> ```js
> {
>   _id: 1,
>   name: "Arjun",
>   age: 22,
>   address: { city: "Kochi", country: "India" },
>   city: "Kochi",
>   country: "India"
> }
> ```
>
> 👉 Notice:
>
> * `city` and `country` are now at **top level**
> * `address` is still there
>
> **🔹 4. `$replaceRoot`**
>
> ```js
> $replaceRoot: { newRoot: ... }
> ```
>
> 👉 Replaces the **entire document** with the merged result
>
> **🔥 Final Output**
>
> ```js
> {
>   _id: 1,
>   name: "Arjun",
>   age: 22,
>   address: { city: "Kochi", country: "India" },
>   city: "Kochi",
>   country: "India"
> }
> ```
>
> #### 🔥 Why do we use this?
>
> 👉 To **flatten nested fields**
>
> Instead of:
>
> ```js
> address.city
> address.country
> ```
>
> 👉 You get:
>
> ```js
> city
> country
> ```
>
> #### 🔥 Important Behavior (VERY IMPORTANT)
>
> **👉 Order matters in `$mergeObjects`**
>
> ```js
> $mergeObjects: ["$$ROOT", "$address"]
> ```
>
> 👉 If same field exists:
>
> * Later object **overwrites earlier one**
>
> Example:
>
> ```js
> {
>   name: "Arjun",
>   address: {
>     name: "Override"
>   }
> }
> ```
>
> Result:
>
> ```js
> {
>   name: "Override"
> }
> ```
>
> #### 🔥 Common variation (cleaner)
>
> 👉 Remove original nested field:
>
> ```js
> {
>   $replaceRoot: {
>     newRoot: {
>       $mergeObjects: [
>         "$$ROOT",
>         "$address"
>       ]
>     }
>   }
> },
> {
>   $project: {
>     address: 0
>   }
> }
> ```

### 🔥 Key Use Cases

* Flatten nested documents 📦
* Extract sub-documents
* Reshape output completely
* Clean up after `$lookup`

### ⚠️ Important

👉 You lose ALL original fields unless you merge them

---

# ----Aggregation- `$redact` in detail

### 👉 What it is

👉 **Aggregation stage**
👉 Used for **conditional document filtering at ANY level (even nested)**

**🧠 Simple intuition**

> “Decide what to KEEP, REMOVE, or GO DEEPER based on conditions”

### 🔹 Special keywords

| Keyword       | Meaning                    |
| ------------- | -------------------------- |
| `$$KEEP`    | Keep document              |
| `$$PRUNE`   | Remove document            |
| `$$DESCEND` | Go deeper into nested docs |

### 🔹 Basic Syntax

```js
{
  $redact: {
    $cond: {
      if: <condition>,
      then: "$$KEEP",
      else: "$$PRUNE"
    }
  }
}
```

### 🔥 Example 1: Simple filtering

```js
db.users.aggregate([
  {
    $redact: {
      $cond: {
        if: { $gte: ["$age", 18] },
        then: "$$KEEP",
        else: "$$PRUNE"
      }
    }
  }
])
```

👉 Same as `$match`, but with deeper control

### 🔥 Example 2: Nested document control (IMPORTANT)

Input:

```js
{
  name: "Arjun",
  level: 3,
  documents: [
    { title: "Doc1", level: 2 },
    { title: "Doc2", level: 5 }
  ]
}
```

Query:

```js
{
  $redact: {
    $cond: {
      if: { $lte: ["$level", 3] },
      then: "$$DESCEND",
      else: "$$PRUNE"
    }
  }
}
```

🧠 What happens:

* Checks root level → DESCEND
* Checks each nested doc
* Removes those with level > 3

### 🔥 Example 3: Role-based access (REAL-WORLD)

```js
const userAccessLevel = 3;

db.docs.aggregate([
  {
    $redact: {
      $cond: {
        if: { $lte: ["$level", userAccessLevel] },
        then: "$$DESCEND",
        else: "$$PRUNE"
      }
    }
  }
])
```

👉 Used for:

* permission filtering 🔐
* access control

### 🔥 Difference from `$match`

| Feature    | `$match`   | `$redact` |                      |
| ---------- | -------------------------- | -------------------- |
| Level      | Top-level only             | Nested documents too |
| Complexity | Simple                     | Advanced             |
| Use case   | Filtering                  | Access control       |

### 🔥 When to use `$redact`?

**✅ Use when:**

* You need **nested filtering**
* Role-based data access
* Conditional visibility

**❌ Avoid when:**

* Simple filtering → use `$match` (faster)

---

# ----Aggregation- `$bucket` in detail

`$bucket` is a  **grouping stage based on ranges** , and it’s super useful for analytics (like histograms) 📊🔥

### 🔥 What is `$bucket`?

👉 **`$bucket` is an aggregation pipeline stage** 🧱
👉 It groups documents into **ranges (buckets)** based on a field value

**🧠 Simple intuition**

> “Group documents like: 0–10, 10–20, 20–30…”

Instead of grouping by exact value (`$group`), you group by **ranges**

### 🔹 Basic Syntax

```js
{
  $bucket: {
    groupBy: <field/expression>,
    boundaries: [b1, b2, b3, ...],
    default: <optional>,
    output: { ... }
  }
}
```

### 🔥 Key fields

| Field          | Meaning                         |
| -------------- | ------------------------------- |
| `groupBy`    | Field to group on               |
| `boundaries` | Range limits                    |
| `default`    | Bucket for out-of-range values  |
| `output`     | Aggregations inside each bucket |

### 🔥 Example 1: Age grouping

```js
db.users.aggregate([
  {
    $bucket: {
      groupBy: "$age",
      boundaries: [0, 18, 30, 50, 100],
      default: "Other",
      output: {
        count: { $sum: 1 }
      }
    }
  }
])
```

**🧠 Meaning:**

* 0–18 → bucket1
* 18–30 → bucket2
* 30–50 → bucket3
* 50–100 → bucket4
* Others → `"Other"`

**🔹 Output:**

```js
[
  { _id: 0, count: 5 },
  { _id: 18, count: 10 },
  { _id: 30, count: 8 },
  { _id: 50, count: 2 },
  { _id: "Other", count: 1 }
]
```

👉 `_id` = lower boundary of bucket

### 🔥 Example 2: Price ranges

```js
db.products.aggregate([
  {
    $bucket: {
      groupBy: "$price",
      boundaries: [0, 500, 1000, 5000],
      default: "expensive",
      output: {
        count: { $sum: 1 },
        avgPrice: { $avg: "$price" }
      }
    }
  }
])
```

**🧠 Output:**

```js
[
  { _id: 0, count: 10, avgPrice: 300 },
  { _id: 500, count: 5, avgPrice: 800 },
  { _id: 1000, count: 2, avgPrice: 2000 },
  { _id: "expensive", count: 1, avgPrice: 7000 }
]
```

### 🔥 Example 3: Without `output` (default count)

```js
{
  $bucket: {
    groupBy: "$age",
    boundaries: [0, 20, 40, 60]
  }
}
```

👉 Automatically does:

```js
count: { $sum: 1 }
```

### 🔥 Example 4: Categorizing scores

```js
db.students.aggregate([
  {
    $bucket: {
      groupBy: "$marks",
      boundaries: [0, 40, 60, 80, 100],
      default: "Invalid",
      output: {
        students: { $push: "$name" }
      }
    }
  }
])
```

**🧠 Output:**

```js
[
  { _id: 0, students: ["A", "B"] },
  { _id: 40, students: ["C"] },
  { _id: 60, students: ["D", "E"] }
]
```

### 🔥 Important Rules ⚠️

**1. Boundaries must be sorted**

```js
[0, 10, 20, 30] ✅
[10, 0, 30] ❌
```

**2. Buckets are:**

👉 Inclusive lower bound
👉 Exclusive upper bound

```js
[0, 10) → includes 0, excludes 10
```

**3. If value doesn’t fit**

👉 Goes to `default` (if provided)
👉 Otherwise → error ❌

### 🔥 `$bucket` vs `$group`

| Feature       | `$bucket`📊 | `$group`📦 |                   |
| ------------- | ---------------------------- | ----------------- |
| Grouping type | Range-based                  | Exact value       |
| Use case      | Analytics                    | Aggregation       |
| Example       | age ranges                   | category grouping |

### 🔥 `$bucket` vs `$bucketAuto`

👉 `$bucketAuto` automatically decides ranges

```js
{
  $bucketAuto: {
    groupBy: "$price",
    buckets: 4
  }
}
```

👉 MongoDB creates 4 equal distribution buckets

### 🧠 When to use `$bucket`?

✅ Use when:

* You need **fixed ranges**
* You know boundaries
* Analytics (charts, reports)

❌ Avoid when:

* You don’t know ranges → use `$bucketAuto`

---

# ----Aggregation- `$$current, $first and $last`

Great — these three are **very important but often misunderstood together** 🔥

Let’s break them down clearly with  **deep intuition + multiple examples** .

### 🧠 Overview

| Operator      | Type                     | Purpose                      |
| ------------- | ------------------------ | ---------------------------- |
| `$$CURRENT` | System variable          | Current document context     |
| `$first`    | Accumulator / expression | First value in a group/order |
| `$last`     | Accumulator / expression | Last value in a group/order  |

### 🔥 1. `$$CURRENT` 🧵

👉 What it is

👉 A **system variable**
👉 Refers to the **current document being processed at that stage**

**🧠 Simple intuition**

> “This is the document I’m currently working on”

##### 🔹 Example 1: Basic usage

```js
db.users.aggregate([
  {
    $project: {
      originalDoc: "$$CURRENT"
    }
  }
])
```

🧠 Input:

```js
{ name: "Arjun", age: 22 }
```

Output:

```js
{
  originalDoc: { name: "Arjun", age: 22 }
}
```

##### 🔥 Difference: `$` vs `$$CURRENT`

| Syntax        | Meaning        |
| ------------- | -------------- |
| `$name`     | Field value    |
| `$$CURRENT` | Whole document |

##### 🔹 Example 2: Modify structure

```js
db.users.aggregate([
  {
    $addFields: {
      copy: "$$CURRENT"
    }
  }
])
```

👉 Creates a duplicate of entire document inside `copy`

##### 🔥 Special Insight

👉 In most cases:

```js
"$field" === "$$CURRENT.field"
```

👉 But `$$CURRENT` is useful when:

* Passing full document
* Using `$let`
* Nested contexts

### 🔥 2. `$first` 📊

👉 What it is

👉 **Accumulator (mostly used in `$group`)**
👉 Returns the **first value in a group**

##### ⚠️ VERY IMPORTANT

👉 “First” depends on **order of documents**
👉 So **use `$sort` before `$group`**

##### 🔹 Example 1: First user per category

```js
db.orders.aggregate([
  { $sort: { date: 1 } },  // earliest first
  {
    $group: {
      _id: "$category",
      firstOrder: { $first: "$date" }
    }
  }
])
```

**🧠 Meaning:**

👉 Get **earliest order per category**

##### 🔹 Example 2: Get full document

```js
{
  $group: {
    _id: "$category",
    firstDoc: { $first: "$$ROOT" }
  }
}
```

👉 Returns full first document of each group

### 🔥 3. `$last` 📊

**👉 What it is**

👉 Same as `$first`, but returns **last value**

##### 🔹 Example 1: Last order per category

```js
db.orders.aggregate([
  { $sort: { date: 1 } },
  {
    $group: {
      _id: "$category",
      lastOrder: { $last: "$date" }
    }
  }
])
```

👉 Returns **latest order**

##### 🔹 Example 2: Full last document

```js
{
  $group: {
    _id: "$category",
    lastDoc: { $last: "$$ROOT" }
  }
}
```

### 🔥 `$first` vs `$last`

| Feature         | `$first`       | `$last` |                    |
| --------------- | ---------------------------- | ------------------ |
| Meaning         | First in order               | Last in order      |
| Needs `$sort` | ✅ Yes (important)           | ✅ Yes (important) |
| Common use      | Earliest                     | Latest             |

### 🔥 CRITICAL INTERVIEW POINT

👉 Without `$sort`, results are **unpredictable** ❌

```js
// WRONG
$group: {
  first: { $first: "$date" }
}
```

👉 Order is undefined 😬

### 🔥 Combined Example (VERY IMPORTANT)

```js
db.orders.aggregate([
  { $sort: { date: 1 } },
  {
    $group: {
      _id: "$userId",
      firstOrder: { $first: "$$ROOT" },
      lastOrder: { $last: "$$ROOT" }
    }
  }
])
```

**🧠 Output:**

```js
{
  _id: 1,
  firstOrder: { ...earliest order... },
  lastOrder: { ...latest order... }
}
```

---

# ----Aggregation- `$regex` in detail

### 👉 What it is

👉 A **query operator**
👉 Used to match **patterns in strings (like search)**

**🧠 Simple intuition**

> “Find text that matches a pattern”

### 🔹 Basic Syntax (in `find()` or `$match`)

```js
{ field: { $regex: "pattern", $options: "flags" } }
```

### 🔥 `$regex` `$options` flags 🧾

👉 `$options` modifies regex behavior

##### 🔥 All supported flags

**🔹 `i` → Case insensitive 🔤**

```js
{ name: { $regex: "ar", $options: "i" } }
```

👉 Matches:

* "Arjun"
* "ARUN"

**🔹 `m` → Multiline 🧾**

👉 `^` and `$` match **start/end of each line**

```js
{ text: { $regex: "^Hello", $options: "m" } }
```

**🔹 `s` → Dotall 🌊**

👉 `.` matches **newline also**

```js
{ text: { $regex: "A.*B", $options: "s" } }
```

**🔹 `x` → Ignore whitespace 🧠**

👉 Allows readable regex

```js
{ field: { $regex: "a b c", $options: "x" } }
```

**🔹 `l` → Locale dependent (RARE) 🌍**

👉 Uses locale rules for matching
👉 Rarely used

**🔹 `u` → Unicode 🌐**

👉 Enables Unicode matching

##### 🔥 Commonly used in real world

👉 Mostly only these:

```js
i  // case insensitive
m  // multiline
```

##### 🔥 Combine flags

```js
{ name: { $regex: "^ar", $options: "im" } }
```

### 🔥 Example 1: Basic match

```js
db.users.find({
  name: { $regex: "ar" }
})
```

👉 Matches:

* "Arjun"
* "Karthik"
* "Zara"

### 🔥 Example 2: Case insensitive

```js
db.users.find({
  name: { $regex: "ar", $options: "i" }
})
```

👉 Matches:

* "Arjun"
* "ARUN"
* "kArthik"

### 🔥 Example 3: Starts with

```js
db.users.find({
  name: { $regex: "^Ar" }
})
```

👉 Matches:

* "Arjun"
* NOT "Karthik"

### 🔥 Example 4: Ends with

```js
db.users.find({
  name: { $regex: "an$" }
})
```

👉 Matches:

* "Kiran"
* "Arman"

### 🔥 Example 5: Inside aggregation

```js
db.users.aggregate([
  {
    $match: {
      name: { $regex: "^A", $options: "i" }
    }
  }
])
```

### 🔥 Common regex patterns

| Pattern   | Meaning           |
| --------- | ----------------- |
| `^A`    | starts with A     |
| `A$`    | ends with A       |
| `.`     | any character     |
| `.*`    | any sequence      |
| `[a-z]` | lowercase letters |

### ⚠️ Performance note

👉 `$regex` can be slow if:

* no index
* pattern starts with `.*`

---

# ----Aggregation- `ifNull` in detail

### 👉 What it is

👉 **Aggregation expression operator**
👉 Replaces `null` or missing values

🧠 Simple intuition

> “If value is null, use fallback”

### 🔹 Syntax

```js
{ $ifNull: [expression, fallback] }
```

### 🔥 Example 1: Basic usage

```js
db.users.aggregate([
  {
    $project: {
      name: 1,
      city: { $ifNull: ["$city", "Unknown"] }
    }
  }
])
```

🧠 Input:

```js
{ name: "Arjun", city: null }
{ name: "Rahul" }
```

Output:

```js
{ name: "Arjun", city: "Unknown" }
{ name: "Rahul", city: "Unknown" }
```

### 🔥 Example 2: Multiple fallbacks

```js
{
  $ifNull: ["$nickname", "$name", "Anonymous"]
}
```

👉 Works like:

* use `nickname` if exists
* else `name`
* else `"Anonymous"`

### 🔥 Example 3: Numeric default

```js
db.orders.aggregate([
  {
    $project: {
      total: { $ifNull: ["$total", 0] }
    }
  }
])
```

### 🔥 Example 4: Combine with `$cond`

```js
{
  $cond: {
    if: { $ifNull: ["$age", false] },
    then: "Has Age",
    else: "No Age"
  }
}
```

### 🔥 `$ifNull` vs `$cond`

| Feature    | `$ifNull`🔀 | `$cond`🔥 |               |
| ---------- | --------------------------- | ------------- |
| Purpose    | Null handling               | General logic |
| Simplicity | Very simple                 | More flexible |

### 🧠 Important behavior

👉 `$ifNull` treats these as null:

* `null`
* missing field

---

# ----Aggregation- `$convert, $toInt & $toString` in detail

These three are all about **type conversion in MongoDB aggregation** 🔥

Let’s break them down in a way that makes their  **relationship very clear** .

### 🧠 Big Picture

| Operator      | Type               | Purpose                        |
| ------------- | ------------------ | ------------------------------ |
| `$convert`  | General conversion | Full control (safe + flexible) |
| `$toInt`    | Shortcut           | Convert to integer             |
| `$toString` | Shortcut           | Convert to string              |

### 🔥 1. `$convert` ⚙️ (MASTER operator)

**👉 What it is**

👉 **Aggregation expression operator**
👉 Converts a value to any type with **error handling**

**🔹 Syntax**

```js
{
  $convert: {
    input: <expression>,
    to: <type>,
    onError: <value>,   // optional
    onNull: <value>     // optional
  }
}
```

##### 🔥 Example 1: String → Number

```js
db.users.aggregate([
  {
    $project: {
      age: {
        $convert: {
          input: "$age",
          to: "int"
        }
      }
    }
  }
])
```

🧠 Input:

```js
{ age: "25" }
```

Output:

```js
{ age: 25 }
```

##### 🔥 Example 2: Handle errors

```js
{
  $convert: {
    input: "$age",
    to: "int",
    onError: 0,
    onNull: -1
  }
}
```

🧠 Behavior:

| Input     | Output |
| --------- | ------ |
| `"25"`  | 25     |
| `"abc"` | 0      |
| `null`  | -1     |

##### 🔥 Example 3: Convert to date

```js
{
  $convert: {
    input: "$createdAt",
    to: "date"
  }
}
```

##### 🔥 Supported types

* `"int"`
* `"double"`
* `"string"`
* `"bool"`
* `"date"`
* `"objectId"`

##### 🧠 Key Insight

👉 `$convert` = **safe + configurable conversion**

### 🔥 2. `$toInt` 🔢 (shortcut)

**👉 What it is**

👉 Shortcut for:

```js
{ $convert: { input: ..., to: "int" } }
```

##### 🔹 Example 1

```js
db.users.aggregate([
  {
    $project: {
      age: { $toInt: "$age" }
    }
  }
])
```

🧠 Input:

```js
{ age: "25" }
```

Output:

```js
{ age: 25 }
```

##### ⚠️ Important

👉 If conversion fails → **throws error** ❌

```js
{ age: "abc" }
```

👉 This will crash the pipeline 😬

🔥 Example 2: Fix with `$convert`

```js
{
  $convert: {
    input: "$age",
    to: "int",
    onError: 0
  }
}
```

👉 Safer ✅

### 🔥 3. `$toString` 🔤 (shortcut)

**👉 What it is**

👉 Converts value to string

##### 🔹 Example 1

```js
db.users.aggregate([
  {
    $project: {
      userIdStr: { $toString: "$_id" }
    }
  }
])
```

🧠 Input:

```js
{ _id: ObjectId("abc123") }
```

Output:

```js
{ userIdStr: "abc123" }
```

##### 🔥 Example 2: Number → String

```js
{
  $toString: "$age"
}
```

Output:

```js
"25"
```

##### ⚠️ Same issue as `$toInt`

👉 If conversion fails → error ❌

### 🔥 `$convert` vs `$toInt` vs `$toString`

| Feature        | `$convert`⚙️ | `$toInt`🔢 | `$toString`🔤  |                  |
| -------------- | ------------------------------- | ---------------- | ---------------- |
| Flexibility    | High                            | Low              | Low              |
| Error handling | ✅ Yes                          | ❌ No            | ❌ No            |
| Types          | Any                             | Only int         | Only string      |
| Use case       | Production-safe                 | Quick conversion | Quick conversion |

### 🔥 Real-world Example (IMPORTANT)

```js
db.orders.aggregate([
  {
    $addFields: {
      total: {
        $convert: {
          input: "$total",
          to: "double",
          onError: 0
        }
      },
      orderIdStr: { $toString: "$_id" }
    }
  }
])
```

### 🧠 When to use what?

✅ Use `$convert` when:

* Data may be dirty ⚠️
* You need fallback values
* Production pipelines

✅ Use `$toInt` / `$toString` when:

* You’re sure data is clean
* Quick/simple conversion

---

# ----Aggregations- Date operations in detail

MongoDB **date operators** are a big topic, but once you organize them properly, everything becomes easy 🔥

I’ll break them into  **clear categories + real examples + intuition** .

### 🧠 Big Picture

Date operators are **aggregation expression operators** used to:

* Extract parts of a date 📅
* Format dates 🧾
* Do calculations ⏱️
* Convert timezones 🌍

### 🔥 1. Date Extraction Operators 📅

👉 Extract parts from a date

##### 🔹 `$year`, `$month`, `$dayOfMonth`

```js
db.users.aggregate([
  {
    $project: {
      year: { $year: "$createdAt" },
      month: { $month: "$createdAt" },
      day: { $dayOfMonth: "$createdAt" }
    }
  }
])
```

🧠 Input:

```js
{ createdAt: ISODate("2024-03-15T10:30:00Z") }
```

Output:

```js
{
  year: 2024,
  month: 3,
  day: 15
}
```

##### 🔹 More extractors

* `$dayOfWeek` → 1 (Sunday) to 7
* `$dayOfYear` → 1–365
* `$week` → week number
* `$isoWeek`, `$isoWeekYear`
* `$hour`, `$minute`, `$second`, `$millisecond`

##### 🔥 Example

```js
{
  $project: {
    hour: { $hour: "$createdAt" },
    minute: { $minute: "$createdAt" }
  }
}
```

### 🔥 2. Date Formatting 🧾

## 🔹 `$dateToString`

👉 Convert date → formatted string

```js
{
  $project: {
    formatted: {
      $dateToString: {
        format: "%Y-%m-%d",
        date: "$createdAt"
      }
    }
  }
}
```

🧠 Output:

```js
"2024-03-15"
```

**🔹 Common format specifiers**

| Code   | Meaning |
| ------ | ------- |
| `%Y` | Year    |
| `%m` | Month   |
| `%d` | Day     |
| `%H` | Hour    |
| `%M` | Minute  |

### 🔥 3. Date Creation 🏗️

##### 🔹 `$dateFromParts`

```js
{
  $dateFromParts: {
    year: 2024,
    month: 3,
    day: 15
  }
}
```

##### 🔹 `$dateFromString`

```js
{
  $dateFromString: {
    dateString: "2024-03-15"
  }
}
```

### 🔥 4. Date Arithmetic ⏱️

##### 🔹 `$dateAdd`

👉 Add time

```js
{
  $dateAdd: {
    startDate: "$createdAt",
    unit: "day",
    amount: 5
  }
}
```

Output:

👉 Adds 5 days

##### 🔹 `$dateSubtract`

```js
{
  $dateSubtract: {
    startDate: "$createdAt",
    unit: "month",
    amount: 1
  }
}
```

##### 🔹 `$dateDiff`

👉 Difference between two dates

```js
{
  $dateDiff: {
    startDate: "$start",
    endDate: "$end",
    unit: "day"
  }
}
```

🧠 Output:

```js
10
```

### 🔥 5. Current Date ⏳

## 🔹 `$$NOW`

```js
{
  $project: {
    now: "$$NOW"
  }
}
```

👉 Current system date

### 🔥 6. Date Truncation 📉

##### 🔹 `$dateTrunc`

👉 Round down date

```js
{
  $dateTrunc: {
    date: "$createdAt",
    unit: "month"
  }
}
```

Output:

```js
2024-03-01T00:00:00Z
```

### 🔥 7. Timezone Handling 🌍

##### 🔹 `$dateToString` with timezone

```js
{
  $dateToString: {
    date: "$createdAt",
    timezone: "Asia/Kolkata",
    format: "%Y-%m-%d %H:%M"
  }
}
```

##### 🔹 `$hour` with timezone

```js
{
  $hour: {
    date: "$createdAt",
    timezone: "Asia/Kolkata"
  }
}
```

### 🔥 8. ISO Date Operators

* `$isoWeek`
* `$isoWeekYear`
* `$isoDayOfWeek`

👉 Used for ISO standard calendars

### 🔥 9. Advanced Operators

##### 🔹 `$dateToParts`

```js
{
  $dateToParts: {
    date: "$createdAt"
  }
}
```

Output:

```js
{
  year: 2024,
  month: 3,
  day: 15,
  hour: 10
}
```

##### 🔹 `$dateFromParts` (reverse of above)

### 🔥 Real-world Example 🚀

```js
db.orders.aggregate([
  {
    $project: {
      orderDate: {
        $dateToString: {
          format: "%Y-%m-%d",
          date: "$createdAt"
        }
      },
      deliveryDate: {
        $dateAdd: {
          startDate: "$createdAt",
          unit: "day",
          amount: 7
        }
      },
      orderAge: {
        $dateDiff: {
          startDate: "$createdAt",
          endDate: "$$NOW",
          unit: "day"
        }
      }
    }
  }
])
```

### 🧠 Final Structure

**📅 Extraction**

* `$year`, `$month`, `$dayOfMonth`, `$hour`, etc.

**🧾 Formatting**

* `$dateToString`

**🏗️ Creation**

* `$dateFromParts`, `$dateFromString`

**⏱️ Arithmetic**

* `$dateAdd`, `$dateSubtract`, `$dateDiff`

**📉 Truncation**

* `$dateTrunc`

**🌍 Timezone**

* Supported in many operators

---

# ----System variables

**MongoDB system variables** are a core concept in aggregation, especially once you start using `$lookup`, `$map`, `$let`, etc. 🔥

Let’s break them down  **clearly + deeply + with examples** .

### 🧠 What are System Variables?

👉 **Predefined variables provided by MongoDB**
👉 Always start with `$$`

**🔥 Syntax**

```js
$$VARIABLE_NAME
```

**🧠 Key idea**

> “They give access to special values like current document, root document, time, etc.”

**🔥 Main System Variables**

### 🔥 1. `$$ROOT` 🌳

**👉 What it is**

 Represents the **entire original document at current stage**

🔹 Example

```js
db.users.aggregate([
  {
    $project: {
      fullDoc: "$$ROOT"
    }
  }
])
```

🧠 Output

```js
{
  fullDoc: {
    name: "Arjun",
    age: 22
  }
}
```

### 🔥 2. `$$CURRENT` 🧵

👉 What it is

Represents the **current working document**

**🔹 Difference from `$$ROOT`**

| Variable      | Meaning                           |
| ------------- | --------------------------------- |
| `$$ROOT`    | Original doc                      |
| `$$CURRENT` | Current doc after transformations |

🔹 Example

```js
db.users.aggregate([
  {
    $addFields: {
      copy: "$$CURRENT"
    }
  }
])
```

### 🔥 3. `$$NOW` ⏳

👉 What it is

Current date/time

🔹 Example

```js
{
  $project: {
    currentTime: "$$NOW"
  }
}
```

👉 Same for all documents in pipeline

### 🔥 4. `$$REMOVE` ❌

**👉 What it is**

Used to **remove a field conditionally**

🔹 Example

```js
db.users.aggregate([
  {
    $project: {
      age: {
        $cond: {
          if: { $lt: ["$age", 18] },
          then: "$$REMOVE",
          else: "$age"
        }
      }
    }
  }
])
```

👉 Removes `age` if less than 18

### 🔥 5. `$$DESCEND`, `$$PRUNE`, `$$KEEP` 🔐

👉 Used inside `$redact`

🔹 Meaning

| Variable      | Meaning         |
| ------------- | --------------- |
| `$$KEEP`    | Keep document   |
| `$$PRUNE`   | Remove document |
| `$$DESCEND` | Go deeper       |

🔹 Example

```js
{
  $redact: {
    $cond: {
      if: { $gte: ["$level", 3] },
      then: "$$DESCEND",
      else: "$$PRUNE"
    }
  }
}
```

### 🔥 6. `$$this` 🔁

**👉 What it is**

Refers to **current element in array iteration**

🔹 Used in:

* `$map`
* `$filter`
* `$reduce`

🔹 Example (`$map`)

```js
{
  $project: {
    doubled: {
      $map: {
        input: "$numbers",
        as: "num",
        in: { $multiply: ["$$num", 2] }
      }
    }
  }
}
```

👉 `$$num` is alias of `$$this`

### 🔥 7. `$$value` 🔄

👉 What it is

 Used in `$reduce`
 Stores accumulated value

🔹 Example

```js
{
  $project: {
    sum: {
      $reduce: {
        input: "$numbers",
        initialValue: 0,
        in: {
          $add: ["$$value", "$$this"]
        }
      }
    }
  }
}
```

👉 `$$value` = running total

### 🔥 8. `$$ROOT` in `$group` (VERY IMPORTANT)

```js
{
  $group: {
    _id: "$category",
    docs: { $push: "$$ROOT" }
  }
}
```

👉 Collects full documents inside array

### 🔥 9. Variables in `$lookup` (`$$var`)

```js
{
  $lookup: {
    from: "users",
    let: { userId: "$userId" },
    pipeline: [
      {
        $match: {
          $expr: {
            $eq: ["$_id", "$$userId"]
          }
        }
      }
    ],
    as: "user"
  }
}
```

👉 `$$userId` is a **custom variable**

### 🔥 10. Variables in `$let` 🧠

```js
{
  $project: {
    result: {
      $let: {
        vars: { x: "$price" },
        in: { $multiply: ["$$x", 2] }
      }
    }
  }
}
```

👉 Define your own variables

### 🧠 Summary Table

| Variable      | Purpose                    |
| ------------- | -------------------------- |
| `$$ROOT`    | Full original document     |
| `$$CURRENT` | Current document           |
| `$$NOW`     | Current time               |
| `$$REMOVE`  | Remove field               |
| `$$KEEP`    | Keep doc                   |
| `$$PRUNE`   | Remove doc                 |
| `$$DESCEND` | Traverse deeper            |
| `$$this`    | Current array element      |
| `$$value`   | Accumulator in `$reduce` |

### 🔥 Key Differences (IMPORTANT)

🔹 `$$ROOT` vs `$$CURRENT`

| Case               | Result    |
| ------------------ | --------- |
| Before stages      | Same      |
| After `$project` | Different |

---

# Array operator- `$push and $pushAll`

**`$push` is real and widely used; `$pushAll` is deprecated/removed** . If you’re seeing `$pushAll`, it’s old MongoDB (pre-2.6 era) or outdated material.

Let’s go deep so it’s crystal clear 🔥

### 🔥 1. `$push` 📥

##### What it is

👉 An **update operator** (also an  **accumulator in aggregation** )
👉 Adds value(s) to an **array field**

**🧠 Intuition**

> “Append item(s) to an array”

##### 🔹 A) `$push` in **update operations**

Basic syntax

```js
db.users.updateOne(
  { _id: 1 },
  { $push: { hobbies: "cricket" } }
)
```

🧠 Before

```js
{ _id: 1, hobbies: ["reading"] }
```

After

```js
{ _id: 1, hobbies: ["reading", "cricket"] }
```

**🔥 Add multiple items → `$each`**

```js
db.users.updateOne(
  { _id: 1 },
  {
    $push: {
      hobbies: {
        $each: ["music", "travel"]
      }
    }
  }
)
```

##### 🔥 Advanced modifiers (VERY IMPORTANT)

###### 🔹 1. `$slice` → limit array size

```js
{
  $push: {
    scores: {
      $each: [90, 85],
      $slice: -5
    }
  }
}
```

👉 Keeps only last 5 elements

###### 🔹 2. `$sort` → sort array

```js
{
  $push: {
    scores: {
      $each: [70, 95],
      $sort: -1
    }
  }
}
```

👉 Sorts descending

###### 🔹 3. `$position` → insert at index

```js
{
  $push: {
    scores: {
      $each: [100],
      $position: 0
    }
  }
}
```

👉 Inserts at beginning

🧠 Combine all (powerful)

```js
{
  $push: {
    scores: {
      $each: [88, 92],
      $position: 0,
      $sort: -1,
      $slice: 5
    }
  }
}
```

##### 🔹 B) `$push` in **aggregation (`$group`)**

👉 Works as an **accumulator**

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$category",
      prices: { $push: "$price" }
    }
  }
])
```

🧠 Output

```js
{
  _id: "electronics",
  prices: [1000, 2000, 3000]
}
```

👉 Collects values into an array

##### 🔥 `$push` vs `$addToSet`

| Feature    | `$push`📥 | `$addToSet`📦 |                |
| ---------- | ----------------------------- | -------------- |
| Duplicates | ✅ Allowed                    | ❌ Removed     |
| Order      | Preserved                     | Not guaranteed |

### ❌ 2. `$pushAll` (DEPRECATED)

##### What it was

👉 Old operator used to push multiple values:

```js
db.users.updateOne(
  { _id: 1 },
  { $pushAll: { hobbies: ["music", "travel"] } }
)
```

##### ❌ Why removed?

👉 Redundant
👉 Replaced by `$push + $each`

##### ✅ Modern equivalent

```js
{
  $push: {
    hobbies: {
      $each: ["music", "travel"]
    }
  }
}
```

### 🔥 Real-world Example 🚀

```js
db.posts.updateOne(
  { _id: 1 },
  {
    $push: {
      comments: {
        $each: [
          { user: "A", text: "Nice!" },
          { user: "B", text: "Great!" }
        ],
        $slice: -10
      }
    }
  }
)
```

👉 Keeps only last 10 comments

---

# Array Operator - `$pull and $pullAll`

This is the **mirror image of `$push`** 🔥

If `$push` adds to arrays,  **`$pull` removes from arrays** .

Let’s go deep and precise.

### 🔥 1. `$pull` 🧲

##### What it is

👉 **Update operator**
👉 Removes elements from an array **based on a condition**

**🧠 Intuition**

> “Remove all array elements that match this condition”

**🔹 Basic Syntax**

```js
{
  $pull: {
    field: <value OR condition>
  }
}
```

##### 🔥 Example 1: Remove a specific value

```js
db.users.updateOne(
  { _id: 1 },
  { $pull: { hobbies: "cricket" } }
)
```

🧠 Before

```js
{ hobbies: ["reading", "cricket", "music"] }
```

After

```js
{ hobbies: ["reading", "music"] }
```

##### 🔥 Example 2: Remove multiple occurrences

```js
{ hobbies: ["cricket", "music", "cricket"] }
```

```js
$pull: { hobbies: "cricket" }
```

👉 Result:

```js
["music"]
```

👉 Removes  **ALL matches** , not just one

##### 🔥 Example 3: Using condition (VERY IMPORTANT)

```js
db.users.updateOne(
  { _id: 1 },
  {
    $pull: {
      scores: { $lt: 50 }
    }
  }
)
```

🧠 Before

```js
{ scores: [30, 60, 40, 80] }
```

After

```js
{ scores: [60, 80] }
```

👉 Removes elements **matching condition**

##### 🔥 Example 4: Remove objects from array

```js
db.orders.updateOne(
  { _id: 1 },
  {
    $pull: {
      items: { price: { $lt: 100 } }
    }
  }
)
```

🧠 Before

```js
items: [
  { name: "A", price: 50 },
  { name: "B", price: 200 }
]
```

After

```js
items: [
  { name: "B", price: 200 }
]
```

##### 🔥 Example 5: Exact object match

```js
$pull: {
  items: { name: "A", price: 50 }
}
```

👉 Removes only exact matches

##### 🧠 Key Behavior

* Works on **arrays only**
* Removes **ALL matching elements**
* Supports **query operators (`$gt`, `$lt`, etc.)**

### 🔥 2. `$pullAll` 🧹

##### What it is

👉 Removes **multiple exact values** from an array

**🔹 Syntax**

```js
{
  $pullAll: {
    field: [value1, value2]
  }
}
```

**🔥 Example**

```js
db.users.updateOne(
  { _id: 1 },
  {
    $pullAll: {
      hobbies: ["cricket", "music"]
    }
  }
)
```

🧠 Before

```js
["reading", "cricket", "music", "travel"]
```

After

```js
["reading", "travel"]
```

##### ⚠️ Important Limitation

👉 `$pullAll`:

* ❌ No conditions
* ❌ No operators (`$gt`, `$lt`)
* ✅ Only exact values

### 🔥 `$pull` vs `$pullAll`

| Feature        | `$pull`🧲   | `$pullAll`🧹 |                   |
| :------------- | ------------------------------ | ----------------- |
| Remove logic   | Condition-based                | Exact values only |
| Flexibility    | High                           | Low               |
| Supports query | ✅ Yes                         | ❌ No             |
| Removes many   | Yes                            | Yes               |

### 🔥 Why `$pull` is preferred

👉 Because you can do everything `$pullAll` does — and more:

**Equivalent of `$pullAll` using `$pull`**

```js
{
  $pull: {
    hobbies: { $in: ["cricket", "music"] }
  }
}
```

👉 More flexible + modern approach ✅

### 🔥 Real-world Example 🚀

```js
db.posts.updateOne(
  { _id: 1 },
  {
    $pull: {
      comments: {
        flagged: true
      }
    }
  }
)
```

👉 Removes all flagged comments


---

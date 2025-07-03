# Aggregation Pipeline Operator: `$hour`, `$group and 2 $project in one aggregation`  with Example--Project[Fitlab]

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
> SYNTAX ----- `{ $hour: `<dateExpression>``````` }
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

# Aggregation- `$unwind- $group- $project` with Example --Project[Fitlab]

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

# Aggregation- `$facet- $count- $project- $addFields- $cond- $round- $multiply` with Example --Project[Fitlab]

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

# Aggregation- `date, $match- $cond- if then else $project- $group- $count `with Example --Project[Fitlab]

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

# Aggregation- `date, $unwind- $sort- $limit- $group- $project `with Example --Project[Fitlab]

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

# Aggregation- `$lookup, $limit- $sort- $unwind $group $project- $match- $count `with Example --Project[Fitlab]

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

# .lean() in Mongoose

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

# Aggregation- `$switch, branches- case- then- default- $unwind- $push- k v -$addFields- $group- $project-  `with Example --Project[Fitlab]

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



# Aggregation- `countDocuments, $lookup- $unwind -$group -$project -$match `with Example --Project[Fitlab]

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



# Aggregation- `$sum, $add- $ifNull -$sort -$nin -$match `with Example --Project[Fitlab]

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


# Aggregation- `$sum, $add- $ifNull -$sort -$nin -$match `with Example --Project[Fitlab]

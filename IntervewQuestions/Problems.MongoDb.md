# --1. Find all users whose age is greater than 20

```javascript
db.users.find({age: {$gt: 20}}})
```


# --2. Find all users who live in `"Kochi"` AND whose age is greater than 20.

```
db.users.find({place: "Kochi", age: {$gt: 20}})
```



# --3. Find all users who either live in `"Delhi"` OR are older than 40.

```javascript
db.users.find({$or: [{place: "Delhi"}, {age: {$gt: 40}}]})
```



# --4. Return only the fields `name` and `email` from the `users` collection. Exclude `_id`.

```javascript
db.users.find({}, {name: 1, email: 1, _id: 0})
```



# --5. Find all products sorted by `price` in descending order.

```javascript
db.products.find().sort({price: -1})
```



# --6. Fetch only the first 5 users from the collection.

```
db.users.find().limit(5)
```

# --7. Update the city of the user whose name is `"Arjun"` to `"Bangalore"`.

```
db.users.updateOne({name: "Arjun"}, {$set: {place: "Bangalore"}}})
```

# --8. Increase the salary of employee `"Rahul"` by 5000.

```javascript
db.employees.updateOne(
	{name: "Rahul"}, 
	{$inc: {salary: 5000}}
)
```

# --9. Delete all users whose age is below 18.

```
db.users.deleteMany({age: {$lt: 18}})
```

# --10. Find all users whose age is between 25 and 35 (inclusive).

```
db.users.find({ $and: [{age: {$gte: 25}}, {age: {$lte: 35}}] })
    OR
db.users.find({
   age: { $gte: 25, $lte: 35 }
})
```

# --11. Find all users who have `"React"` in their `skills` array.

```js
db.users.find({
   skills: {
      $elemMatch: "React"
   }
})
```

is invalid because:

```txt
$elemMatch expects an OBJECT containing conditions
```

NOT a direct value.

For simple arrays like:

```js
{
   skills: ["React", "Node.js"]
}
```

the correct query is simply:

```js
db.users.find({
   skills: "React"
})
```

MongoDB automatically checks array elements 👍

Correct `$elemMatch` usage is for:

* multiple conditions
* arrays of objects
* complex matching

Example 👇

```js
{
   scores: [
      { subject: "Math", marks: 90 },
      { subject: "Science", marks: 80 }
   ]
}
```

Find users having:

```txt
subject = "Math"
AND
marks > 85
```

```js
db.users.find({
   scores: {
      $elemMatch: {
         subject: "Math",
         marks: { $gt: 85 }
      }
   }
})
```

That’s the proper purpose of `$elemMatch` 🚀

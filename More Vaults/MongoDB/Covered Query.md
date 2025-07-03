A covered query in MongoDB is a query where all the fields in the query are covered by an index, meaning that the necessary data can be retrieved from the index itself without the need to access the actual documents in the collection. This type of query is considered highly efficient because it minimizes the amount of data that needs to be read from the storage.

To understand covered queries in more detail, let's break down the key concepts:

1. **Index Coverage:**
   - For a query to be considered covered, the fields involved in the query must be part of an index.
   - If all the fields in the query are part of a single index, and that index can fulfill the query requirements, then the query is covered.

2. **Fields in Query and Projection:**
   - A covered query includes fields in both the query filter and the projection (fields to be returned).
   - All fields involved in the query (filter conditions) and projection (fields to be returned) must be part of the index.

3. **Benefits of Covered Queries:**
   - Improved Performance: Covered queries can significantly improve query performance by avoiding the need to access the actual documents in the collection. Instead, MongoDB can satisfy the query using the index alone.
   - Reduced Disk I/O: Since the required data is available in the index, there is less need for disk I/O operations, leading to faster query execution.

4. **Example of Covered Query:**
   - Suppose you have a collection named `users` with documents like this:
     ```javascript
     {
       _id: ObjectId("5fbeb4f7a9b39b5f7b88f8d6"),
       username: "john_doe",
       email: "john@example.com",
       age: 30
     }
     ```
   - You have an index on the `username` field: `db.users.createIndex({ username: 1 })`.
   - A covered query might look like this:
     ```javascript
     db.users.find({ username: "john_doe" }, { _id: 0, username: 1 });
     ```
   - In this example, the query is covered because the `username` field is in both the query filter and the projection, and it is part of the index.

5. **Limitations:**
   - Covered queries are beneficial for read operations, but write operations may incur additional costs, especially if the index size is large.
   - The choice of which fields to include in the index should consider the balance between read and write performance.

6. **Monitoring Covered Queries:**
   - MongoDB's query planner can provide information on whether a query is covered or not. You can use the `explain()` method to get query execution details.

Here's an example using the `explain()` method:

```javascript
db.users.find({ username: "john_doe" }, { _id: 0, username: 1 }).explain("executionStats");
```

The output will include information about whether the query is covered (`"totalDocsExamined"` will be less than the total number of documents in the collection).

In summary, covered queries in MongoDB leverage indexes to fulfill query requirements without accessing the actual documents, resulting in improved query performance and reduced disk I/O.
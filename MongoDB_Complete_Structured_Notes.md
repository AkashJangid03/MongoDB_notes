# MongoDB — Complete Structured Notes

> **Note:** These notes are structured for study and revision. They cover MongoDB fundamentals, NoSQL, data modelling, database/collection operations, CRUD, operators, projection, limit/skip, aggregation, sorting, indexing, and additional MongoDB concepts.

---

# 1. Introduction to MongoDB

## What is MongoDB?

MongoDB is a **NoSQL, document-oriented database management system**.

Unlike relational databases such as MySQL and Oracle, MongoDB stores data in **documents** instead of rows and columns.

Example:

```javascript
{
    name: "Akash",
    age: 22,
    course: "MSc CA"
}
```

MongoDB internally stores documents using **BSON (Binary JSON)**.

### MongoDB is useful for

- Flexible data structures
- Large-scale applications
- High availability
- Horizontal scalability
- Fast read/write operations
- Structured and semi-structured data

---

# 2. What is NoSQL?

**NoSQL = Not Only SQL / Non-relational database.**

NoSQL databases are designed to handle data that may not naturally fit into traditional relational tables.

They are useful when data is:

- Large
- Distributed
- Frequently changing
- Semi-structured
- Unstructured
- Hierarchical

---

# 3. Types of NoSQL Databases

| Type | Description | Example |
|---|---|---|
| Document Database | Stores data as documents | MongoDB |
| Key-Value Database | Stores data as key-value pairs | Redis |
| Graph Database | Stores relationships between entities | Neo4j |
| Wide-Column Database | Stores data in column-oriented structures | Cassandra, HBase |

MongoDB is a **Document-Oriented Database**.

---

# 4. Structured vs Unstructured Data

## Structured Data

Data having a predefined and organized format.

Example:

| ID | Name | Age |
|---:|---|---:|
| 1 | Akash | 22 |
| 2 | Rahul | 23 |

Usually stored in relational tables.

## Unstructured Data

Data that does not follow a fixed tabular structure.

Examples:

- Images
- Videos
- Documents
- Audio
- Social media content

Modern applications can contain both structured and unstructured data.

---

# 5. SQL vs NoSQL

| Feature | SQL | NoSQL |
|---|---|---|
| Database type | Relational | Non-relational |
| Schema | Fixed/predefined | Flexible/dynamic |
| Data structure | Tables | Documents/key-value/graphs/columns |
| Basic unit | Row | Document |
| Scaling | Mainly vertical | Mainly horizontal |
| Query language | SQL | Database-specific APIs |
| Relationships | Joins | Embedding/references |
| Best suited for | Structured relational data | Flexible/distributed data |
| Examples | MySQL, Oracle, PostgreSQL | MongoDB, Redis, Cassandra |

## Vertical Scaling

Increasing the power of a single server.

```text
More CPU
More RAM
More Storage
```

## Horizontal Scaling

Adding more servers.

```text
Server 1
Server 2
Server 3
Server 4
```

MongoDB supports horizontal scaling through techniques such as **sharding**.

---

# 6. Advantages of MongoDB

Major advantages:

1. Document-oriented storage
2. Flexible schema
3. High availability
4. Replication
5. Horizontal scalability
6. Auto-sharding
7. Ad-hoc queries
8. Indexing
9. Fast updates
10. Integration with programming languages
11. Handles structured and unstructured data
12. Suitable for rapidly changing applications

---

# 7. MongoDB Terminology

| RDBMS | MongoDB |
|---|---|
| Database | Database |
| Table | Collection |
| Row/Tuple | Document |
| Column | Field |
| Primary Key | `_id` |
| Join | Embedded documents / references |

### MongoDB Hierarchy

```text
MongoDB Server
      ↓
   Database
      ↓
  Collection
      ↓
   Document
      ↓
    Field
```

Example:

```text
Database: college
        ↓
Collection: students
        ↓
Document:
{
    name: "Akash",
    age: 22,
    course: "MSc CA"
}
```

---

# 8. Document

A **document** is a set of key-value pairs.

Example:

```javascript
{
    name: "Akash",
    age: 22,
    city: "Pune"
}
```

Here:

```text
name  → "Akash"
age   → 22
city  → "Pune"
```

MongoDB documents can contain:

- Primitive values
- Arrays
- Objects
- Nested documents
- Arrays of documents

---

# 9. Collection

A **collection** is a group of MongoDB documents.

It is approximately equivalent to a **table in RDBMS**.

Example:

```text
Database: college

Collection: students

Documents:
{ name: "Akash", age: 22 }
{ name: "Rahul", age: 23 }
{ name: "Kunal", age: 21 }
```

MongoDB does not require every document in a collection to have exactly the same fields.

---

# 10. Flexible / Dynamic Schema

In SQL, the structure is generally predefined.

Example:

```text
Student
-------------------
id INT
name VARCHAR
age INT
```

In MongoDB:

```javascript
{
    name: "Akash",
    age: 22
}
```

Another document can contain additional fields:

```javascript
{
    name: "Rahul",
    age: 23,
    city: "Pune"
}
```

Both documents can exist in the same collection.

This is called a **flexible/dynamic schema**.

---

# 11. BSON

**BSON = Binary JSON**

MongoDB uses BSON internally to represent documents.

Example:

```json
{
    "name": "Akash",
    "age": 22
}
```

BSON provides additional data types beyond basic JSON.

Examples:

- Date
- ObjectId
- Binary data
- Integer types
- Decimal types

---

# 12. MongoDB Data Types

| Data Type | Example |
|---|---|
| String | `"Akash"` |
| Integer | `22` |
| Double | `3.14` |
| Boolean | `true` |
| Array | `["Java", "Python"]` |
| Object | `{city: "Pune"}` |
| Date | `ISODate(...)` |
| Null | `null` |
| ObjectId | `ObjectId(...)` |
| Regular Expression | `/pattern/` |
| Binary Data | Binary values |
| Timestamp | MongoDB timestamp |

---

# 13. `_id` Field

Every MongoDB document normally has a unique `_id` field.

Example:

```javascript
{
    _id: ObjectId("..."),
    name: "Akash",
    age: 22
}
```

If `_id` is not provided, MongoDB automatically generates one.

The `_id` field uniquely identifies a document.

---

# 14. Data Modelling

Data modelling means deciding how documents and relationships should be structured.

MongoDB provides two major approaches:

1. **Embedded Data Model**
2. **Normalized / Referenced Data Model**

---

# 15. Embedded Data Model

In the embedded model, related data is stored inside the same document.

Example:

```javascript
{
    empId: "E101",

    personalDetails: {
        firstName: "Radhika",
        lastName: "Sharma"
    },

    contact: {
        email: "radhika@gmail.com",
        phone: "9876543210"
    },

    address: {
        city: "Pune",
        state: "Maharashtra"
    }
}
```

## Advantages

- Fewer queries
- No need for joins
- Related data is available together
- Good read performance

## Suitable when

Data is:

- Frequently accessed together
- Relatively small
- Closely related

---

# 16. Normalized / Referenced Data Model

Related information is stored in separate collections and connected using references.

### Employee

```javascript
{
    _id: ObjectId("101"),
    empId: "E101"
}
```

### PersonalDetails

```javascript
{
    _id: ObjectId("102"),
    empDocId: ObjectId("101"),
    firstName: "Radhika"
}
```

### Address

```javascript
{
    _id: ObjectId("103"),
    empDocId: ObjectId("101"),
    city: "Pune"
}
```

---

# 17. Embedded vs Normalized

| Embedded | Normalized / Referenced |
|---|---|
| Related data in one document | Data in separate documents |
| Fewer queries | More queries may be required |
| Faster reads for related data | Useful for independently accessed data |
| Possible data duplication | Less duplication |
| Good for closely related data | Good for complex/shared relationships |

### Basic Rule

> If data is usually accessed together → **Embed it.**

> If data is large, shared or independently accessed → **Use references.**

---

# 18. Example of MongoDB Data Modelling

A blog post can contain:

```javascript
{
    _id: 101,

    title: "MongoDB Basics",

    description: "Introduction to MongoDB",

    by: "Akash",

    url: "example.com",

    tags: [
        "MongoDB",
        "Database",
        "NoSQL"
    ],

    likes: 100,

    comments: [
        {
            user: "Rahul",
            message: "Good article",
            dateCreated: "...",
            likes: 10
        }
    ]
}
```

Related information such as tags and comments can be embedded inside the document.

---

# 19. Creating / Selecting a Database

MongoDB does not require a traditional SQL-style `CREATE DATABASE` command.

Use:

```javascript
use college
```

If the database exists, MongoDB switches to it.

If it does not exist, MongoDB uses that name and the database becomes persistent after data is stored.

## Check Current Database

```javascript
db
```

## Show Databases

```javascript
show dbs
```

> An empty database may not appear in `show dbs` until data is stored in it.

---

# 20. Dropping a Database

To delete the current database:

```javascript
db.dropDatabase()
```

Example:

```javascript
use college

db.dropDatabase()
```

This permanently removes the database and its collections.

---

# 21. Creating a Collection

Explicit creation:

```javascript
db.createCollection("students")
```

General syntax:

```javascript
db.createCollection(
    "collection_name",
    options
)
```

MongoDB can also create a collection automatically when data is inserted.

Example:

```javascript
db.students.insertOne({
    name: "Akash",
    age: 22
})
```

---

# 22. Capped Collection

A **capped collection** has a fixed maximum size.

When the limit is reached, older documents can be overwritten by newer ones.

Example:

```javascript
db.createCollection("logs", {
    capped: true,
    size: 100000
})
```

Important options:

| Option | Meaning |
|---|---|
| `capped` | Enables capped collection |
| `size` | Maximum size in bytes |
| `max` | Maximum number of documents |

---

# 23. Dropping a Collection

```javascript
db.students.drop()
```

This removes the collection and its associated indexes.

---

# 24. CRUD Operations

CRUD stands for:

| Operation | Meaning |
|---|---|
| **C** | Create |
| **R** | Read |
| **U** | Update |
| **D** | Delete |

---

# 25. CREATE — Insert Documents

## Insert One

```javascript
db.students.insertOne({
    name: "Akash",
    age: 22,
    course: "MSc CA"
})
```

## Insert Many

```javascript
db.students.insertMany([
    {
        name: "Akash",
        age: 22
    },
    {
        name: "Rahul",
        age: 23
    }
])
```

---

# 26. Embedded Documents

MongoDB allows documents to contain:

- Objects
- Arrays
- Arrays of objects

Example:

```javascript
{
    name: "Stan Lee",

    comics: [
        {
            name: "Superman",
            year: 2004
        },
        {
            name: "Batman",
            year: 2001
        }
    ]
}
```

---

# 27. READ — Find Documents

## Find All Documents

```javascript
db.students.find()
```

## Find One Document

```javascript
db.students.findOne()
```

## Find Using Condition

```javascript
db.students.find({
    age: 22
})
```

Example:

```javascript
db.students.find({
    course: "MSc CA"
})
```

---

# 28. `pretty()`

Older MongoDB shell examples may use:

```javascript
db.students.find().pretty()
```

It formats the output in a more readable way.

---

# 29. UPDATE Documents

## Update One

```javascript
db.students.updateOne(
    { name: "Akash" },
    { $set: { age: 23 } }
)
```

## Update Many

```javascript
db.students.updateMany(
    { course: "MSc CA" },
    { $set: { university: "Fergusson College" } }
)
```

---

# 30. Important Update Operators

## `$set`

Sets or changes a field.

```javascript
{
    $set: {
        age: 23
    }
}
```

## `$unset`

Removes a field.

```javascript
{
    $unset: {
        age: ""
    }
}
```

## `$inc`

Increments a numeric value.

```javascript
{
    $inc: {
        age: 1
    }
}
```

## `$push`

Adds an element to an array.

```javascript
{
    $push: {
        skills: "MongoDB"
    }
}
```

## `$pull`

Removes matching elements from an array.

```javascript
{
    $pull: {
        skills: "C"
    }
}
```

---

# 31. DELETE Documents

## `deleteOne()`

Deletes only one matching document.

```javascript
db.students.deleteOne({
    name: "Akash"
})
```

Even if multiple documents match, only one is deleted.

## `deleteMany()`

Deletes all matching documents.

```javascript
db.students.deleteMany({
    age: 22
})
```

## Delete All Documents

```javascript
db.students.deleteMany({})
```

> Be careful: this removes all documents from the collection.

---

# 32. `remove()` — Older Syntax

Older MongoDB syntax uses:

```javascript
db.student.remove({
    regNo: "3014"
})
```

or:

```javascript
db.programmers.remove({})
```

For modern MongoDB, prefer:

```text
deleteOne()
deleteMany()
```

---

# 33. Comparison Operators

| Operator | Meaning |
|---|---|
| `$eq` | Equal |
| `$ne` | Not equal |
| `$gt` | Greater than |
| `$gte` | Greater than or equal |
| `$lt` | Less than |
| `$lte` | Less than or equal |
| `$in` | Matches any value in an array |
| `$nin` | Does not match values in an array |

## Examples

### Greater Than

```javascript
db.students.find({
    age: { $gt: 20 }
})
```

### Greater Than or Equal

```javascript
db.students.find({
    age: { $gte: 20 }
})
```

### Less Than

```javascript
db.students.find({
    age: { $lt: 25 }
})
```

### `$in`

```javascript
db.students.find({
    city: {
        $in: ["Pune", "Mumbai"]
    }
})
```

---

# 34. Logical Operators

| Operator | Meaning |
|---|---|
| `$and` | All conditions must be true |
| `$or` | At least one condition must be true |
| `$nor` | None of the conditions should be true |
| `$not` | Negates a condition |

## `$and`

```javascript
db.students.find({
    $and: [
        { age: { $gt: 20 } },
        { course: "MSc CA" }
    ]
})
```

## `$or`

```javascript
db.students.find({
    $or: [
        { city: "Pune" },
        { city: "Mumbai" }
    ]
})
```

---

# 35. Array Operators

| Operator | Purpose |
|---|---|
| `$all` | Matches arrays containing all specified values |
| `$size` | Matches arrays with specified number of elements |
| `$elemMatch` | Matches array elements satisfying multiple conditions |

Example:

```javascript
db.students.find({
    skills: {
        $all: ["Java", "Python"]
    }
})
```

---

# 36. Projection

**Projection** means selecting which fields should appear in the result.

Syntax:

```javascript
db.collection.find(
    { condition },
    { field1: 1, field2: 1 }
)
```

Example:

```javascript
db.students.find(
    {},
    {
        name: 1,
        age: 1
    }
)
```

### `1` = Include

### `0` = Exclude

Example:

```javascript
db.students.find(
    {},
    {
        _id: 0,
        name: 1,
        age: 1
    }
)
```

This displays only `name` and `age`.

---

# 37. `limit()`

`limit()` restricts the number of documents returned.

```javascript
db.students.find().limit(5)
```

Meaning:

> Return a maximum of 5 documents.

---

# 38. `skip()`

`skip()` skips a specified number of documents.

```javascript
db.students.find().skip(5)
```

Meaning:

> Skip the first 5 documents.

Can be combined with `limit()`:

```javascript
db.students.find()
    .skip(5)
    .limit(10)
```

Meaning:

> Skip the first 5 documents and return the next 10.

---

# 39. Sorting

MongoDB uses:

```javascript
sort()
```

## Ascending

```javascript
db.students.find().sort({
    age: 1
})
```

## Descending

```javascript
db.students.find().sort({
    age: -1
})
```

| Value | Meaning |
|---:|---|
| `1` | Ascending |
| `-1` | Descending |

---

# 40. Combining Query + Projection + Sort + Limit

Example:

```javascript
db.students.find(
    { age: { $gt: 20 } },
    { _id: 0, name: 1, age: 1 }
)
.sort({ age: -1 })
.limit(5)
```

This performs:

1. Find students older than 20
2. Display only `name` and `age`
3. Sort by age descending
4. Return maximum 5 documents

---

# 41. Aggregation

**Aggregation** is used to process multiple documents and produce calculated or grouped results.

It is conceptually similar to SQL operations such as:

```text
GROUP BY
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Basic syntax:

```javascript
db.collection.aggregate([
    {
        stage
    }
])
```

---

# 42. Aggregation Pipeline

MongoDB uses an **aggregation pipeline** to process documents through multiple stages.

Example:

```javascript
db.students.aggregate([
    {
        $group: {
            _id: "$course",
            totalStudents: {
                $sum: 1
            }
        }
    }
])
```

This groups students according to their course and counts them.

---

# 43. Aggregation Expressions

| Expression | Purpose |
|---|---|
| `$sum` | Calculates total |
| `$avg` | Calculates average |
| `$min` | Finds minimum |
| `$max` | Finds maximum |
| `$push` | Adds values to an array |
| `$first` | Gets first value/document |
| `$last` | Gets last value/document |
| `$addToSet` | Adds unique values |

---

# 44. Important Aggregation Stages

## `$match`

Filters documents.

```javascript
{
    $match: {
        age: { $gt: 20 }
    }
}
```

Similar to SQL:

```text
WHERE
```

## `$group`

Groups documents.

```javascript
{
    $group: {
        _id: "$course",
        count: { $sum: 1 }
    }
}
```

Similar to SQL:

```text
GROUP BY
```

## `$project`

Selects or calculates fields.

```javascript
{
    $project: {
        name: 1,
        age: 1
    }
}
```

## `$sort`

Sorts aggregation output.

```javascript
{
    $sort: {
        age: -1
    }
}
```

## `$limit`

Limits output.

```javascript
{
    $limit: 5
}
```

## `$skip`

Skips documents.

```javascript
{
    $skip: 5
}
```

## `$unwind`

Converts array elements into separate documents for further processing.

Example:

```javascript
{
    name: "Akash",
    skills: ["Java", "Python"]
}
```

`$unwind` can process each skill separately.

---

# 45. Aggregation Example

Suppose the collection contains:

```javascript
{ name: "A", course: "MSc CA", marks: 80 }

{ name: "B", course: "MSc CA", marks: 90 }

{ name: "C", course: "MCA", marks: 70 }
```

Find average marks by course:

```javascript
db.students.aggregate([
    {
        $group: {
            _id: "$course",
            averageMarks: {
                $avg: "$marks"
            }
        }
    }
])
```

Conceptual result:

```text
MSc CA → 85
MCA    → 70
```

---

# 46. Indexing

An **index** is a special data structure used to improve the speed of database queries.

Without an index:

```text
Query
  ↓
Search documents one by one
  ↓
Collection
```

With an index:

```text
Query
  ↓
Index
  ↓
Matching documents
```

Indexes improve read/search performance.

However, indexes also consume storage and can increase the cost of insert, update and delete operations.

---

# 47. Creating an Index

Syntax:

```javascript
db.collection.createIndex({
    field: 1
})
```

Example:

```javascript
db.students.createIndex({
    student_name: 1
})
```

| Value | Meaning |
|---:|---|
| `1` | Ascending |
| `-1` | Descending |

---

# 48. Default `_id` Index

MongoDB automatically creates a unique index on:

```javascript
_id
```

Therefore, every normal MongoDB collection has an `_id` index.

---

# 49. View Indexes

Use:

```javascript
db.students.getIndexes()
```

This displays the indexes created on the collection.

---

# 50. Drop an Index

## Drop Specific Index

```javascript
db.students.dropIndex({
    student_name: 1
})
```

## Drop All Non-`_id` Indexes

```javascript
db.students.dropIndexes()
```

---

# 51. Array Indexing

MongoDB can create indexes on array fields.

Example document:

```javascript
{
    name: "James",
    tags: [
        "developer",
        "programmer",
        "writer"
    ]
}
```

Create index:

```javascript
db.users.createIndex({
    tags: 1
})
```

This allows queries involving values in the array to make use of the index.

---

# 52. Indexing Nested Fields

Consider:

```javascript
{
    location: {
        city: "Pune",
        country: "India"
    }
}
```

Create an index:

```javascript
db.users.createIndex({
    "location.city": 1,
    "location.country": 1
})
```

Query:

```javascript
db.users.find({
    "location.city": "Pune"
})
```

---

# 53. Advantages of Indexing

- Faster searches
- Efficient query execution
- Faster sorting for suitable queries
- Useful for frequently searched fields

---

# 54. Disadvantages / Limitations of Indexing

Too many indexes can:

- Consume storage
- Consume RAM/memory
- Slow down insert operations
- Slow down update operations
- Slow down delete operations
- Increase maintenance overhead

### Key Point

> **Indexes improve read performance but can reduce write performance.**

---

# 55. Change Streams — Additional Concept

Change Streams allow applications to listen for real-time changes in MongoDB.

Changes can include:

- Insert
- Update
- Delete
- Replace

Basic example:

```javascript
const changeStream = db.collection.watch();

changeStream.on("change", change => {
    console.log(change);
});
```

### Uses

- Real-time notifications
- Event-driven applications
- Synchronization
- Audit systems
- Real-time dashboards

---

# 56. Replication — Additional Concept

Replication means maintaining multiple copies of data on different MongoDB servers.

MongoDB uses a **Replica Set**.

Basic structure:

```text
             Primary
                |
       -------------------
       |                 |
   Secondary         Secondary
```

## Primary

- Receives writes
- Replicates changes to secondary members

## Secondary

- Maintains a copy of the data
- Provides redundancy
- Can participate in failover

## Main Purposes

- High availability
- Fault tolerance
- Data redundancy

---

# 57. Sharding — Additional Concept

Sharding means distributing data across multiple servers.

```text
          Large Dataset
               |
       -------------------
       |        |        |
    Shard 1  Shard 2  Shard 3
```

## Purposes

- Horizontal scaling
- Handling very large datasets
- Distributing workload
- Increasing storage capacity

---

# 58. MongoDB Architecture

Simplified architecture:

```text
Application
     |
     ↓
MongoDB Driver
     |
     ↓
MongoDB Server
     |
     ↓
Database
     |
     ↓
Collection
     |
     ↓
Documents
     |
     ↓
Fields
```

---

# 59. MongoDB Installation — Basic Concept

Basic workflow:

```text
Install MongoDB Server
        ↓
Start MongoDB Server
        ↓
Open mongosh
        ↓
Select Database
        ↓
Create Collection
        ↓
Insert / Query Documents
```

### MongoDB Server

Historically:

```text
mongod
```

### MongoDB Shell

Modern shell:

```text
mongosh
```

---

# 60. Modern vs Older MongoDB Syntax

| Older / Legacy Syntax | Modern Preferred Syntax |
|---|---|
| `insert()` | `insertOne()` / `insertMany()` |
| `update()` | `updateOne()` / `updateMany()` |
| `remove()` | `deleteOne()` / `deleteMany()` |
| `ensureIndex()` | `createIndex()` |
| `mongo` shell | `mongosh` |

For current projects and interviews, prefer the modern APIs.

---

# 61. Complete MongoDB Command Cheat Sheet

```javascript
// Select database
use college

// Show current database
db

// Show databases
show dbs

// Create collection
db.createCollection("students")

// Insert one
db.students.insertOne({
    name: "Akash",
    age: 22
})

// Insert many
db.students.insertMany([
    { name: "A", age: 21 },
    { name: "B", age: 22 }
])

// Read all
db.students.find()

// Read one
db.students.findOne()

// Filter
db.students.find({
    age: 22
})

// Update one
db.students.updateOne(
    { name: "Akash" },
    { $set: { age: 23 } }
)

// Update many
db.students.updateMany(
    { course: "MSc CA" },
    { $set: { active: true } }
)

// Delete one
db.students.deleteOne({
    name: "Akash"
})

// Delete many
db.students.deleteMany({
    age: 22
})

// Sort
db.students.find().sort({
    age: -1
})

// Limit
db.students.find().limit(5)

// Skip
db.students.find().skip(5)

// Create index
db.students.createIndex({
    name: 1
})

// Show indexes
db.students.getIndexes()

// Drop index
db.students.dropIndex({
    name: 1
})

// Aggregation
db.students.aggregate([
    {
        $group: {
            _id: "$course",
            count: { $sum: 1 }
        }
    }
])

// Drop collection
db.students.drop()

// Drop database
db.dropDatabase()
```

---

# 62. Important Operators — Quick Table

| Category | Operators |
|---|---|
| Comparison | `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte`, `$in`, `$nin` |
| Logical | `$and`, `$or`, `$nor`, `$not` |
| Array | `$all`, `$size`, `$elemMatch` |
| Update | `$set`, `$unset`, `$inc`, `$push`, `$pull` |
| Aggregation | `$sum`, `$avg`, `$min`, `$max`, `$push`, `$first`, `$last`, `$addToSet` |

---

# 63. Query Examples

Assume:

```javascript
{
    name: "Akash",
    age: 22,
    course: "MSc CA",
    marks: 85,
    city: "Pune",
    skills: ["Java", "Python"]
}
```

## 1. Find all students

```javascript
db.students.find()
```

## 2. Find students from Pune

```javascript
db.students.find({
    city: "Pune"
})
```

## 3. Find students older than 20

```javascript
db.students.find({
    age: { $gt: 20 }
})
```

## 4. Find students with marks >= 80

```javascript
db.students.find({
    marks: { $gte: 80 }
})
```

## 5. Find students from Pune or Mumbai

```javascript
db.students.find({
    city: {
        $in: ["Pune", "Mumbai"]
    }
})
```

## 6. Display only names

```javascript
db.students.find(
    {},
    {
        _id: 0,
        name: 1
    }
)
```

## 7. Sort by marks descending

```javascript
db.students.find().sort({
    marks: -1
})
```

## 8. Top 5 students

```javascript
db.students
    .find()
    .sort({ marks: -1 })
    .limit(5)
```

## 9. Add a skill

```javascript
db.students.updateOne(
    { name: "Akash" },
    {
        $push: {
            skills: "MongoDB"
        }
    }
)
```

## 10. Delete a student

```javascript
db.students.deleteOne({
    name: "Akash"
})
```

---

# 64. Important Differences

## Collection vs Document

| Collection | Document |
|---|---|
| Group of documents | Individual record |
| Similar to table | Similar to row |
| Contains multiple documents | Contains fields |
| Example: `students` | `{name:"Akash", age:22}` |

---

## `deleteOne()` vs `deleteMany()`

| `deleteOne()` | `deleteMany()` |
|---|---|
| Deletes one matching document | Deletes all matching documents |
| Even if multiple documents match | Every matching document is removed |

---

## `limit()` vs `skip()`

| `limit()` | `skip()` |
|---|---|
| Restricts number of returned documents | Skips documents |
| `limit(5)` → maximum 5 | `skip(5)` → skip first 5 |

---

## Embedded vs Referenced

| Embedded | Referenced |
|---|---|
| Data stored together | Data stored separately |
| Fewer queries | More queries may be needed |
| Good for related data | Good for shared/large data |
| Possible duplication | Less duplication |

---

# 65. Data Modelling — Exam Answer

## Embedded Model

```text
One Document
    |
    ├── Personal Details
    ├── Contact
    └── Address
```

## Referenced Model

```text
Employee
   |
   ├── PersonalDetails
   ├── Contact
   └── Address
```

### Use Embedded Model When

- Data belongs together
- Data is frequently accessed together
- Data is relatively small
- Fewer queries are desired

### Use Referenced Model When

- Data is large
- Data is shared
- Data changes independently
- Separate access is common

---

# 66. MongoDB Advantages vs Limitations

| Advantages | Limitations / Considerations |
|---|---|
| Flexible schema | Less suitable for some highly relational workloads |
| Horizontal scaling | Data duplication can occur |
| Document-oriented | Requires careful schema design |
| High availability | Too many indexes hurt writes |
| Fast development | Large indexes consume resources |
| Embedded documents | Complex relationships may require references |
| Rich querying | Query design affects performance |
| Indexing | Index maintenance has a cost |

---

# 67. Important Definitions — 2/3 Marks

### MongoDB

MongoDB is a **NoSQL document-oriented database** that stores data as BSON documents.

### NoSQL

NoSQL refers to non-relational database systems designed for flexible, distributed and large-scale data.

### Document

A document is a collection of key-value pairs stored in BSON format.

### Collection

A collection is a group of MongoDB documents and is approximately equivalent to a table in RDBMS.

### BSON

BSON stands for **Binary JSON** and is the internal document representation used by MongoDB.

### Index

An index is a data structure used to improve the speed of database queries.

### Aggregation

Aggregation processes multiple documents and produces calculated or grouped results.

### Projection

Projection specifies which fields should be included or excluded from query results.

### Replication

Replication maintains multiple copies of data across MongoDB servers for availability and redundancy.

### Sharding

Sharding distributes data across multiple servers to provide horizontal scalability.

---

# 68. High-Priority Topics for Exam

## Very Important

1. MongoDB introduction
2. SQL vs NoSQL
3. MongoDB terminology
4. Document, Collection, Database
5. BSON and data types
6. Data modelling
7. Embedded vs referenced model
8. CRUD operations
9. Comparison operators
10. Logical operators
11. Projection
12. `limit()` and `skip()`
13. Sorting
14. Aggregation
15. Aggregation expressions
16. Indexing

## Also Prepare

17. Change Streams
18. Replication
19. Sharding
20. Capped Collections
21. Array Indexing
22. Advantages and limitations

---

# 69. MongoDB — One-Page Mental Map

```text
                         MongoDB
                            |
                 -----------------------
                 |                     |
                NoSQL             Document DB
                                      |
                              BSON Documents
                                      |
                            ------------------
                            |                |
                         Collection       Collection
                            |
                         Documents
                            |
                          Fields
                            |
                  -----------------------
                  |         |            |
                 CRUD     Queries      Indexes
                  |         |            |
          ----------------   |       createIndex()
          |   |   |   |      |
        Create Read Update Delete

                         Query Operators
                              |
              -------------------------------
              |              |              |
          Comparison      Logical         Array

                         Aggregation
                              |
                 -----------------------
                 |    |     |     |    |
               match group project sort limit

                         Data Modelling
                              |
                    -------------------
                    |                 |
                 Embedded          Referenced

                       Scalability
                           |
                  -------------------
                  |                 |
              Replication         Sharding
```

---

# 70. Final Revision Formula

```text
MongoDB
   ↓
NoSQL
   ↓
Document-Oriented
   ↓
Database → Collection → Document → Field
   ↓
BSON
   ↓
CRUD
   ↓
Queries + Operators
   ↓
Projection + Sort + Limit + Skip
   ↓
Aggregation
   ↓
Indexing
   ↓
Data Modelling
   ↓
Replication + Sharding + Change Streams
```

## Core Definition to Remember

> **MongoDB is a NoSQL, document-oriented database that stores data as flexible BSON documents inside collections and provides CRUD operations, powerful querying, aggregation, indexing, replication and horizontal scalability.**

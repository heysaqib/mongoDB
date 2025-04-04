**MongoDB Indexing Notes with Useful Query Syntaxes:**

* **Purpose:** Significantly improve query performance by reducing the number of documents scanned.

* **How:** Creates B-tree or other data structures that hold a small portion of the collection's data in an easily traversable form.

* **Types:**

    * **Single-Field Indexes:**
        * Syntax: `db.collection.createIndex({ field: 1/-1 })`
        * Example: `db.students.createIndex({ age: 1 })` (1 for ascending, -1 for descending)

    * **Compound Indexes:**
        * Syntax: `db.collection.createIndex({ field1: 1/-1, field2: 1/-1, ... })`
        * Example: `db.students.createIndex({ age: 1, grade: -1 })` (Order matters!)

    * **Text Indexes:**
        * Syntax: `db.collection.createIndex({ field: "text" })` or `db.collection.createIndex({ "$**": "text" })` for all string fields.
        * Example: `db.students.createIndex({ bio: "text" })`
        * Text search query: `db.students.find({ $text: { $search: "keyword" } }, {ourScore:{$meta: 'textScore'}})`

    * **Geospatial Indexes:**
        * 2dsphere (for GeoJSON): `db.collection.createIndex({ location: "2dsphere" })`
        * 2d (for legacy coordinates): `db.collection.createIndex({ location: "2d" })`
        * Geo Near Query Example: `db.places.find({ location: { $near: { $geometry: { type: "Point", coordinates: [ longitude, latitude ] } } } })`

    * **Unique Indexes:**
        * Syntax: `db.collection.createIndex({ field: 1 }, { unique: true })`
        * Example: `db.users.createIndex({ email: 1 }, { unique: true })`

    * **TTL (Time-To-Live) Indexes:**
        * Syntax: `db.collection.createIndex({ dateField: 1 }, { expireAfterSeconds: seconds })`
        * Example: `db.sessions.createIndex({ lastAccessed: 1 }, { expireAfterSeconds: 3600 })`

* **`explain()`:**

    * Syntax: `db.collection.find(query).explain("executionStats")`
    * Analyzes query execution and reveals index usage (or lack thereof).

* **Over-indexing:**

    * Too many indexes can slow down write operations (inserts, updates, deletes).

* **Index Size:**

    * Indexes consume memory and disk space.

* **Covered Queries:**

    * If all query fields and returned fields are in the index, MongoDB can satisfy the query directly from the index, which is very fast.

* **Index Selection:**

    * MongoDB's query optimizer automatically selects the most efficient index for a query.
    * Consider query patterns when designing indexes.


* **Exercises**

Ex: Retrieve by Index scan all ages above 27 and male teachers
`db.teachers.find({age: {$gte: 27}, gender: 'male'}).explain('executionStats')`


Ex: Create an index for everyone more than age 22
`db.teachers.createIndex({age:1},{partialFilterExpression: {age: {$gt: 22}}})`



* **Covered Query**

`db.teachers.createIndex({name: 1})`
`db.teachers.find({name: 'Mike'},{_id:0, name:1}).explain('executionStats')`


***Weights**
-It sets priority to the fields
`db.students.createIndex({name:'text',bio:'text'}, {weights:{name: 1000, bio:1}})`
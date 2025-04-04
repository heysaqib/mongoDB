$unwind
$group
$size
$match

$bucket
    The $bucket aggregation stage in MongoDB is used to categorize incoming documents into groups, or "buckets," based on specified boundaries. Here's the syntax and a breakdown of its components:

{
  $bucket: {
    groupBy: <expression>,
    boundaries: [ <lowerbound1>, <lowerbound2>, ..., <upperboundN> ],
    default: <literal>,
    output: {
      <output1>: { <accumulator1> : <expression1> },
      <output2>: { <accumulator2> : <expression2> },
      ...
    }
  }
}


* **$lookup**
It is an aggregation pipeline stage that allows you to perform a left outer join between two collections

db.collection.aggregate([
    {
    $lookup: {
        from: "<collection to join>",
        localField: "<field from input documents>",
        foreignField: "<field from documents of the 'from' collection>",
        as: "<output array field>"
  }
}
])


* **$project**
The `$project` stage in MongoDB's aggregation pipeline is used to reshape documents in the pipeline. It allows you to:

* Select specific fields to include or exclude.
* Add new computed fields.
* Rename existing fields.
* Perform various transformations on field values.

**Basic Syntax:**

```javascript
{
  $project: {
    <field1>: <expression1>,
    <field2>: <expression2>,
    ...
  }
}
```

**Explanation:**

* **`<field1>`, `<field2>`, ...:** These are the field names you want to include or create in the output documents.
* **`<expression1>`, `<expression2>`, ...:** These are expressions that determine the values of the corresponding fields.

**Key Concepts and Examples:**

1.  **Including Fields:**

    * To include a field, set its value to `1`.
    * Example:

    ```javascript
    db.students.aggregate([{ $project: { name: 1, age: 1, _id: 0 } }]);
    ```

    * This query includes the `name` and `age` fields and excludes the `_id` field.

2.  **Excluding Fields:**

    * To exclude a field, set its value to `0`.
    * Note: the _id field is included by default, so if you don't want it, you must explicitly exclude it.
    * Example:

    ```javascript
    db.students.aggregate([{ $project: { bio: 0 } }]);
    ```

    * This query excludes the `bio` field.

3.  **Adding New Fields:**

    * You can add new fields by specifying expressions.
    * Example:

    ```javascript
    db.students.aggregate([
      {
        $project: {
          fullName: { $concat: ["$name", " ", "$lastName"] },
          age: 1
        }
      }
    ]);
    ```

    * This query adds a `fullName` field by concatenating the `name` and `lastName` fields.

4.  **Renaming Fields:**

    * You can rename a field by assigning its value to a new field name.
    * Example:

    ```javascript
    db.students.aggregate([{ $project: { studentName: "$name", age: 1 } }]);
    ```

    * This query renames the `name` field to `studentName`.

5.  **Using Expressions:**

    * You can use various aggregation expressions within `$project` to transform field values.
    * Example:

    ```javascript
    db.students.aggregate([
      {
        $project: {
          ageInMonths: { $multiply: ["$age", 12] },
          name: 1
        }
      }
    ]);
    ```

    * This query adds an `ageInMonths` field by multiplying the `age` field by 12.

6. **Using conditional logic**
    * Example:

    ```javascript
    db.students.aggregate([{
        $project:{
            status : {$cond : {if: {$gt: ["$age", 20]}, then: "adult", else: "minor"}},
            name:1
        }
    }])
    ```

    * This query adds a status field based on the age of the student.

Ex: Format the display of employee details properly and show annual salary as well
```javascript
db.empdb.aggregate([{$project: {_id:0, EmployeeId:'$eid', EmployeeName: '$ename', Department:'$dept', MonthlySalary:'$salary', AnnualSalary:{$multiply:[12,'$salary']}}}])
```

**Key Points:**

* `$project` is powerful for reshaping documents.
* You can include, exclude, add, and rename fields.
* Expressions allow for complex transformations.
* The `_id` field is included by default; you must explicitly exclude it if needed.

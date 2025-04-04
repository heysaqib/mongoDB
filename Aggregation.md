```javascript

AGGREGATION
- It groups the data from multiple documents into a single document based on the specified expression

AGGREGATION PIPELINE
- The aggregation process in MongoDB consists of several stages, each stage transforming the data in some way.
- The output of one stage is fed as the input to the next stage, and so on, until the final stage produces the desired result.
- MongoDB provides several built-in aggregation pipeline stages to perform various operations on the data, such as $group, $sum, $avg, $min, $max, etc.


db.collection.aggregate(pipeline, options)
 here,  pipeline arguement is a array of operations
        options arguement is used rarely


db.teachers.aggregate( [ { $match:{ gender:"male" } } ])
 here, we are grouping all teachers by age.


    $group:
    { _id: expression,
    field1: expression,
    field2: expression, ... }

Ex: 'group teachers by age'
db.teachers.aggregate([ {$group: { _id: "$age"}} ])
 here,  The $group operator groups documents by the age field, creating a new document for each unique age value. 
        The _id field in the group stage specifies the fiel d based on which the documents will be grouped.

Ex: 'group teachers by age, and show names according to the age group'
db.teachers.aggregate([{$group: { _id: "$age", names: { $push:"$name" } }}])
 here, The names field uses the $push operator to add the name field from each document in the group to an array

Ex: 'group teachers by age, and show complete document per age group'
db.teachers.aggregate([{$group:{ _id: "$age", students: { $push:"$$ROOT" } }}])
 here, the $$ROOT value is a reference to the current document being processed in the pipeline, which represents the complete document

Ex: 'give a count per age of male teacher'
db.teachers.aggregate([{$match: {gender: 'male'}},{ $group: {_id: '$age', count: {$sum:1} }}])
 here, the value of $sum is 1, which means that for each document in the group, the value of 'count' will be incremented by 1

 Ex: 'give a count per age of male teachers and sort them by count in descending manner'
 db.teachers.aggregate([{$match: {gender: 'male'}},{$group: {_id: '$age', count: {$sum:1}}},{$sort: {count: -1}}])


syntax of the above example:
 db.teachers.aggregate([{operator 1},{operator 2},{operator 3}])


Ex: 'Find Hobbies per age group'
db.students.aggregate([{$unwind: '$Hobbies'},{$group: {_id: '$age',hobbies: {$push: '$Hobbies'}}}])
 here, $unwind creates a copies of data for all the individual hobbies of the student.

Ex: 'Find number of students per each hobby'
db.students.aggregate([{$unwind: '$Hobbies'}, {$group: {_id: '$Hobbies', count: {$sum:1}}}])
 here, we use unwind to seperate all the hobbies and then group them to count the number of students at each hobby


 Ex: 'Find average age of all students'
 db.students.aggregate([{ $group: {_id: null, average: {$avg: '$age'}}}])
  here, if you specify the _id:null in the $group operator, it means that all the documents in the collection will be grouped together into a single document


Ex: 'Find the total number of hobbies for all the students in a collection'
db.students.aggregate([{$unwind: '$Hobbies'}, {$group: {_id: null, totalNumberOfHobbies: {$sum: 1}}}])
//also you can acheive it by,
db.students.aggregate([{$group: {_id: null, count: {$sum: {$size: '$Hobbies'}}}}])


Ex: 'List all hobbies'
db.students.aggregate([{$unwind: '$Hobbies'},{$group: {_id: null, allHobbies: {$push: '$Hobbies'}}}])
 here,  $push adds all the hobbies which results in repeatation. 
        Instead use $addToSet to avoid repeatation of same hobbies.


Ex: 'Bulk write scores arrays into every document'
async function updateScores() {
...   const scoresArrays = [
...     [85, 92, 78, 89, 95, 81],
...     [76, 88, 91, 84, 79, 86],
...     [90, 83, 87, 92, 88, 94],
...     [78, 81, 85, 76, 82, 79],
...     [94, 91, 89, 96, 93, 90],
...     [82, 86, 80, 83, 87, 84],
...     [89, 93, 88, 91, 90, 92],
...     [77, 80, 75, 79, 81, 78],
...     [92, 87, 90, 94, 88, 91],
...     [84, 89, 83, 86, 90, 85],
...     [87, 91, 85, 88, 92, 89],
...     [80, 84, 79, 82, 85, 81],
...     [91, 86, 89, 93, 87, 90],
...     [79, 83, 77, 81, 84, 80],
...     [95, 90, 93, 97, 92, 94],
...     [83, 87, 81, 85, 88, 82],
...     [90, 94, 89, 92, 91, 93],
...     [78, 82, 76, 80, 83, 79],
...     [93, 88, 91, 95, 89, 92],
...     [85, 90, 84, 87, 91, 86]
...   ];
...
...   const bulkOps = scoresArrays.map((scores, index) => ({
...     updateOne: {
...       filter: { _id: (index + 1).toString() }, // Adjust this filter if your _id structure is different
...       update: { $set: { scores: scores } },
...     },
...   }));
...
...   try {
...     const result = await db.students.bulkWrite(bulkOps);
...     print(`Bulk update completed. Modified ${result.modifiedCount} documents.`);
...   } catch (err) {
...     print(`Error during bulk update: ${err}`);
...   }
... }
...
... updateScores();


Ex: 'Find average of scores for students whose age is greater than 20'
db.students.aggregate([{$group: {_id: null, avgScore: {$avg: {$filter: {input: '$scores', as: 'score', cond: { $gt: {'$age',20}}}}}}}]) //this is not working
 but, most accurate one is the below one,
db.students.aggregate([{$match: {age: {$gt: 20}}},{$unwind: '$scores'},{$group: {_id: null, avgScore: {$avg: '$scores'}}}])


Ex: 'Categorize male teachers based on their ages into three buckets ages less than 30, ages between 30 and 40, and ages greater than 40 along with their names'
db.teachers.aggregate([{$match: {gender: 'male'}},{$bucket: {groupBy: '$age', boundaries: [0,35,40], default: 'Greater than 40', output: {count: {$sum: 1}, names: { $push: '$name'}}}}])
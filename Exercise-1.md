```javascript
// Assuming you have MongoDB shell open and let's create the 'studb' database.

> use studb
switched to db studb

> doc1=({srn:110,sname:"Rahul",degree:"BCA",sem:6,CGPA:7.9})

> db.studb.insert(doc1)


// Insert 10 documents:
> db.stud09.insertMany([
  { srn: 110, sname: "Rahul", degree: "BCA", sem: 6, CGPA: 7.9 },
  { srn: 111, sname: "Sneha", degree: "BSc", sem: 4, CGPA: 8.5 },
  { srn: 112, sname: "Amit", degree: "BCA", sem: 5, CGPA: 6.8 },
  { srn: 113, sname: "Priya", degree: "BBA", sem: 6, CGPA: 9.2 },
  { srn: 114, sname: "Kiran", degree: "BCA", sem: 6, CGPA: 7.2 },
  { srn: 115, sname: "Maya", degree: "BSc", sem: 3, CGPA: 6.5 },
  { srn: 116, sname: "Vikram", degree: "BBA", sem: 5, CGPA: 8.8 },
  { srn: 117, sname: "Neha", degree: "BCA", sem: 4, CGPA: 6.2 },
  { srn: 118, sname: "Arjun", degree: "BSc", sem: 6, CGPA: 9.0 },
  { srn: 119, sname: "Divya", degree: "BCA", sem: 5, CGPA: 7.5 }
]);

// 1. Display all the documents
> db.stud09.find();

// 2. Display all the students in BCA
> db.stud09.find({ degree: "BCA" });

// 3. Display all the students in ascending order (based on SRN)
> db.stud09.find().sort({ srn: 1 });

// 4. Display first 5 students
> db.stud09.find().limit(5);

// 5. Display students 5, 6, 7 (skip 4, limit 3)
> db.stud09.find().skip(4).limit(3);

// 6. List the degree of student "Rahul"
> db.stud09.findOne({ sname: "Rahul" }, { degree: 1, _id: 0 });

// 7. Display students details of 5, 6, 7 in descending order of CGPA
> db.stud09.find().skip(4).limit(3).sort({ CGPA: -1 });

// 8. Display the number of students in BCA
> db.stud09.countDocuments({ degree: "BCA" });

// 9. Display all the degrees without _id
> db.stud09.find({}, { degree: 1, _id: 0 });

// 10. Display all the distinct degrees
> db.stud09.distinct("degree");

// 11. Display all the BCA students with CGPA greater than 6, but less than 7.5
> db.stud09.find({ degree: "BCA", CGPA: { $gt: 6, $lt: 7.5 } });

// 12. Display all the students in BCA and in 6th Sem
> db.stud09.find({ degree: "BCA", sem: 6 });
```javascript
// Assuming you have MongoDB shell open and let's create the 'empdb' database.
> use empdb
switched to db empdb

> doc1 = {eid:001, ename:"Rahul", dept:"production", desig:"developer", salary:30000, yoj:2015,		     address:{dno:397, street:2, locality:"rmnagar", city:"bangalore"} }

> db.empdb.insert(doc1)


// Insert 10 documents:
db.empdb.insertMany([
  { eid: 1, ename: "Rahul", dept: "production", desig: "developer", salary: 30000, yoj: 2015, address: { dno: 397, street: 2, locality: "rmnagar", city: "bangalore" } },
  { eid: 2, ename: "Sneha", dept: "sales", desig: "manager", salary: 60000, yoj: 2018, address: { dno: 123, street: 10, locality: "jayanagar", city: "bangalore" } },
  { eid: 3, ename: "Amit", dept: "production", desig: "designer", salary: 45000, yoj: 2016, address: { dno: 456, street: 5, locality: "mgroad", city: "bangalore" } },
  { eid: 4, ename: "Priya", dept: "marketing", desig: "developer", salary: 70000, yoj: 2019, address: { dno: 789, street: 8, locality: "indiranagar", city: "bangalore" } },
  { eid: 5, ename: "Kiran", dept: "sales", desig: "analyst", salary: 55000, yoj: 2017, address: { dno: 101, street: 3, locality: "koramangala", city: "bangalore" } },
  { eid: 6, ename: "Maya", dept: "production", desig: "tester", salary: 48000, yoj: 2020, address: { dno: 202, street: 7, locality: "whitefield", city: "bangalore" } },
  { eid: 7, ename: "Vikram", dept: "marketing", desig: "manager", salary: 80000, yoj: 2014, address: { dno: 303, street: 1, locality: "rrnagar", city: "bangalore" } },
  { eid: 8, ename: "Neha", dept: "sales", desig: "developer", salary: 65000, yoj: 2021, address: { dno: 404, street: 9, locality: "electroniccity", city: "bangalore" } },
  { eid: 9, ename: "Arjun", dept: "production", desig: "designer", salary: 52000, yoj: 2013, address: { dno: 505, street: 4, locality: "marathahalli", city: "bangalore" } },
  { eid: 10, ename: "Divya", dept: "marketing", desig: "analyst", salary: 75000, yoj: 2022, address: { dno: 606, street: 6, locality: "hsrLayout", city: "bangalore" } }
]);

// 1. Display all the employees with salary in range (50000, 75000)
db.empdb.find({ salary: { $gt: 50000, $lt: 75000 } });

// 2. Display all the employees with desig developer
db.empdb.find({ desig: "developer" });

// 3. Display the Salary of “Rahul”
db.empdb.findOne({ ename: "Rahul" }, { salary: 1, _id: 0 });

// 4. Display the city of employee “Rahul”
db.empdb.findOne({ ename: "Rahul" }, { "address.city": 1, _id: 0 });

// 5. Update the salary of developers by 5000 increment
db.empdb.updateMany({ desig: "developer" }, { $inc: { salary: 5000 } });

// 6. Add field age to employee “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $set: { age: 30 } }); //replace 30 with correct age.

// 7. Remove YOJ from “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $unset: { yoj: 1 } });

// 8. Add an array field project to “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $set: { projects: ["p1"] } });

// 9. Add p2 and p3 project to “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $push: { projects: { $each: ["p2", "p3"] } } });

// 10. Remove p3 from “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $pull: { projects: "p3" } });

// 11. Add a new embedded object “contacts” with “email” and “phone” as array objects to “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $set: { contacts: { email: ["rahul@example.com"], phone: ["123-456-7890"] } } });

// 12. Add two phone numbers to “Rahul”
db.empdb.updateOne({ ename: "Rahul" }, { $push: { "contacts.phone": { $each: ["987-654-3210", "555-123-4567"] } } });
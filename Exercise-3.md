```javascript
// Assuming you have MongoDB shell open and you've already created the 'bookdb' database.
> use bookdb
switched to db bookdb

> doc1=({isbn:"e40", bname:"let us C", author:["yeshanth", "kanaka"], year:2012, publisher:"pearson", price:100})

> db.book.insert(doc1)

// Insert 5 documents:
db.book.insertMany([
  { isbn: "e40", bname: "let us C", author: ["yeshanth", "kanaka"], year: 2012, publisher: "pearson", price: 100 },
  { isbn: "e41", bname: "java", author: ["herbert", "schildt"], year: 2018, publisher: "mcgraw hill", price: 250 },
  { isbn: "e42", bname: "data structures", author: ["rudresh", "anand"], year: 2020, publisher: "oxford", price: 180 },
  { isbn: "e43", bname: "operating systems", author: ["kuvempu", "galvin"], year: 2015, publisher: "pearson", price: 220 },
  { isbn: "e44", bname: "algorithms", author: ["rama", "john"], year: 2023, publisher: "mit press", price: 300 }
]);

// 1. list all the documents.
db.book.find();

// 2. list all the book name except year and price.
db.book.find({}, { bname: 1, year: 0, price: 0, _id: 0 });

// 3. display all the books authored by rudresh.
db.book.find({ author: "rudresh" });

// 4. list all the books published by pearson.
db.book.find({ publisher: "pearson" });

// 5. list the publisher of book java.
db.book.findOne({ bname: "java" }, { publisher: 1, _id: 0 });

// 6. list the author, publisher and year of the book let us C.
db.book.findOne({ bname: "let us C" }, { author: 1, publisher: 1, year: 1, _id: 0 });

// 7. Display the price of “let us C” except _id.
db.book.findOne({ bname: "let us C" }, { price: 1, _id: 0 });

// 8. sort and display all books in ascending order of book names.
db.book.find().sort({ bname: 1 });

// 9. sort and display only 3 books in descending order of price.
db.book.find().sort({ price: -1 }).limit(3);

// 10. Display all the books written by herbert and kuvempu.
db.book.find({ author: { $all: ["herbert", "kuvempu"] } });

// 11. Display all the books either written by herbert or kuvempu.
db.book.find({ author: { $in: ["herbert", "kuvempu"] } });

// 12. display all the books where rama is the first author.
db.book.find({"author.0" : "rama"});
```

**Explanation:**

* **`$all`**: Selects the documents where the value of a field is an array that contains all the specified elements.
* **`$in`**: Selects the documents where the value of a field equals any value in the specified array.
* **`"author.0": "rama"`**: Accesses the first element of the `author` array and checks if it's "rama". This is how you target a specific index in an array within a MongoDB query.

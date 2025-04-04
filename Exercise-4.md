```javascript
// Assuming you have MongoDB shell open and you've already created the 'fooddb' database.

// Insert 5 documents:
db.food.insertMany([
  { foodid: 1, foodcat: "fastfood", foodname: "burger", chefname: ["naveen", "rakesh"], price: 500, ingredients: ["cheese", "corn"], hotelname: "mcburger", address: { no: 31, street: "belroad", locality: "yelahanka", city: "bangalore" } },
  { foodid: 2, foodcat: "italian", foodname: "pizza", chefname: ["john", "alice"], price: 700, ingredients: ["cheese", "tomato", "olives"], hotelname: "pizzahut", address: { no: 12, street: "mgroad", locality: "indiranagar", city: "bangalore" } },
  { foodid: 3, foodcat: "indian", foodname: "biryani", chefname: ["ram", "sita"], price: 600, ingredients: ["rice", "chicken", "spices"], hotelname: "paradise", address: { no: 45, street: "jayanagar", locality: "koramangala", city: "bangalore" } },
  { foodid: 4, foodcat: "fastfood", foodname: "fries", chefname: ["bob", "eve"], price: 300, ingredients: ["potato", "salt"], hotelname: "mcdonalds", address: { no: 78, street: "hsrLayout", locality: "electroniccity", city: "bangalore" } },
  { foodid: 5, foodcat: "chinese", foodname: "noodles", chefname: ["lee", "mei"], price: 550, ingredients: ["noodles", "vegetables"], hotelname: "chinatown", address: { no: 99, street: "whitefield", locality: "marathahalli", city: "bangalore" } }
]);

// 1. list the price of pizza with ingredients.
db.food.findOne({ foodname: "pizza" }, { price: 1, ingredients: 1, _id: 0 });

// 2. display the item in the price range(500,800).
db.food.find({ price: { $gte: 500, $lte: 800 } });

// 3. display the item prepared by john and alice.
db.food.find({ chefname: { $all: ["john", "alice"] } });

// 4. Display the item prepared by john or alice.
db.food.find({ chefname: { $in: ["john", "alice"] } });

// 5. Add one chef to the food pizza.
db.food.updateOne({ foodname: "pizza" }, { $push: { chefname: "david" } });

// 6. Add ingredients to the food burger.
db.food.updateOne({ foodname: "burger" }, { $push: { ingredients: { $each: ["onion", "tomato"] } } });

// 7. Delete last ingredient added to the food burger.
db.food.updateOne({ foodname: "burger" }, { $pop: { ingredients: 1 } });

// 8. Delete All the ingredients from the food biryani.
db.food.updateOne({ foodname: "biryani" }, { $set: { ingredients: [] } });

// 9. Add food type to the food burger.
db.food.updateOne({ foodname: "burger" }, { $set: { foodtype: "veg" } });

// 10. Modify the burger price by 200.
db.food.updateOne({ foodname: "burger" }, { $inc: { price: 200 } });

// 11. Add or insert a new food item with the food Id “f08 “ using upsert as True.
db.food.updateOne({ foodid: "f08" }, { $set: { foodcat: "dessert", foodname: "icecream", price: 250 } }, { upsert: true });

// 12. Increment the price of all food item in food cat: fastfood by 120.
db.food.updateMany({ foodcat: "fastfood" }, { $inc: { price: 120 } });
```

**Explanation:**

* **`$gte`**: Greater than or equal to.
* **`$lte`**: Less than or equal to.
* **`$pop: { ingredients: 1 }`**: Removes the last element from the `ingredients` array.
* **`$set: { ingredients: [] }`**: Sets the `ingredients` array to an empty array.
* **`upsert: true`**: If a document with the specified `foodid` doesn't exist, it inserts a new document; otherwise, it updates the existing document.
* **`updateMany`**: updates all documents that match the filter.

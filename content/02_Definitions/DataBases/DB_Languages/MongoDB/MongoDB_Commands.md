#DataBases 

---
## # Shell-Commands
### # MongoDB Cheatsheet

```js
// if database exists: switch to it - otherwise creates database
use test;  

// show/drop available databases
show databases;  
db.dropDatabase("test");

// show/create/drop collection
show collections;  
db.createCollection("any");  
db.any.drop();


db.any.remove({});  

// insert into collection
db.any.insertOne({  
    name: "Apple",  
    price: 0.99,  
}); 
db.any.insertMany(  
    [  
        {  
            name: "Plum",  
            price: 0.80,  
        },  
        {  
            name: "Kiwi",  
            price: 1.20,  
            ratings: [  
                { user: "Lassl", stars: 5},  
                { user: "Haul", stars: 4},  
                { user: "Muster", stars: 1}  
            ]  
        }  
    ]  
); 
  
// show all documents
db.any.find();

// selection - WHERE-statement
db.any.find({name: "Apple"});
db.any.find({age: 18});
// sorting ascending/descending
db.any.find().sort({name: 1});  // descending
db.any.find().sort({name: -1}); // ascending

// projection - show documents 
db.any.find({}, {name:0});  // show all fields - without field: name  
db.any.find({}, {name:1});  // show only _id-field & name-field 

// aggregate functions
// - count
db.any.find().count();
db.any.countDocuments();

```

---
## # MQL

The MongoDB Query Language supports a wide range of queries, including CRUD operations, aggregation, geospatial queries.

---

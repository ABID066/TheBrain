#### Create and Select a Database

In the MongoDB, use the `use('databaseName')` command to create and switch to a new database

```js
use('CraftShop')
```

####  Insert One

```js
use('CraftShop')
db.employees.insertOne({
    "name": "Nasim Ahmed",
    "designation": "System Administrator",
    "salary": 75000,
    "city": "Barisal"
})
```

#### Insert Many

``` js
use('CraftShop')
db.employees.insertMany([

    {
        "name": "Ahmed Hossain",
        "designation": "Software Engineer",
        "salary": 85000,
        "city": "Dhaka"
    },
	{
	    "name": "Fatima Begum",	
	    "designation": "Product Manager",	
	    "salary": 105000,	
	    "city": "Chittagong"	
	},	
	{
	    "name": "Anik Chowdhury",
	    "designation": "DevOps Engineer",
	    "salary": 82000,
	    "city": "Sylhet"
	}
	
]);
```

#### Rename
```js
use("Inventory")

db.brands.dropIndex("name_1") // Drop the unique index on `name` //if needed use it

db.brands.updateMany({}, { $rename: { "name": "brand_name" } }) // Rename field
```


#### Finding Document

- Fine documents from a collection using the `find` method.

**Comparison Operators:**

- `$eq:`  ***Equal To*** - Operator
- `$lt:` ***Less Than*** - Operator
- `$lte:` ***Less Than or Equal To*** - Operator 
- `$gt:` ***Greater Than*** - Operator
- `$lte:` ***Less Than or Equal To*** - Operator 
- `$gt:` ***Greater Than*** - Operator
- `$gte:` ***Greater Than or Equal To*** - Operator
- `$ne:` ***Not Equal To*** Operator
- `$in:`***In*** Operator
- `$nin:` ***Not In*** Operator
  

```js
use('CraftShop')
db.employees.find(
    {
        salary: { $gt: 80000 }
    },
    {
        _id: 0,
        name: 1,
        designation: 1,
        salary: 1,
        city: 1
    }
)
```


When condition is in a single object
```js

use('CraftShop');
db.employees.find(
    {
        salary: { $gt: 80000 },
        designation: "Engineer"
    },
    {
        _id: 0,
        name: 1,
        designation: 1,
        salary: 1,
        city: 1
    }
);
```


When condition is in multiple object
```js
use("CraftShop");
db.employees.find(
    {
        $and: [
            { salary: { $gte: 90000 } },
            { city: { $eq: "Dhaka" } }
        ]
    },
    {
        _id: 0
    }
);

```

#### Multiple condition Using Logical Operator
  
  ```JS
use ("CraftShop")
db.employees.find({

    $or:[
        {salary:{$gte:90000}},
        {city:{$eq:'Dhaka'}}
	    ]
    },
    {
        _id:0
    })
```

#### Element Query Operator : `exists`

	Check if the `country` value exist then show
```js

use ("CraftShop")

db.brands.find({
   country: {$exists:false}
},{
    _id:0
})
```


#### Element Query Operator : `type`

|   Type   | **Number** |  **Alice**   |
| :------: | :--------: | :----------: |
|  Double  |     1      |  `"Double"`  |
|  String  |     2      |  `"string"`  |
|  Object  |     3      |  `"object"`  |
|  Array   |     4      |  `"array"`   |
| ObjectId |     7      | `"objectid"` |
| Boolean  |     8      | `"boolean"`  |
|   Null   |     10     |   `"null"`   |
	It checks what `type` of data in there
```js

use ("CraftShop")

db.employees.find({
    salary:{$type:2}

},{
    _id:0
})
```

#### Evaluation Query Operator : Expression 

						just comparing
```js
use('CraftShop')

db.monthlyBudget.find({
    $expr:
    {
        $gt:["$budget","$spend"]
    }
})
```

	                    just comparing 
```js
use('CraftShop')

db.monthlyBudget.find({
    $where:"this.budget<this.spend"
})
```

	                      just comparing 
```js
use('CraftShop')

db.monthlyBudget.find({
    $where:"this.budget==this.spend"
})
```
#### Evaluation Query Operator :  `$mod`

```js
use('CraftShop')

db.monthlyBudget.find({
    budget:{$mod:[4,1]}   //Divided by 4 but at the end (ভাগশেষ) is 1
```

#### Evaluation Query Operator :  Regular Expression

```js
use('CraftShop')

db.employees.find({
    name:{
		$regex:"An",  //used for keyword searching
	    "$options":"i"  //work with both  lowercase & uppercase
	}
})  

```

#### Sorting

- sorting the name descending order
```js
use('CraftShop')

db.brands.find().sort({
    name:-1
}) 
```

- count total **Number of Data** or **Number of Row**
```js
use('CraftShop')

db.brands.find().count('total')
```

```js
let total = await UserModel.countDocuments({ email: email, otp: otp });
```

- select last two data of the table or collection
```js
use('CraftShop')

db.brands.find().sort({"_id":-1}).limit(2)
```

- show unique data
```js
use('CraftShop')

db.brands.distinct('name')
```
#### Delete Operation

- delete One using `ObjectId`
``` js
use('CraftShop')

db.brands.deleteOne({
    "_id": ObjectId('665d5a8f0981c37c0b7ccf54')
}) 
```

- delete many using `salary` and `in` operator
```js
use('CraftShop')

db.employees.deleteMany({
    salary:{$in:[72000,82000]}
}) 

```


#### Update Operation

```js
const result = await User.updateOne(

		{ name: "John Doe" }, // Filter

		{ $set: { age: 30 } } // Update
	
	);
```

```js
const result = await User.updateOne( 

		{ name: "Alice" },   // Filter: Find user with name "Alice" 
		
		{ $set: { age: 28 } }, // Update: Set age to 28 
		
		{ upsert: true } // If not found, insert a new document 
	
	);
```
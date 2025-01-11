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
    budget:{$mod:[4,1]}
}) 
```

#### Evaluation Query Operator :  Regular Expression

```js
use('CraftShop')

db.employees.find({
    name:{$regex:"An"}
}) regex 

```

#### Sorting

- sorting the name descending order
```js
use('CraftShop')

db.brands.find().sort({
    name:-1
}) 
```

- count total number of data
```js
use('CraftShop')

db.brands.find().count('total')
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



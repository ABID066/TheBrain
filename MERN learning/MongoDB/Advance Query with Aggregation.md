
#### Counting Number of Row

```js

db.employees.aggregate([

	{$count: 'toatal'}

])
```


#### Sorting, Skipping & Selecting

```js
db.employees.aggregate([

{$sort:{salary:-1}}

])
```

```js
db.employees.aggregate([

{$sort: {_id:-1}},
{$limit:2}

])
```

```js
db.employees.aggregate([

{$sort:{_id:-1}},
{$skip:2},
{$limit:2}

])

//Same code
db.employees.find({}).sort({"_id":-1}).skip(2).limit(2)
```



#### Find data using `$match` condition

```js
db.employees.aggregate([
 
 {$match: {salary:{$lt:90000}}}
 
])
```



#### Using `$match` with multiple condition

```js
db.employees.aggregate([

{$match:{salary:{$lt:100000}}},
{$match:{city:{$nin:["Dhaka"]}}}

])
```

```js
db.employees.aggregate([

{$match:{salary:{$lte:100000}}},
{$match:{city:"Dhaka"}}

])

//Same Code
db.employees.aggregate([

{$match:{$and:[
		{salary:{$lte:100000}},
		{city:"Dhaka"}
	]}}
])
```



#### Select alike data

```js 
db.employees.aggregate([
	{$match:{designation:/Scientist/}}
])

//Same code
db.employees.find({designation:/Eng/})
```



#### Projection the data by Column

```js
db.employees.aggregate([

	{$project:{_id:0, name:1}}

])
```



### Show data by Group


##### Show only unique data
```js
db.employees.aggregate([

	{$group:{_id:"$city"}}  
	
	//In here, _id is not that you think. This is the property of $group

])

//Same code
db.employees.distinct('city')
```

##### show sum by grouping
```js
db.employees.aggregate([

	{$group:{_id:"$designation", total:{$sum:"$salary"}}}

])
```

##### show average by grouping 
```js
db.employees.aggregate([

	{$group:{_id:"$designation", avg:{$avg:"$salary"}}}

])
```

##### show max value by grouping
```js
db.employees.aggregate([

	{$group:{_id:"$designation", max:{$max:"$salary"}}}

])

```

##### show min value by grouping
```js

db.employees.aggregate([

	{$group:{_id:"$designation", min:{$min:"$salary"}}}

])
```

##### combine group
```js
db.employees.aggregate([
{$group:
	 {
		 _id:{city: "$city"},
		 avg:{$avg:"$salary"},
		 sum:{$sum:"$salary"},
		 max:{$max:"$salary"},
		 min:{$min:"$salary"}
	}}
])
```

```js
db.employees.aggregate([
{$group:
	{
		_id:{designation: "$designation", city: "$city"},
		avg:{$avg:"$salary"},
		sum:{$sum:"$salary"},
		max:{$max:"$salary"},
		min:{$min:"$salary"}
	}}
])
```
#### Show Max, Min & Avg value of a column

- Max value
```js
db.employees.aggregate([

	{$group:{_id:0, max:{$max:"$salary"}}},

])
```

- Max value
```js
db.employees.aggregate([

	{$group:{_id:0, min:{$min:"$salary"}}},

])
```

- Avg value
```js
db.employees.aggregate([

	{$group:{_id:0, avg:{$avg:"$salary"}}},

])
```



#### Add a new column & Set value with condition

- Add a column and the values of this column depend on the value of the price
```js
db.products.aggregate([

{
	$addFields:{
		something:{
			$gte:[{$toDouble:"$price"} ,12000]
		}
	}
},

{$match:{"something" : false}}

])
```


- Add a column and the values of this column depend on the value of the price and city.
- if both condition is true then it will be true.
```js
db.employees.aggregate([

{
    $addFields: {
        something: {
            $and: [
                { $gt: ["$salary", 80000] },
                { $eq: ["$city", "Dhaka"] }
            ]
        }
    }
}
,

{$match:{"something" : true}}

])
```


```js
db.employees.aggregate([
    {
        $addFields: {
            value: {
                $switch: {
                    branches: [
                        {
                            case: { $and: [
                            
		 { $gt: ["$salary", 80000] }, 
		{ $eq: ["$city", "Dhaka"] }
										
					                    ] 
					                },
                            then: 30
                        },
                        {
                            case: { $gt: ["$salary", 60000] },
                            then: 20
                        },
                        {
                            case: { $eq: ["$city", "Mumbai"] },
                            then: 10
                        }
                    ],
                    
                    default: 0
	                // Default value if no conditions match
                }
            }
        }
    }   
]);

```


### Deconstructs an array field ($unwind):

- when data is insert like this

```js
db.inventory.insertOne({ "_id" : 1, "item" : "ABC1", sizes: [ "S", "M", "L"] })
```

- Then use this

```js
db.inventory.aggregate( [ { $unwind : "$sizes" } ] )
```

- And the result will be

```json
{ "_id" : 1, "item" : "ABC1", "sizes" : "S" },
{ "_id" : 1, "item" : "ABC1", "sizes" : "M" }, 
{ "_id" : 1, "item" : "ABC1", "sizes" : "L" }
```


#### How it works:

- `reduce()` takes two parameters

```js

const numbers = [1, -1, 2, 3]

const sum = numbers.reduce((accumulator, currentValue) => {
	return accumulator + currentValue;
}, 0) //---> In here, 1st accumulator's value initialize as 0, then current value will be number[0] as 1 


//And it will return 5
```

-  1st  --->>   a=0,     c=1    =>  a=1 
-  2nd --->>   a=1,    c=-1   =>  a=0 
-  3rd  --->>   a=0,    c=2    =>  a=2 
-  2nd --->>   a=2,    c=3    =>  a=5 


```js

const numbers = [1, -1, 2, 3]

const sum = numbers.reduce((accumulator, currentValue) => {
	return accumulator + currentValue;
})//---> In here, 1st accumulator's value doesn't initialize so it will be number[0] as 1, then current value will be number[1] as -1 

//And it will return 5
```
-  1st  --->>   a=1,    c=-1   =>  a=0 
-  2nd --->>   a=0,    c=2    =>  a=2 
-  3rd  --->>   a=2,    c=3    =>  a=5 



```js

const maxID = data.reduce((max, task) => {
           return task.id > max ? task.id : max;
}, 0);

```





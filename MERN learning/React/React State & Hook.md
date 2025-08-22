#### Best use of `useSate()`: 

```js
import { useState } from "react";
import axios from 'axios';

const App = () => {
  const [loginData, setLoginData] = useState({ email: "", pass: "", fname: "", lname: "" });

  const InputOnChange = (e) => {
    let key = e.target.name;
    let value = e.target.value;
    setLoginData(loginData => ({
      ...loginData,
      [key]: value
    }));
  };

  const SaveData = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post('https://my-api-endpoint.com/login', loginData);
      alert(`Data sent: ${JSON.stringify(loginData)}`);
      console.log(response.data);
    } catch (error) {
      console.error('There was an error sending the data!', error);
    }
  };

  return (
    <div>
      <ul>
        <li>Email: {loginData.email}</li>
        <li>Pass: {loginData.pass}</li>
        <li>Fname: {loginData.fname}</li>
        <li>Lname: {loginData.lname}</li>
      </ul>
      <form onSubmit={SaveData}>
        <input name="fname" onChange={(e) => {InputOnChange(e)}} type="text" placeholder="first name"/><br/><br/>
        <input name="lname" onChange={(e) => {InputOnChange(e)}} type="text" placeholder="last name"/><br/><br/>
        <input name="email" onChange={(e) => {InputOnChange(e)}} type="text" placeholder="email"/><br/><br/>
        <input name="pass" onChange={(e) => {InputOnChange(e)}} type="text" placeholder="password"/><br/><br/>
        <button type="submit">Submit</button>
      </form>
    </div>
  );
};

export default App;


```
***OR,***
```js

import axios from 'axios';
import { useState } from "react";

const App = () => {

  const [loginData, setLoginData] = useState({ email: "", pass: "", fname: "", lname: "" });


  function InputOnChange (e) {

    let key = e.target.name;
	let value = e.target.value;

    setLoginData(loginData => ({
    
		...loginData,
		[key]: value

    }))};

  

  async function SaveData (e) {
	 e.preventDefault();

    try {

      const response = await axios.post('https://my-api-endpoint.com/login', loginData);
	alert(`Data sent: ${JSON.stringify(loginData)}`);
	console.log(response.data);

    } catch (error) {
		console.error('There was an error sending the data!', error);

    }

  };

  

  return (

    <div>
      <ul>
        <li>Email: {loginData.email}</li>
        <li>Pass: {loginData.pass}</li>
        <li>Fname: {loginData.fname}</li>
        <li>Lname: {loginData.lname}</li>
      </ul>

      <form onSubmit={SaveData}>
        <input name="fname" onChange={InputOnChange} type="text" placeholder="first name"/><br/><br/>
        <input name="lname" onChange={InputOnChange} type="text" placeholder="last name"/><br/><br/>
        <input name="email" onChange={InputOnChange} type="text" placeholder="email"/><br/><br/>
        <input name="pass" onChange={InputOnChange} type="text" placeholder="password"/><br/><br/>
        <button type="submit">Submit</button>
      </form>
      
    </div>

  );
};

export default App;


```

#### Use of `useRef()`:


- `useRef` can use to store data of API after calling. So that it doesn't need to recall every time.

```js
import React, { useRef } from "react";

const App = () => {
  const expensiveResultRef = useRef(null);
  const myDiv = useRef(null);

  const fetchData = async () => {
    const response = await fetch('https://dummyjson.com/products');
    expensiveResultRef.current = await response.json();
  };

  const ShowData = () => {
    myDiv.current.innerHTML = JSON.stringify(expensiveResultRef.current);
  };

  return (
    <div>
      <div ref={myDiv}></div>
      <button onClick={ShowData}>Show Data</button>
      <button onClick={fetchData}>Call API</button>
    </div>
  );
};

export default App;

```


#### Use of `useEffect()`:

- It behaves like constructor which means it can executed by itself.
- But number of execution depends on changing any value of the dependency array.
- When you pass an empty dependency array (`[]`), it runs **once** after the initial render.
- Dependencies (`[dep1, dep2]`): Run when specified dependencies change.

```js
import React, { useEffect, useState } from 'react';

const Index = () => {
  const [Data, SetData] = useState([]);

  useEffect(() => {
    (async () => {
      try {
        let response = await fetch('https://dummyjson.com/products/1');
        let result = await response.json();
        SetData(result);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    })();
  }, []);

  return (
    <div>
      {JSON.stringify(Data)}
    </div>
  );
};

export default Index;

```

```js
import React, { useEffect, useState } from 'react';

// Reusable API function
const fetchData = async (url) => {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    throw error;
  }
};

const Index = () => {
  const [data, setData] = useState(null); // Null initial value for better type handling
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadProduct = async () => {
      try {
        const result = await fetchData('https://dummyjson.com/products/1');
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false); // Ensure loading state is updated
      }
    };

    loadProduct();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <h1>Product Details</h1>
      <p><strong>ID:</strong> {data.id}</p>
      <p><strong>Name:</strong> {data.title}</p>
      <p><strong>Description:</strong> {data.description}</p>
      <p><strong>Price:</strong> ${data.price}</p>
    </div>
  );
};

export default Index;

```




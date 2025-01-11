#### Best use of `useSate()`

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
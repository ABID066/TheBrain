
#### Handling the form submission event

```js
const PostFormData = (event) => {
    event.preventDefault();
}
```
- Without reload the page the data of form will be submitted

#### Looping Using `map` in React

```js
const city = ['Dhaka', 'Khulna', 'Delhi', 'Kolkata'];

<ul>
    {city.map((item, i) => {
        return <li key={i.toString()}>{item}</li>
    })}
</ul>
```

```js
const cities = [  
{ id: 1, name: 'Dhaka' },  
{ id: 2, name: 'Khulna' },  
{ id: 3, name: 'Delhi' },  
{ id: 4, name: 'Kolkata' }  
];

<ul>  
    {cities.map((city) => {  
        return <li key={city.id}>{city.name}</li>  
    })}  
</ul>
```

#### Passing Props

- **Parent Component (`App.jsx`)**
```js
import Header from "./component/Header.jsx";
import Hero from "./component/Hero.jsx";
import ContactForm from "./component/ContactForm.jsx";
import Footer from "./component/Footer.jsx";

const App = () => {
    const itemsObj = {
        name: "md abid",
        age: 27,
        city: "Khulna"
    };

    const btnClick = () => {
        alert("sAY HELLO!! people");
    };

    return (
        <div>
            <Header title="Passing props in the name of title" />
            <Hero items={itemsObj} childBtnClick={btnClick} />
            <ContactForm />
            <Footer />
        </div>
    );
};

export default App;
```


- **Child Component (`Header.jsx`)**
```js
const Header = (props) => {
    return (
        <div>
            <h1>{props.title}</h1>
            <ul>
                <li>Home</li>
                <li>Others</li>
                <li>fgds</li>
                <li>fgfd</li>
            </ul>
        </div>
    );
};

export default Header;
```


- **Child Component (`Hero.jsx`)**:
```js
const status= true
const Hero = (props) => {
    return (
        <div>
            <img src="https://cdn.ostad.app/course/photo/2022-10-31T06-20-24.263Z-12.jpg" />
            {(() => {
                if (props.status)
                    return <button onClick={props.childBtnClick}>lOGoUT FORM HERO</button>
                else
                    return <button onClick={props.childBtnClick}>lOGin to HERO</button>
            })()}
            <ul>
                <li>Name: {props.items.name}</li>
                <li>Age: {props.items.age}</li>
                <li>City: {props.items.city}</li>
            </ul>
        </div>
    );
};

export default Hero;
```



# React Hook(useRef, useState, useEffect), Router DOM, Link & NavLink for Router, Browser Router VS HashRouter, Passing Parameter Via Naviagation

**Everything will be in Module 16**
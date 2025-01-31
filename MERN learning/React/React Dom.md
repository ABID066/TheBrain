`npm i react-router-dom`


#### BrowserRouter, Routes, Route

```js
<BrowserRouter>  
    <Routes>        
	    <Route path="/" element={<HomePage/>} />  
        <Route path="/byCategory/:categoryID" element={<PostByCategory/>} />  
        <Route path="/details/:postID" element={<DetailsPage/>} />  
    </Routes>
</BrowserRouter>
```

- passing value through parameters using router
```js
const DetailsPage = () => {  
    let {postID} = useParams();

	............
	............
	............
    }
```


- `HashRouter` does not use the HTML5 History API and instead relies on the hash part of the URL to handle routing.` BrowserRouter` uses the HTML5 History API for navigation.



#### NavLink, Link

```js
categories.map((item, index) => {  
    return <li key={index.toString()}><NavLink  to={"/byCategory/" + item["id"]}>{item["name"]}</NavLink></li>  
})
```

- `NavLink` is a special kind of `Link` that knows whether or not it is "active" or "pending"


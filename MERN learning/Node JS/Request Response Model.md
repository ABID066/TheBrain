

##### This is how you can start to know about the model

```js
var http = require('http');

var server = http.createServer(function (req, res) {

   // res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end("Hello world");

});

server.listen(5050);

console.log("Server is running on port 5050");
```




##### By using the URL we see different content

```JS
var http = require('http');
var server = http.createServer(function (req, res) {

    if(req.url=="/"){
        res.writeHead(200,{'Content-Type':'text/html'})
        res.write('<h1>This is Home Page</h1>')
        res.end();
    }
    else if(req.url=="/about"){
        res.writeHead(200,{'Content-Type':'text/html'})
        res.write('<h1>This is About Page</h1>')
        res.end();
    }
    else if (req.url=="/contact") {
	    res.writeHead(200,{'Content-Type':'text/html'})
        res.write('<h1>This is Contact Page</h1>')
        res.end();
    }
});
server.listen(5050);
console.log("Server is running on port 5050");
```






##### Split URL to get URL path as Object Name

```js
var http = require('http');
var URL= require('url')
var server = http.createServer(function (req, res) {

    var myURL="https://www.rabbitholebd.com/seeall?category=10&title=Drama";
    
    var myURLObj=URL.parse(myURL,true);
    
    var myHostName=myURLObj.host;
    var myPathName=myURLObj.pathname;
    var mySearchName=myURLObj.search;

  

    res.writeHead(200,{'Content-Type':'text/html'})
	res.write(myHostName);

    //res.write(myPathNameName);myPathNameName is not defined
    //res.write(mySearchName);
    res.end();
});

server.listen(5000);
console.log("Server is running on port 5000");
```




#MERN
#Node_JS

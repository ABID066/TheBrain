##### The index file
```js
var fs= require('fs');
var http=require('http');
var server=http.createServer(function(req,res)

{
    if(req.url=="/")
    {
        let data = fs.readFileSync("home.html","utf8");
        res.end(data);
    }
    else if(req.url=="/About")
    {
        let data = fs.readFileSync("about.html","utf8");
        res.end(data);
    }
    else if(req.url=="/Contact")
    {
        let data = fs.readFileSync("contact.html","utf8");
        res.end(data);
    }
    else if(req.url=="/Terms")
    {
        let data = fs.readFileSync("terms.html","utf8");
        res.end(data);
    }
});
server.listen(8080);
console.log("Server is running.....");
```

##### The Home Page
```html
<!DOCTYPE html>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
</head>
<body>
    <a href="/">Home</a><br>
    <a href="/Contact">Contact page</a><br>
    <a href="/About">About PAGE</a><br>
    <a href="/Terms">Terms</a><br>
    <hr>
    <h1>This is Home PAGE</h1>
</body>
</html>
```

##### The About Page
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About Page</title>
</head>
<body>
    <a href="/">Home</a><br>
    <a href="/Contact">Contact page</a><br>
    <a href="/About">About PAGE</a><br>
    <a href="/Terms">Terms</a><br>
    <hr>
    <h1>This is About PAGE</h1>
</body>
</html>
```

##### The Contact Page
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contact Page</title>
</head>
<body>
    <a href="/">Home</a><br>
    <a href="/Contact">Contact page</a><br>
    <a href="/About">About PAGE</a><br>
    <a href="/Terms">Terms</a><br>
    <hr>
    <h1>This is Contact PAGE</h1>
</body>
</html>
```

##### The Terms Page
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Title</title>
</head>
<body>
    <a href="/">Home</a><br>
    <a href="/Contact">Contact page</a><br>
    <a href="/About">About PAGE</a><br>
    <a href="/Terms">Terms</a><br>
    <hr>
    <h1>This is Term PAGE</h1>
</body>
</html>
```


#Node_JS
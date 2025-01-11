```js
var fs = require("fs");

var http = require('http')

  
  

var server=http.createServer(function (req,res){

    if(req.url=="/"){

  
        /*
        //readFile

	        let data=fs.readFileSync('home.html')
            res.writeHead(200,{'Content-Type': 'text/html'});
            res.write(data);
            res.end();

		//writeFile
        let error= fs.writeFileSync("demo.txt","Welcome to Node js as Sync And Override");
        if(error){
            res.writeHead(200,{"Content-Type":"text/html"});
            res.writeFile("File write failed");
            res.end();
        }
        else{
            res.writeHead(200,{'Content-Type':'text/html'});
            res.write("File write success");
	        res.end();

        }
        

		//renameFile
        let error=fs.renameSync("demo.txt","demoNew.txt")

        if(error){
            res.writeHead(200,{"Content-Type":"text/html"});
            res.writeFile("File rename fail");
            res.end();
        }
        else{
            res.writeHead(200,{'Content-Type':'text/html'});
            res.write("File rename success");
            res.end();
        }



		//deleteFile
       let error= fs.unlinkSync("demoNew.txt")

        if(error){
            res.writeHead(200,{"Content-Type":"text/html"});
            res.writeFile("File delete failed");
            res.end();
        }
        else{
            res.writeHead(200,{'Content-Type':'text/html'});
            res.write("File delete success");
            res.end();
        }

     */
		//checkIfItExist
        let result= fs.existsSync("home1.html");

        if(result)
	        res.end("True")
        else
	        res.end("False")  
    }
});

server.listen(3030);
console.log("Server is Running......")
```

##### The HTML file for `fs.exists` and `readFile`
```html
<!DOCTYPE html>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Home PAGE</title>
</head>
<body>
    <h1>THis is Home Page Sync</h1>
    <button onclick="AlerMe()">Click Me</button>
    <script>
        function AlerMe()
        {
            alert('HELLO')
        }
    </script>
</body>
</html>
```


#Node_JS
#sync

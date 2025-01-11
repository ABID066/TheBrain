##### By using JSON file you can run a Node JS file

```json
{
  "name": "practice",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node main.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

- To create the package.json file, you need to go the terminal and type `npm init --y`
- Then edit the JSON file, `"test": "echo \"Error: no test specified\" && exit 1"` After this line add `,"start": "node main.js"`
- To run the nodeJS File in terminal type `npm start`

- Without the help of the JSON file you can also run the NodeJS file. Just type `node main.js `


#JSON_Node_JS
#JSON
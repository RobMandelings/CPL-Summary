
```javascript
async function asyncExample() {  
    try {  
        let content = await readFile("hello.txt");  
        console.log("hello");
        return content + "_suffix";  
  
    } catch (err) {  
    }  
}

let p = readFile("hello.txt").then((content) => {
	console.log("hello");
	return content + "_suffix";  
})
return p;

// When content promise resolves, the 'rest' of the computation is resumed
async function callback(content) { // Continuation of await readFile("hello.txt")
        console.log("hello");
        return content + "_suffix";  
}
```
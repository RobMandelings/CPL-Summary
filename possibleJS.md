```javascript
function passwordCheckerb(email, password, cb) {
	return new Promise((resolve, reject) => {
		
	})
	const user = passwordChecker(email, password) :
	if (user) {
		cb (null, user);
	
	) else ‹
	
	setTineout (()
	
	cb 'User Not
	
	Found! ');
	
	}, 1006):
	}
}
```

## POSSIBLE QUESTION TYPE 1: CONVERT BETWEEN CB AND PROMISES
### PROMISE TO CALLBACK
```Javascript
function addPromise(a, b) {
  return Promise.resolve(a + b);
}
```

### CALLBACK TO PROMISES
```Javascript
function add(a, b, callback) {
  callback(null, a + b);
}
```

Convert it:

```Javascript
function addPromise(a, b) {
  return new Promise((resolve, reject) => {
    add(a, b, (error, result) => {
      if (error) {
        reject(error);
      } else {
        resolve(result);
      }
    });
  });
}
```

Now you can use:

```Javascript
addPromise(2, 3).then(result => {
  console.log(result);
});
```

## POSSIBLE QUESTION TYPE 2: What is the output order of the following code snippets involving asynchronous JavaScript?
Question 1: setTimeout vs sync code
```Javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```


What is the output order?

```Javascript
Question 2: Promise .then()
console.log("A");
```

```Javascript
Promise.resolve()
  .then(() => {
    console.log("B");
  });

console.log("C");
```

What is the output order?

Question 3: setTimeout vs Promise
```Javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```


What is the output order?

Question 4: chained .then()
```Javascript
Promise.resolve()
  .then(() => {
    console.log("A");
  })
  .then(() => {
    console.log("B");
  });
```

console.log("C");


What is the output order?

Question 5: Promise rejection
```Javascript
Promise.reject("Error!")
  .then(() => {
    console.log("A");
  })
  .catch(() => {
    console.log("B");
  });

console.log("C");
```


What is the output order?

Question 6: async / await basics
```Javascript
async function test() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
}

test();
console.log("C");
```


What is the output order?

Question 7: await with setTimeout
```Javascript
async function test() {
  console.log("A");

  await new Promise(resolve => {
    setTimeout(() => {
      console.log("B");
      resolve();
    }, 0);
  });

  console.log("C");
```
}

```Javascript
test();
console.log("D");
```


What is the output order?

Question 8: catch returning a value
```Javascript
Promise.reject("Oops")
  .catch(() => {
    console.log("A");
    return "Recovered";
  })
  .then(value => {
    console.log("B");
  });

console.log("C")
```

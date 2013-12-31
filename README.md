# Promise

This Promise polyfill follows the current [Promises/A+ specification](https://github.com/promises-aplus/promises-spec).

## Usage

Creating a Promise is simple. We create a new **Promise** object.

```javascript
var promise = new Promise();
```

A Promise is immediately initialized with a **Resolver** function.

```javascript
var promise = new Promise(function () {});
```

A **Resolver** is given two arguments, **fulfill** and **reject**. They control the Promise.

```javascript
var promise = new Promise(function (fulfill, reject) {
	if (true) {
		// fulfill the promise because everything worked
		fulfill();
	} else {
		// reject the promise because something failed
		reject();
	}
});
```

From here, **then** is used to add functions which will not run until a Promise has been **resolved** or **rejected**. If a Promise has already been resolved, the additional functions will be run immediately (though asynchronously).

```javascript
var promise = new Promise(function (resolve) {
	if (true) {
		resolve('Put anything here.');
	} else {
		reject('Put a sad face here.');
	}
});

promise.then(
	// if resolved
	function (resolveValue) {
		console.log(resolveValue); // logs 'Put anything here.'
	},
	// if rejected
	function (rejectValue) {
		console.log(resolveValue); // logs 'Put a sad face here.'
	}
);
```

Promises are extremely useful for simplifying your scripts and applications. Here is an example of a Promise-based function that simplifies **XMLHttpRequest**.

```javascript
function get(url) {
	return new Promise(function (resolve, reject) {
		var xhr = new XMLHttpRequest();

		xhr.open('GET', url);
	
		xhr.onload = function () {
			if (/200|302/.test(xhr.status)) {
				resolve(xhr.responseText);
			} else {
				reject(new Error(xhr.status));
			}
		};
	});
}

// USAGE: love.txt contains "All you need is love."
get('love.txt').then(function (responseText) {
	console.log(responseText); // logs "All you need is love."
});
```

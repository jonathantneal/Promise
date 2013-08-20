# Promises

This Promises polyfill follows the [WHATWG specification](http://dom.spec.whatwg.org/#promises).

## Usage

Creating a Promise is simple. We create a new **Promise** object.

```javascript
var promise = new Promise();
```

The Promise object is immediately initialized with a **function**.

```javascript
var promise = new Promise(function () {});
```

The function is given one parameter, which is referred to as a **Resolver**.

```javascript
var promise = new Promise(function (resolver) {});
```

The Resolver controls the Promise. It can **resolve** or **reject** the Promise.

```javascript
var promise = new Promise(function (resolver) {
	resolver.resolve('Put anything here.');
});
```

The above Promise is immediately initialized with a function which immediately resolves itself. In practice, Promises are usually used for things that take an uncertain amount of time.

Additional functions can be added to a Promise using a **then** method. The additional functions will not run until a Promise has been **resolved** or **rejected**. If a Promise has already been resolved, the additional functions will be run immediately, but asynchronously.

```javascript
var promise = new Promise(function (resolver) {
	resolver.resolve('Put anything here.');
});

promise.then(function (resolveValue) { // runs asynchronously
	console.log(resolveValue); // will log 'Put anything here.'
});
```

With that, a useful example of a Promise would be to create a simplified **XMLHttpRequest**. 

```javascript
function request(url) {
	return new Promise(function (resolver) {
		var request = new XMLHttpRequest();

		request.open('GET', url);
	
		request.onload = function () {
			if (/200|302/.test(request.status)) {
				resolver.resolve(request.response);
			} else {
				resolver.reject(new Error(request.status));
			}
		};
	});
}

// usage
// love.txt contains "All you need is love."
request('love.txt').then(function (response) {
	console.log(response); // logs "All you need is love."
});
```

The above example highlights how useful Promises are at simplifying your scripts and applications.

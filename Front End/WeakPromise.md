`WeakPromise` allows the developer to describe the asynchronous action as well as how to abort when cancelled. The WeakPromise will throw an error after invoking the abort handler.

```typescript
const weakPromise = new WeakPromise((resolve, reject) => {
	const timeoutId = window.setTimeout(resolve, 1000);

	function abortHandler(){
		window.clearTimeout(timeoutId);
	}
	
	return abortHandler;
});

weakPromise.catch((error)=>{
	console.log(error.message); // Cancelled
});

///...
weakProimse.cancel(new Error("Cancelled"));
```

Here is another example with the `fetch` api.

```typescript
function abortableFetch(request: RequestInfo, options: RequestInit) {
	const controller = new AbortController();
	const { signal } = controller;
	
	return new WeakPromise((resolve, reject) => {
		fetch(request, { ...options, signal })
		    .then(resolve)
			.catch(reject);
    
	    return () => {
	      signal.abort();
	    };
	});
}

const weakPromise = abortableFetch("https://google.com");

window.setTimeout(function(){
	weakPromise.cancel(new Error("Timedout"));
}, 1000);
```

WeakPromises are very useful with UI. For example, when a user navigates away from a loading table. If using react, a developer could cancel the WeakPromise within an unmounted callback. 

### Pros
Allows the developer to describe how to clean up if the weak promise is cancelled.

### Cons
The developer will need to consider how to handle cancellation errors.
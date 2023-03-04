Promises allow developers to be notified when the value has been retrieved by the action given to the contructor. This is the  `push` version of a variable.  

```typescript
const promise = new Promise((resolve, reject) => {
	window.setTimeout(resolve, 1000);
});

promise.then(() => {
	console.log("Finished");
});
```

### Pros
Encapulates all the complexity of asynchronous values.

### Cons
Does not support cancellation. See [[WeakPromise]] for cancellation support.

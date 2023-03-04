AsyncActionRunner enables developers to run actions and be informed with its status, value and error. 

Here is an example of status changes.
```typescript
const asyncActionRunner = new AsyncActionRunner([]);

asyncActionRunner.status.onChange((status)=>{
	console.log(status); // pending, success
});

asyncActionRunner.dispatch(()=>Promise.resolve([1,2,3]));
```

You can use [[WeakPromise]] so the action can be cancelled or just a regular [[Promise]].

If you are interested in the value that comes back from a single action use execute.
```typescript
const asyncActionRunner = new AsyncActionRunner([]);

asyncActionRunner.execute(()=>WeakPromise.resolve([1,2,3]))
	.catch(()=>{
		// Don't forget to catch. 
		// This will throw an error in the console if you do not catch this.
	})
	.then(()=>{});
```

You can run many actions on one runner. If you start another action on a pending action the runner will cancel the previous action and start the new one. 

```typescript
const asyncActionRunner = new AsyncActionRunner([]);

asyncActionRunner.status.onChange((status)=>{
	console.log(status); // pending, success, pending, success, pending, success
});

asyncActionrunner.execute(()=>WeakPromise.resolve([1,2,3]).then(()=>{
	return asyncActionRunner.execute(()=>WeakPromise.resolve([1,2,3,4]);
}).then(()=>{
	return asyncActionRunner.execute(()=>WeakPromise.resolve([1,2,3,4, 5]);
});
```

The best way to use runners is to fetch data from a server and be informed of its status.

```typescript
interface Api {
	getItems: WeakPromise<string[]> 
}

class Presenter {
	private _api: Api
	private _itemsRunner: AsyncActionRunner<number[]>;

	get itemsBroadcast(): ReadonlyAsyncActionRunner<number[]>{
		return this._itemsRunner.broadcast;
	}

	constructor(api: Api){
		this._api = api;
		this._itemsRunner = new AsyncActionRunner([])
	}
	
	loadItems(){
		this._itemsRunner.dispatch(()=>this._api.getItems());
	}
}

const presenter = new Presenter();

presenter.itemsBroadcast.onChange((items)=>{
	console.log(items) // [1,2,3]
});

presenter.itemsBroadcast.status.onChange((status)=>{
	console.log(status); // pending, success
});
```
This combines the value of a push and pull system. It stores a current value as well as notifies the developer if it changes. 

```typescript
const observableValue = new ObservableValue("John");

observableValue.onChange((newValue)=>{
   console.log(newValue); // Jane, Joe
});

observableValue.setValue("Jane"); 
observableValue.setValue("Joe");
```

ObservableValues can store an error along side a value. This means you can have an error and a value. A good example of this dilemma is with input fields, we need to save the current value as well as an error if it isn't valid. 

```typescript
const observableValue = new ObservableValue("John");

observableValue.onError((error)=>{
   console.log(error.message); // Invalid Name
});

observableValue.setValue("Jane");
observableValue.setError(new Error("Invalid Name")); 
```

### Unsubscribe
```
```
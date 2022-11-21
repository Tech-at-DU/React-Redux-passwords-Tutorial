# React Redux Local Storage

# Local Storage

At this stage your app should keep track of passwords but the list is lost when then app is refreshed. That's because all of the data is stored in memory. The browser gives an easy way for web sites to store data locally on a users computer: localstorage. 

Localstorage stores key value pairs that are accessible to we site by it's domain. Here are a couple facts about localstorage: 

- local storage is stores data locally and not in the cloud. You will have access to the data on the computer where it was saved. 
- Local storage stores access data by the domain. That means that websites that are loaded from another domain will not have access to the data stored by a app running on a different domain. 
- Local storage is saved for ever but can be cleared by clearing the browsers cahce. 
- Local storage stores key value pairs and values are stored as strings. This limits what can be stored. 

Local storage is not meant to be secure. Other web sites should not be able to read data stored by your site, but anyone sitting at your computer can inspect the cahce. 

Sounds like we may have pivot on the this product! But more on this at the end of the tutorial...

## `localStorage`

`localStorage` has two methods `getItem` and `setItem`. The first
retrieves data saved to `locaLstorage`, the second saves data to 
the local store. 

### `localStorage.setItem()`

Set an item with a key: 

```js
localStorage.setItem('key', 'some value')
```

### `localStorage.getItem()`

Get the value stored with the key: 

```JS
localStorage.getItem('key')
```

## `JSON.parse` and `JSON.stringify`
 
JavaScript Objects and Arrays can be converted to Strings in JSON format.

You can convert an Object or Array to a JSON string with: 

```JS
JSON.stringify(obj)
```

and parse a JSON back into a JavaScript object or array with: 

```JS
JSON.parse(json)
```

## Add local storage to the app

Add a new file: `src/app/persistState.js`.

```JavaScript
const PSSWRDZ_STATE = "PSSWRDZ_STATE"

// Load State
export const loadState = () => {
  try {
    const serializedState = localStorage.getItem(PSSWRDZ_STATE)
    if (serializedState === null) {
      return undefined
    }
    return JSON.parse(serializedState)
  } catch(err) {
    return undefined
  }
}

// Save State
export const saveState = (state) => {
  try {
    const serializedState = JSON.stringify(state)
    localStorage.setItem(PSSWRDZ_STATE, serializedState)
  } catch(err) {
    console.log("Error saving data:"+err)
  }
}
```

The first method loads state from the key defined on line 1. If there is nothing stored for that key it returns `undefined`. 

The second method takes `state` as a parameter serializes this into a JSON string and saves that to local storage with the key defined on line 1. Here we `try` the operation and catchany errors that occur. 

An error will occur if the data can not be serialized for some reason. 

Note! In both functions, when we call `setItem()` and `getItem()` you supply a string which is the key that was used to store the data. You're saving the same key that you're reading the data with, so get the same data you saved. 

With these two methods in place you can save and retrieve data to and from localstorage. 

## Saving data as JSON

Since the Redux store is a JavaScript object you can pass it to `saveState()` and save it with local storage as long as we convert the data to a JSON string. 

JSON is a string format that can be converted to and from a JavaScript object.

Note! The JSON format doesn't support all of the features of JS objects! values in JSON can be strings, numbers, boolean, null, array, or object. JSON Does not support functions! You can't save functions to JSON Objects. 

## Initializing and saving state

You want to restore the previous state with data saved from localstorage.

Edit app/store.js, import the `saveState` and `loadState` functions from `persistState.js` at the top: 
```JavaScript
import { saveState, loadState } from './persistState';
```

Next, edit the store where you called `configureStore`:

```JS
export const store = configureStore({
  reducer: {
		passwords: passwordsReducer
	},
	preloadedState: loadState() 
})
```

I added a new property: `preloadState`. This allows to set the default state of the store when it is initialized. Here you called `loadState` which should load the data and return an object or return `undefined` if there is nothing to load. 

Now add the following:

```JS
store.subscribe(() => {
	saveState(store.getState());
})
```

Here you are subscribing to the store. This gives us a hook that is triggered when the store is updated. Here you supply a callback for the store to call each time it updates. 

In the callback you're calling `saveState` and passing the state as an argument! Notice that you call `getState` on the store to get the state object that has the data that was stored. 

## Resouces 

- https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage
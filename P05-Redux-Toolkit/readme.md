# Implement Redux with Redux toolkit

Add Redux toolkit to an existing project 

## Installation

Install dependencies.

```bash
npm install @reduxjs/toolkit react-redux
```

(You can also start a new project with Redux Toolkit using

Setup

`npx create-react-app my-app --template redux`)

Create a file named `src/app/store.js`

```JS
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```

This file defines your Redux store. This is where the application state is defined and updated. 

Modify `src/index.js`

```JS
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { store } from './app/store'
import { Provider } from 'react-redux'
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

Your components need to access the store. You'll do that by passing it through the `Provider` component, it will be accessible to all descendants of this component. 

## Creating Slices, exporting Actions, and Reducers

Create a new folder `src/features`

For each slice of state you define you'll add a folder and a file to manage it. A slice is one value of the Application state. For this Application the slice will be `passwords`, it will be a list/array of passwords.

Think of state as an Object and a slice being one property on that object. 

Create a new folder for the passwords slice: `features/passwords`.

Now create a file that will define the actions and reducers for this slice: `features/passwords/passwordsSlice.js`

Add the following to `passwordsSlice.js`:

```JS
import { createSlice } from '@reduxjs/toolkit' // 1

// 2
const initialState = {
  value: [{ password: 'hello', name: 'test'}],
}

// 3
export const passwordsSlice = createSlice({
	name: 'passwords',
	initialState,
	reducers: {
		addPassword: (state, action) => {
			state.value.push(action.payload)
		}
	}
})

export const { addPassword } = passwordsSlice.actions
export default passwordsSlice.reducer
```

This block of code will be explained in the next section. 

**What's an action?**

An action is a "message" that your application can send that will trigger a change in the application state. 

In practical terms, an action is a function you can call that will trigger a change in the application state. An action might take arguments, these might be new values to store in the Redux store. 

**What's a reducer?** 

A reducer is a function that handles changes to the application state. 

A reducer receives an action and uses that action to decide how state will be changed. 

Update `app/store.js`

```JS
import { configureStore } from '@reduxjs/toolkit'
import passwordsReducer from '../features/passwords/passwordsSlice'

export const store = configureStore({
  reducer: {
    passwords: passwordsReducer
  },
})
```

With this in place, our application can store and update the list of passwords. It will not make changes to the list yet.

## Testing your work

If your app is compiling without error it should be working even though it won't be doing anything new you can see, yet! In the next step, you'll be able to effects of all of the work you've done here. 

## Resources

-

Next: [Actions and Reducers](../P06-Actions-and-Reducers)


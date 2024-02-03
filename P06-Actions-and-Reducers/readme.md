# Redux State 

Redux stores state as a JS object. It divides state into slices. Imagine each slice as a property/key stored on the state object. You can create as many slices as you need. The value for a slice can be any type. 

# Actions and Reducers

Actions and reducers are how Redux handles changes to the application state. You send an action from any component to the dispatcher. The action is received by a reducer that makes the change to the state. 

**In redux the only way to make a change to state is by sending an action.** 

Think about that last sentence for a moment. Unlike other systems you may have created where state can be changed from anywhere, in redux state can only be changed by an action. This part of the reason that redux exists, and why it is called a "redictable state container". 

## Adding reducer and actions

Let's walk through the code you added in the previous step. 

```JS
import { createSlice } from '@reduxjs/toolkit' // 1. import createSlice

// 2. Define initial state for this slice
// This slice will store an array of objects
const initialState = {
  value: [{ password: 'hello', name: 'test'}],
}

// 3. Define the passwordsSlice
// This includes both the action and the reducer!
export const passwordsSlice = createSlice({
  // the name of the slice
	name: 'passwords',
  // Initial value of this slice
	initialState,
  // Reducers and actions
	reducers: {
		addPassword: (state, action) => {
			state.value.push(action.payload)
		}
	}
})

// 4. Export the actions and reducers
export const { addPassword } = passwordsSlice.actions
export default passwordsSlice.reducer
```

1. Import `createSlice`, you'll use this to create a new slice.
2. Every slice needs to define it's initial value. Notice that the inisital value is added to the options object when creating the slice. You could set this there but it was easier to define the vairable in this case. 
3. Create and export the slice. Use `createSlice` to create the slice, pass it an options object that includes the `name` of the slice, `initialState`, and `reducers` and actions. 
    - A reducer is a function that will make a change to state. Here the reducer is the function assigned to the property `addPassword`. This function adds a new password to `state.value`, this is where the change to state happens. A reducer function always takes `state` and `action` as parameters! The action here is an o bject with a property `patload` which is data that comes from where the action originates. (I'll point it out when we get there!)
    - The action is the property that holds this reducer function, `addPassword` in this example.
4. Last you need export to actions and reducers. Notice here you exported `addPassword` (action) and `passwordSlice.reducer` (reducer).

## Working with State 

One of the main advantages to using redux is that it allows you to access and update your application state from anywhere without having to pass values with props. 

- To access state from any component you can use the `useSelector` hook.
- To update state from any component you can use the `useDispatch` hook and one of your actions. (Remember, you can only change state with an action!)
- When the redux state is updated and components that use values from state will render and update their view. 

## Adding new passwords

To add a new password you need to send an action with the `useDispatch` hook. You'll do this by importing the actions from your slice and calling the action from inside one of your components. 

Edit `src/Password.js`, import `useDisptach`, and an action from your slice at the top:

```JS
import { useDispatch } from 'react-redux'
import { addPassword } from './features/passwords/passwordsSlice'

...
```

At the top of the component function call `useDispatch` to get the dispatcher: 

```JS
...

function Password() {
 const dispatch = useDispatch()

 ...
```

Make a button that will save the current password to the Redux store. 

The arguments you provide to the action are attached to the payload of the action, look for payload on the [previous page](../P05-Redux-Toolkit#Creating-Slices-exporting-Actions-and-Reducers). 

We are passing an object with name and password, these should be values reflected in your inputs. Notice this matches what is shown in the `initialState` in the [previous page](../P05-Redux-Toolkit#Creating-Slices-exporting-Actions-and-Reducers). 

```JS
...

<button
  onClick={() => dispatch(addPassword({ name, password }))}
>Save</button>

...
```

**Study the code snippet until you recognize what this paragraph is describing before moving on!** Compare what is here with the code in `passwordsSlice.js` make some connections these are working together!

Testing your code at this stage should create and add new passwords to the list, as if by magic! **You won't see them yet!** You will display them in the next step!

## We need more than a string! 

What you are saving to the store looks like this: 

```JS
[
  {
    password: 'helloPassword',
    name: 'facebook'
  }, 
  { 
    password: 'p@$$w0rd',
    name: 'twitter'
  }, { 
    password: 'dgZAdTA9',
    name: 'google'
  }
]
```

Let's tour the Redux toolkit system and update our code along the way!

Update your initial state, edit `features/password/passwordsSlice.js`:

```JS
const initialState = {
  value: [
    { password: 'hello', name: 'test'}
  ],
}
```

**Alternately** you could do this, but the initial list of passwords would be empty: 

```JS
const initialState = {
  value: [],
}
```

## Displaying a list of passwords

To display the passwords list you'll use the `useSelector` hook. This hook takes a callback that returns state (redux store). This is object with properties for each slice. 

Create a new component: `src/PasswordsList.js`. Add the following: 

```JS
import { useSelector } from 'react-redux'

function PasswordsList() {
  const passwords = useSelector(state => state.passwords.value)

  return (
    <ul>
      {passwords.map(password => (
        <li>{password.name} - {password.password}</li>
      ))}
    </ul>
  )
}

export default PasswordsList
```

This component displays a list of passwords and their values.  

You used `useSelector()` to get `state` from the Redux store. This has all of our application state. You got the passwords slice and the value, which is the list of password objects. 

## Resources 

- https://redux-toolkit.js.org/tutorials/quick-start

Next: [Password Strength](../P07-Password-Strength)
Previous: [Redux Toolkit](../P05-Redux-Toolkit)

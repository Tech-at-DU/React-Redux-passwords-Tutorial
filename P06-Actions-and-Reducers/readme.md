# Redux State 

Redux stores state as a JS object. It divides state into slices. Imagine each slice as a property/key stored on the state object. You can have as many slices as you need. The value for a slice can be any type. 

# Actions and Reducers

Actions and reducers are how Redux handles changes to the application state. You send an action from any component. The action is received by a reducer that makes the change to the state. 

**In redux the only way to make a change to state is by sending an action.** 

Let's walk through the code you added in the previous step. 

## Adding reducer and actions

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
2. Every slice needs to define it's initial value.
3. Create and export the slice. Use `createSlice` to create the slice, pass it an object that includes the name of the slice, initial state, and reducers and actions. 
  - A reducer is a function that will make a change to state. Here the reducer is the function assigned to the key `addPassword`. This function adds a new password to `state.value`, this is where the change to state happens. A reducer function always takes `state` and `action` as parameters!
  - The action is the property that holds this reducer function, `addPassword` in this example.
4. Last you need to actions and reducers. Notice here you exported `addPassword` and `passwordSlice.reducer`.

Redux Toolkit uses the term "slice" to represent a piece of state in the store. A slice has actions and reducers. A slice handles one of the values on state. The slice includes an action and a reducer. The action is used to initiate a change to the slice and the reducer receives the action and makes the change. InitialState sets the initial value for a slice. 

Notice that you exported "addPassword" and "passwordSlice.reducer" at the bottom. You will be using these in other parts of the application. 

Here you added the "passwords" slice, "addPassword" is the action, it's a function you can call, this function is the reducer. A reducer always takes state and an action and makes chanegs to state in its code block.  

For this application there is a single reducer: `addPassword`. The function takes the name and password as strings, it creates a new password object and adds to the list of passwords that are stored on state. 

## Adding new passwords

To add a new password you need to send an action. You'll do this by importing the actions from your slice and calling the action from inside one of your components. 

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

Make a button that will save the current password to the Redux store. You can place this button at the JSX block returned from the component. 

```JS
...

<button
  onClick={() => dispatch(addPassword(password))}
>Save</button>

...
```

Notice here you are handling the button click by calling the `addPassword()` function and passing the `password` as an argument. The value returned from this is passed to the `dispatch()` function as an argument. **Study the code snippet until you recognize what this paragraph is describing before moving on!**

Testing your code at this stage should create and add new passwords to the list, as if by magic! Notice there is no direct connection between the `Password` component and the `PasswordsList` components!

Your data is being passed to Redux where the reducer you wrote, as part of the `PasswordsSlice`. 

When the store updates you're components are notified and they update. When updating the `PasswordsList` component gets the list of passwords from the store with `useSelector()`, which returns `state`. 

**Read those last three paragraphs again, maybe three times!** 

## We need more than a string! 

Saving the password string is not very useful. It would be better to also save a name or label along with the password string. 

To do this you'll need to store objects instead of strings in the passwords array. 

Currently `passwords` looks like: 

```JS
['helloPassword', 'p@$$w0rd', 'dgZAdTA9']
```

You need it to look like this: 

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

Create a new component: `src/PasswordsList.js`, and add the following: 

```JS
import { useSelector } from 'react-redux'

function PasswordsList() {
  const passwords = useSelector(state => state.passwords.value)

  return (
    <ul>
      {passwords.map(password => (
        <li>{password}</li>
      ))}
    </ul>
  )
}

export default PasswordsList
```

This component displays everything in the passwords list stored in our Redux store. 

It uses `useSelector()` to get `state` from the Redux store. This has all of our application state. Since this component is only concerned with the `passwords` list we use `state.passwords.value` to get that list. 

The rest of the component is standard React and displays the list as `<ul>` and `<li>`. 

You can test this by updating the `initialState` in `features/passwords/passwordsSlice.js`. Try it for yourself. Add another string here: 

```JS
const initialState = {
 value: ['helloPassword', 'ABCDEF'],
}
```

You should see the new item displayed on your page. 

## Test your work

Edit the `src/PasswordsList.js`:

```JS
passwords.map((password, i) => (
  <li key={`${i}-item`}>{password.name} : {password.password}</li>
))
```

Since each password is now an object with password and name properties we need to adjust how we get these values. 

Now edit `src/Password.js`. See if you can make these changes on your own! You need to do the following: 

- Add a new input field so we can enter a name
- Declare a state variable for the name value, use `useState()`
- Use the controlled component pattern, set the value of the new input to the value of the new state variable, and update the new state variable in the `onChange` handler. 
- In the call to `addPassword()` change the argument from a string to an object with name and password properties. 


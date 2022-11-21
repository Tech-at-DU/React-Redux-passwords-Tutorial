# Actions and Reducers

Actions and reducers are how Redux handles changes to application state. You send an action from any component. The action is received by a reducer that makes the change to state. 

**In redux no change to the store can be made without sending an action to the dispatcher!** 

There are two things that you need to do to see your application state working. You need to display the contents of the passwords list that is stored in the store. Second you need to send actions that will add new passwords to the list. 

## Displaying a list of passwords

Create a new component: `src/PasswordsList.js`, add the following: 

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

It uses `useSelector()` to get `state` from the Redux store. This has all of out application state. Since this component is only concerned with the `passwords` list we use `state.passwords.value` to get that list. 

The rest of the component is standard React and displays the list as `<ul>` and `<li>`. 

You can test this by updating the `initialState` in `features/passwords/passwordsSlice.js`. Try it for yourself. Add another string here: 

```JS
const initialState = {
  value: ['helloPassword', 'ABCDEF'],
}
```

You should see the new item displayed on your page. 

## Adding new passwords

To add a new password you need to send an action. You'll do this by importing the actions fromyour slice and calling the action from inside one of your components. 

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

Testing your code at this stage should create add new passwords to the list, as if by magic! Notice there is no direct connection between the `Password` component and the `PasswordsList` components!

Your data is being passes to Redux where the reducer you wrote, as part of the `PasswordsSlice`. 

When the store updates you're components are notified and they update. When updating the `PasswordsList` component gets the list of passwords from the store with `useSelector()`, which returns `state`. 

**Read those last three paragraphs again, maybe three times!** 

## We need more than a string! 

Saving the password string is not very useful. It would be better to also save a name or label along with the password string. 

To do this you'll need to store objects instead of strings to the passwords array. 

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


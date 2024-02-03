# Adding names and passwords

To add the name and password to your redux store/state you you'll use the `useDispatch` hook along with an action. 

Edit `src/Password.js`, import `useDispatch` and `addPassword` at the top. 

```JS
import { useDispatch } from 'react-redux'
import { addPassword } from './features/passwords/passwordsSlice'
```

Now declare a new state variable name. 

```JS
...

const [name, setName] = useState('')

...
```

Add a new input element that displays and sets name: 

```JS
...

<input 
  type="text"
  onChange={(e) => setName(e.target.value)}
  value={name}
/>

...
```

Now edit the `onClick` handler so that we are sending objects with name and password to the store: 

```JS
...
onClick={() => dispatch(addPassword({ name, password }))}
...
```

test your work. You should be able to enter both name and password. Clicking the save button should add an object containing the name and password to the store and display it below. 

## Test your work

At this stage entering a name and password into the fields and clicking save should add the new object to the store and cause the list component to update and render the new element. 

Refreshing the page should start the app over again and delete all of your data. This happens because the store is stored in RAM. We'd like the data to be saved and loaded again when the app is reloaded. To do that you'll use `localstorage` in an upcoming step.

## Reflect on your work

It might seem like you have done a lot of work, maybe more than you could have. For the extra work of setting up Redux Toolkit you can access state anywhere in your project by importing `useSelector`. Notice that you did not pass props anywhere! This means that you could move compopnents around and rearrange them without having to worry about props. This is a big advantage. 

Also, your state is all in one location. If you need to think about how state is modified you can look at your slice. The only changes that happen to state happen there. In another app you may have things that are changing state located all over your app. 

If you were to look at a new redux app and wanted to know how it managed state you need only exmamine the slices and these will tell you what changes can be made to state and the actions that you can send to make changes. 

Next: [Password Strength](../P08-Password-Strength)

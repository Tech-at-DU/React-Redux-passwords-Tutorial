# Adding names and passwords

Edit `src/Password.js`, declare a new state variable for name. 

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

At this stage enerting a name and password into the fields and clicking save should add the new object to the store and cause the list component to update and redner the new element. 

Refreshing the page should start the app over again and delete all of your data. This happens because the store stored in RAM. We'd like the data to be saved and loaded again when the app is reloaded. To do that you'll use localstorage in an upcoming step. 
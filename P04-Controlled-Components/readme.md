# What is a controlled Component? 

"Controlled Component" is a name used to describe form elements 
in React. Form elements require a special treatment due to the 
virtual DOM. 

https://reactjs.org/docs/forms.html#controlled-components

## The virtual DOM

The DOM is Document Object Model is the structure of nodes or elements created from the an HTML document. 

Traversing and modifying elements in the DOM, like updating the text/html content of an element is a slow operation in the browser. 

React solves this speed issue by creating a virtual DOM holds the structure of the document in JavaScript. Changes to the virtual DOM are much faster than making changes to DOM directly. 

When the DOM needs to be changed the React uses a diffing algorithm to identify which elements need to be changed, and applies changes to only those elements in the DOM. 

Using the virtual DOM introduces some problems. For example if you create a reference to an element in the DOM sooner or later that element will be replaced when React updates the component that renders it, and your reference is no longer valid. 

https://reactjs.org/docs/faq-internals.html

## The Controlled Component Pattern

Inputs (as in `<input type="text">`) hold the value that was entered and if these components are replaced when the virtual DOM renders a component the value entered will be lost. The controlled component pattern solves this problem by storing the value entered in a react state variable. 

Here's how the controlled component pattern soplves this problem. Imagine an input element (input tag) inside of one your components. As you enter text, the following steps occur in your software. 

1. When the value in the input element is changed this triggers an `onChange` event. 
2. `onChange` handler sets a value in state on the component. 
3. Changes to state trigger the component to render. 
4. When the component renders React replces the elements in the component with new elements. This means any text that was entered in an input will be lost. 
5. To make the new replacement input display the entered text we set the value of the input to the state variable. 

Try it out. Add the following to the `Password` Component. 

```JSX
function Password() {
  const [password, setPassword] = useState('p@$$w0rd')
 
  return (
    <div>
      <input 
        type="text"
        onChange={(e) => setPassword(e.target.value)}
        value={password}
       />
       <div>
         <button onClick={(e) => {
           generatePassword()
         }}>Generate</button>
       </div>
      </div>
   )
}
```

Make this change to your Password component. 

Notice the input element has an onChange function. When a change occurs it sets the `password` state variable to the value that was entered into the input. 

`onChange={(e) => setPassword(e.target.value)}}`

When the component is redrawn the value of the input is set to the `password` state. 

`value={password}`

You should use this pattern with all form elements! That includes: checkboxes, radio buttons, range sliders, and text area!

## Challenges 

**Challenge 1**

Modify the Password component so the password displays in a text input. use the Controlled Component Pattern. 

**Challenge 2**

Add a new text input. This will be used to store a name or description for the password. You will need to create another text input, and add another property to state. Use the Controlled Component Pattern again.

Besure to declare a new state variable for the name! 

### Stretch Challenges

**Stretch Challenge 1**
Add an input, range, or select element that let's us choose the length of the pasword. This sets the number of random characters that are generated. 

**Stretch Challenge 2**
Add a check box that indicates whether the password generated should contain hyphens between every 3rd character. e.g.

"6N1-kMH-d?i"

Hyphens should appear when the box is checked. And not when the box is unchecked e.g.

"6N1kMHd?i"

Use the Controlled Component Pattern again with the check box. 

**Stretch Challenge 2**
Use a select menu to choose one of three different password generators. 

You can use one of these patterns, or make up your own. 

- Letters (Only letters: AbC...)
- Letters + Numbers (Letters and numbers: a2B...)
- Letters + Numbers + Punctuation (Letter, numbers, and punctuation: a2#...)

Use the Controlled Component Pattern again withe select element. 

You will need to modify your password generation code to handle the 
system chosen from the menu. 

## Resources 

- https://reactjs.org/docs/forms.html#controlled-components

Next: [Redux Toolkit](../P05-Redux-Toolkit)
Previous: [Generating Passwords](../P03-Generating-Passwords)

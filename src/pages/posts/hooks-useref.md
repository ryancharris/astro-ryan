---
title: "Hooks Revisited: useRef"
publishDate: "2020-03-13"
published: true
layout: "../../layouts/BlogPost.astro"
description: "useRef can be used to directly interact with the browser's DOM or store and update values without causing re-renders like useState."
category: "react"
tags:
  - "hooks"
---

### What are refs?

If you read <a href="/posts/hooks-uselayouteffect">my last article</a>, about the differences between useEffect and useLayoutEffect, you may remember seeing some code snippets that looked like this:

```javascript
useEffect(() => {
  const greenSquare = document.querySelector(".App__square");
  greenSquare.style.transform = "translate(-50%, -50%)";
  greenSquare.style.left = "50%";
  greenSquare.style.top = "50%";
});

useLayoutEffect(() => {
  const greenSquare = document.querySelector(".App__square");
  greenSquare.style.transform = "translate(-50%, -50%)";
  greenSquare.style.left = "50%";
  greenSquare.style.top = "50%";
});
```

In these examples, we're directly accessing the DOM in order to select and manipulate an element (i.e. .App\_\_square), which is considered an [anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern) in React because it manages UI state via a [virtual DOM](https://reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom) and comparing it to the browser's version. Then, the framework handles the work of [reconciling](https://reactjs.org/docs/reconciliation.html) the two. However, there **are** cases where we need may need to break this rule. That's where refs come in.

While the React docs cite a few examples where using refs would be appropriate, including managing focus, triggering animations, and working with third party libraries, they also warn against overusing them.

> Avoid using refs for anything that can be done declaratively. [[React docs]](https://reactjs.org/docs/refs-and-the-dom.html#when-to-use-refs)

For a practical example of how to use refs in your React app, [checkout my previous article about rebuilding a search UI](https://ryanharris.dev/2019-10-27-redoing-search/) using refs and React Context. We'll also cover the ins and outs of Context in the next article in this series.

In the next section, we'll look more closely at the useRef hook and its syntax.

### Anatomy of useRef

> ...useRef is like a ???box??? that can hold a mutable value... [[React docs]](https://reactjs.org/docs/hooks-reference.html#useref)

The useRef hook only takes one argument: its initial value. This can be any valid JavaScript value or JSX element. Here are a few examples:

```javascript
// String value
const stringRef = useRef("initial value");

// Array value
const arrayRef = useRef([1, 2, 3]);

// Object value
const objectRef = useRef({
  firstName: "Ryan",
  lastName: "Harris",
});
```

Essentially, you can store any value in your ref and then access it via the ref's current field. For example, if we logged out the variables from the snippet above, we would see:

```javascript
console.log(stringRef);
// {
//   current: "initial value"
// }

console.log(arrayRef);
// {
//   current: [1, 2, 3]
// }

console.log(objectRef);
// {
//   current: {
//     firstName: 'Ryan',
//     lastName: 'Harris'
//   }
// }
```

As I mentioned in the intro, refs are primarily used for accessing the DOM. Below is an example of how you would define and use a ref in the context of a class component:

```javascript
class MyComponent extends React.Component {
  constructor() {
    super();
    this.inputRef = React.createRef();
  }

  render() {
    return (
      <div className="App">
        <input ref={this.inputRef} type="text" />
      </div>
    );
  }
}
```

To do the same exact thing using hooks, we would leverage useRef like you see in the snippet below:

```javascript
function MyComponent() {
  const inputRef = useRef(null);

  return (
    <div className="App">
      <input ref={inputRef} type="text" />
    </div>
  );
}
```

Hopefully, those examples clearly illustrated how to define a ref. Just remember: refs are a "reference" to a DOM element -- it's right in the name!

refs also have another less-known use case. Since a ref's value can be any JavaScript value, you can also use refs as basic data stores. Usually, you would use useState for something like that, however, there are time where you want to avoid uneccessary re-renders but cache a value. Updating values in state cause a re-render each time, whereas updating refs **do not cause the component to update**. This is a subtle, but important distinction.

### In practice

In the sections below, we'll walk through two examples that better illustrate how to use useRef both to access DOM elements and store values without causing our component to re-render.

#### Accessing DOM elements

For this example, I have built a small SearchInput component that uses the useRef hook in order to refer to the input element rendered by our component:

<iframe
  src="https://codesandbox.io/embed/serene-sunset-z80cl?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useRef (DOM)"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

In this specific case, our SearchInput component takes an autoFocus prop, which determines whether or not we want the input to be focused automatically on mount. In order to do this, we need to use a web API (i.e. .focus()) and thus need to be able to directly refer to the HTML element on the page.

To get this to work, the first thing we need to do is create a ref and assign it to our element:

```javascript
// This instantiates our ref
const inputRef = useRef(null);

// Inside our return, we point `inputRef` at our <input /> element
<input ref={inputRef} type="search" className="SearchInput__input" />;
```

Now, our inputRef is pointing at the search input, so if we were to log out inputRef.current, we would see our input:

```javascript
console.log(inputRef.current);
// <input type="search" class="SearchInput__input"></input>
```

With this wired up, we can now autofocus the input on mount, as well as add some styling to make our SearchInput component look more cohesive even though it is made up of multiple elements "under the hood". In order to handle the autofocus behavior, we need to use the useLayoutEffect hook to focus the input prior to the DOM painting.

_Note: For more info on when to use useLayoutEffect vs. useEffect, <a href="/posts/hooks-uselayouteffect">check out my previous article in this series</a>._

```javascript
useLayoutEffect(() => {
  if (autoFocus) {
    inputRef.current.focus();
    setFocused(true);
  }
}, [autoFocus]);
```

By calling inputRef.current.focus(), we are setting the input inside our component as the active element in the document. In addition, we're also updating our focused value stored in a useState hook in order to style our component.

```javascript
const focusCn = focused ? "SearchInput focused" : "SearchInput";
```

Finally, I have also added an event listener using <a href="/posts/hooks-useeffect">a useEffect hook</a> in order to update our focus state based on mouse clicks both inside and outside of our component. Essentially, when the user clicks inside SearchInput, we call .focus() and update our focused state to true. Alternatively, when the user clicks outside the component, we call .blur() and set focused to false.

```javascript
useEffect(() => {
  function handleClick(event) {
    if (event.target === inputRef.current) {
      inputRef.current.focus();
      setFocused(true);
    } else {
      inputRef.current.blur();
      setFocused(false);
    }
  }

  document.addEventListener("click", handleClick);
  return () => {
    document.removeEventListener("click", handleClick);
  };
});
```

While accessing DOM elements is a React anti-pattern (as discussed above), this example is a valid use case for refs because our goal requires the use of .focus(), which is only available to HTML elements.

#### Storing values without re-rendering

In this example, I want to illustrate the subtle difference between using useState and useRef to store values.

<iframe
  src="https://codesandbox.io/embed/fancy-hooks-cxw57?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useRef (Data)"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

Here, we have two sections that have buttons, which allow us to increment/decrement our refValue or stateValue, respectively. When the page initally loads, each section is assigned a random hex value as its background-color. From then on, you will see the colors change whenever our App component re-renders.

Since updating state values cause a re-render, you should see the stateValue number update every time you click on of the buttons; however, if you click on the buttons for our refValue, nothing happens. This is because **updating ref values does not cause a component to re-render**. To demonstrate that the refValue is in fact changing, I have added console.log statements to the onClick handlers for both buttons.

While incrementing or decrementing the refValue will not cause our UI to update with the proper numeric value, when you change the stateValue our refValue will update and its section will have a new background color. This is because our ref section is re-rendered when the state value is updated since the parent component App has to go through reconciliation to bring the virtual DOM and browser DOM into sync with one another. This can be a great strategy for avoiding uneccessary renders in your application and improving its performance!

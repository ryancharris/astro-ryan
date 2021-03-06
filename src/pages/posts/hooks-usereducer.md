---
title: "Hooks Revisited: useReducer"
publishDate: "2020-05-18"
published: true
layout: "../../layouts/BlogPost.astro"
description: "useReducer provides an alternate way to store component state beyond useState. It is especially useful for storing complex data structures and/or related values."
category: "react"
tags:
  - "hooks"
---

### An update on state

Throughout this series, all of the code snippets and sandboxes we've created have used useState to manage our component data. However, React offers us an additional hook to use for storing data: useReducer.

While useState allows us to store and set a single value, useReducer helps us work with more complex or structured data by allowing us to store and manipulate related values alongside one another.

### Anatomy of useReducer

Like useState, useReducer returns an array with two values:

- The current state
- A function used to update the state

```javascript
const [value, setValue] = useState(null);

const [state, dispatch] = useReducer(reducer, initialState);
```

The seReducerhook takes up to three arguments:

1. **Reducer function** -- This function describes how our state should be updated based on the action that was dispatched.

2. **Initial state** -- This value defines the hook's initial state and works similiarly to how we provide the useState hook a default value when instantiating it.

3. **Initialization function** -- This argument is optional and is useful for...

> ...calculating the initial state outside the reducer. This is also handy for resetting the state later in response to an action. ~ [[React docs](https://reactjs.org/docs/hooks-reference.html#lazy-initialization)]

### Difference from useState

To best illustrate the difference in how useReducer and useState update their state values, respectively, let's take a look at them side by side. The snippet below shows the code you'd need to use to instantiate and update a state value using both hooks:

```javascript
// useState
const [name, setName] = useState("");
setName("Ryan");
console.log(name); // 'Ryan'

// useReducer
const initialState = {
  name: "",
};

function reducer(state, action) {
  switch (action.type) {
    case "update-name":
      return {
        name: action.value,
      };
  }
}

const [state, dispatch] = useReducer(reducer, initialState);
dispatch({ type: "update-name", value: "Ryan" });
console.log(state.name); // 'Ryan'
```

The first difference here is that while useState is storing a string, useReducer's initial value is an object. In this case, it has a single key (i.e. name), however, we can always add more keys to the state as we build out our UI.

Secondly, while useState's setter function updates its value directly, useReducer dispatches an action. The reducer function then determines what type of action was fired and, subsequently, how to update its state.

_Note:_ If you haven't used it in the past, this is pretty much how [Redux](https://redux.js.org/) works.

### In practice

In the sandbox below, I've built a form for scheduling an appointment. Though there are multiple inputs with different types, all of the values are related to one another as they are in the same form.

<iframe
  src="https://codesandbox.io/embed/hooksusereducer-5rntg?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useReducer"
  allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr"
  sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

Instead of storing each input value in its own useState hook, we can store and manage all of the values in our form using a single useReducer. In this case, its state is an object with many keys, each representing a different value we want to store. Personally, this reminds me a bit of this.state in class components before we had hooks.

In App.js, we've defined our initial state like this:

```javascript
const blankForm = {
  name: "",
  email: "",
  date: "",
  time: "",
  feeling: "3",
};

const [formState, dispatch] = useReducer(reducer, blankForm);
```

Each of the fields in the blankForm object represents and stores the value for an associated input in our form. Since the initial state of email is an empty string, the e-mail input will be blank on render as it reads its value from useReducer's state:

```javascript
<input
  className="Form__input"
  name="email"
  type="email"
  value={formState.email}
/>
```

To make this work, we've also set our inputs' onChange handlers to dispatch specific actions in order to update the state. Here's what our e-mail input now looks like:

```javascript
<input
  className="Form__input"
  name="email"
  type="email"
  value={formState.email}
  onChange={(event) => {
    dispatch({ type: "setEmail", value: event.target.value });
  }}
/>
```

In the snippet above, we're specifically dispatching the setEmail action. Inside of our reducer function, the switch statement looks for the case that matches the action.type and executes its logic to update state:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case "setName":
      return {
        ...state,
        name: action.value,
      };
    case "setEmail":
      return {
        ...state,
        email: action.value,
      };
    case "setDate":
      return {
        ...state,
        date: action.value,
      };
    case "setTime":
      return {
        ...state,
        time: action.value,
      };
    case "setFeeling":
      return {
        ...state,
        feeling: action.value,
      };
    case "reset":
      return blankForm;
  }
}
```

For example, when setEmail is called the reducer returns a new object that contains all of the current state information, except it **also** updates the email field.

```javascript
return {
  ...state,
  email: action.value,
};
```

Finally, since our useReducer hook's state has now been updated, the component will re-render and the inputs all display their updated value from formState.

### Notes about performance

As my friend [Josef Aidt](https://josefaidt.dev/) pointed out while reviewing an early draft of this article, our use case for useReducer in the sandbox above has certain performance implications. Since each input's onChange function fires each time an input's value changes, we are actually causing our component to re-render on each key press. This is alright for demonstration purposes, but is something to be aware of when building production apps.

Two ways we could avoid this are:

- Adding a debounce to each input, so that we do not trigger a state update on each keypress.
- Storing our input values in refs instead of useReducer as changing the value of a ref does not cause our component to re-render (see my <a href="/posts/hooks-useref">useRef</a> article for more on this).

Now, go forth and be performant!

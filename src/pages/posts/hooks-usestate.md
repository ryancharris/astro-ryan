---
title: "Hooks Revisited: useState"
publishDate: "2020-02-17"
published: true
layout: "../../layouts/BlogPost.astro"
description: "As the name suggests, the useState hook is used for storing component state."
category: "react"
tags:
  - "hooks"
---

One of the first hooks you'll probably encounter is useState, which replaces the setState() function used to update state in class components. The big difference here, however, is that useState allows function components to have multiple state values as opposed to one monolithic object. The snippet below illustrates this idea (don't worry about the syntax as we'll be covering that below):

```javascript
// Class component state
class Cart extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      apples: 10,
      oranges: 25,
      peaches: 40,
    };
  }
}

// Function component using hooks
function Cart(props) {
  const [apples, setApples] = useState(10);
  const [oranges, setOranges] = useState(25);
  const [peaches, setPeaches] = useState(40);
}
```

Not too crazy, right? In this example, all of our values are numbers but the value stored in useState can be any JavaScript type, including:

- Strings
- Booleans
- Objects
- Arrays
- Numbers
- `null`

### Anatomy of useState

Let's take one of the previous examples and look at it a bit more closely.

```javascript
const [apples, setApples] = useState(10);
```

First, let's look at what is happening on the right side of this expression. Here, we have the value of 10 being passed as the only argument to our useState hook. This sets its initial value to 10.

On the left-hand side of the assignment, we are destructuring two values returned from useState as an array: apples and setApples. The first (i.e. apples), represents the current value of this state. In this case, the value would be 10.

The second value in the array (i.e. setApples) is a setter function that allows you to update the value of apples by calling setApples(200), which would update apples to be equal to 200.

While you can technically give these setter functions whatever name you want, the common convention is to prepend the value's name with set (ex. setApples).

### In practice

To better illustrate the mental model of having multiple states instead of one, I built a simplified version of an e-commerce cart.

The first instance is a function component using three useState hooks to manage the quantity values of how many apples, oranges and peaches are in the user's cart. The second is a class component that is still using setState().

As you can see, both components do the same things: keep track of how many of each item a user wants and increment/decrement that value based on button presses.

<iframe
  src="https://codesandbox.io/embed/hooksusestate-o64ve?autoresize=1&fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useState"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

Take a look at the code and see which one you prefer. While hooks may still be new to you, can you see any benefits from using them?

---
title: "Hooks Revisited: useEffect"
publishDate: "2020-02-26"
published: true
layout: "../../layouts/BlogPost.astro"
description: "useEffect is one of the most commonly used hooks and is used for things like fetching data, setting up event listeners and manipulating the DOM."
category: "react"
tags:
  - "hooks"
---

In <a to="/posts/hooks-usestate">my last article</a>, we learned about one of the most commonly used hooks, useState. This time around, we are going to look at another commonly used hook: useEffect:

> By using this Hook, you tell React that your component needs to do something after render. React will remember the function you passed (we'll refer to it as our "effect"), and call it later after performing DOM updates. [[React docs]](https://reactjs.org/docs/hooks-effect.html)

### What are effects?

Effects, a shorthand for "side effects", represent component operations or actions that cannot be done during the render phase. Examples of this can include:

- Fetching data from an API
- Setting up data subscriptions or document event listeners
- Manipulating the DOM

We can also further classify them into two categories:

1. Effects that require cleanup
2. Effects that don't

For example, if we attach an event listener to the document, we will want to remove it when the component unmounts as this will help with performance and avoid conflicting listeners. On the other hand, something like updating the document.title does not require any further work when the component unmounts.

To make cleaning up effects easy, the useEffect API allows you to optionally return a function from the hook, which does the work of removing listeners, subscriptions, etc. Previously, you would've needed to leverage both the componentDidMount and componentDidUnmount lifecycle methods to achieve this whereas useEffect allows us to take care of it all at once.

### Anatomy of useEffect

Now that we've talked about what useEffect does, let's take a look at the syntax:

```javascript
useEffect(() => {
  // 2. This function body is your effect
  window.addEventListener("resize", handleResize);

  return () => {
    // 1. Optionally clean up effects inside this function
    window.removeEventListener("resize", handleResize);
  };
}, []); // 3. Conditionally execute based on dependencies array
```

If this syntax looks a bit strange, don't worry. We'll break down each piece before moving on to some practical examples. Let's start with the optional cleanup function since we were just talking about it.

#### 1. Cleanup

Inside our effect, we can optionally return a function. This function will perform any cleanup work we want to happen when this component unmounts. In our example, we are removing the event listener from the window to make sure it doesn't keep listening/firing after the component is no longer in the DOM.

#### 2. The effect

The first argument useEffect takes is a function. This function **is your effect** and defines the work you want to do whenever the component mounts. In this case, we are simply adding an event listener to the window that executes the handleResize function on resize.

#### 3. Dependency array

The optional second argument in this example is what's known as the "dependency array". Essentially, leveraging this array allows you to control the conditional execution of the effect based upon changing prop or state values in the component. We will talk about this more in-depth in the next section.

### What is the dependency array?

> By default, effects run after every completed render, but you can choose to fire them only when certain values have changed. [[React docs]](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)

As I mentioned above, the dependency array is an optional secondary argument passed to the useEffect hook. Its purpose is to allow you to more easily control the execution of your effects based upon the values within you component. In class components, we would most likely need to use the componentDidUpdate lifecycle method to achieve the same results, which would've looked something like this:

```javascript
componentDidUpdate(prevProps, prevState) {
  if (prevState.cardTypes !== this.state.cardTypes) {
    // Your effect logic would live here
  }
}
```

Using the dependencies array we can do things like:

- Fire the effect everytime the component renders

```javascript
useEffect(() => {
  const cardTypes = fetchCardTypes();
  setCardTypes(cardTypes);
});
```

- Fire the effect only upon the first render

```javascript
useEffect(() => {
  const cardTypes = fetchCardTypes();
  setCardTypes(cardTypes);
}, []);
```

- Fire the effect only when certain prop or state values have changed

```javascript
useEffect(() => {
  const cardTypes = fetchCardTypes();
  setCardTypes(cardTypes);
}, [cards]);
```

One thing to note here is that while you can also use fstatements within your seEffecthooks to conditionally execute logic, **you cannot wrap hooks in fstatements**. Part of how React keeps effects predictable is to run them all upon render. Skipping effects like this is considered bad practice, so don't do it!

### In practice

In the sandbox below, I've create a small application that leverages seEffectin numerous ways in order to serve us information about the latest weather on Mars. Elon Musk, eat your heart out!

For simplicity's sake, I've created two components: ppand eatherDisplay The former handles fetching our data from the Nasa API and our application logic, whearas the latter simply displays the data we've passed down to it as props. Because of that, all of our seEffecthooks live inside of pp

As you will notice, we actually have **three** seEffecthooks inside of our component, which may seem a bit strange, but is the whole idea of hooks! This allows us to compartmentalize our component logic and more easily reason about the effects they trigger. In this example, our three hooks are doing the following:

- Hook #1 sets the title of our document **on every render** using the value of our aystate
- Hook #2 fetchs our API data **only on the first render** as we don't want to make be making continually API calls as the component updates
- Hook #3 parses the proper data object based on the value of ay\*_any time the value of ayor ata_ change

Each of these hooks is using a diffrent variation of the optional dependency array we discussed earlier. Look at the code a bit more closely -- do you know why each array looks the way it does?

<iframe
  src="https://codesandbox.io/embed/hooksuseeffect-gkejk?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useEffect"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

Don't worry if you're still a bit confused because learning to "think in hooks" can take some time. The best way to get more comfortable with them is to use them, so feel free to fork the sandbox above and play around with the code. As a good place to start, try removing the dependency array from Hook #2 completely. What happens?

With an empty dependency array, the effect makes a request for our API data on mount and this **only happens once**. Previously, we would componentDidUpdate and compare the components prevProps to it's current props and use that to determine if there was work to be done. Now, with useEffect, we no longer have to do this and can just define what prop values we care about and run the effect only when one of them changes. We'll talk more about this more later in the series.

If we remove the dependency array altogether, the effect runs on **every** render, which means we're making API calls every time the component re-renders. Since something simple like changing state (ex. clicking on the Today or Yesterday buttons) causes a re-render, we'd essentially fetch data everytime the user clicked one of the buttons. This is not good for application performance, nor your API bill.

Ultimately, hooks are meant to compartmentalize application logic to make them easier to re-use and reason about. useEffect is no different. Like all hooks, it runs on every render by default, but unlike other hooks, provides a way to conditionally control our logic execution (i.e. the dependency array). While useEffect is often described as componentDidMount, componentDidUpdate and componentWillUnmount altogether, I would try to avoid thinking about hooks in terms of their equivalent lifecycle methods. Instead, identify the effects that need to take place in your component, determine how often your would like the effect to run and cleanup your work (if needed) when the component unmounts.

Try adding useEffect to your next component. Maybe you'll get hooked.

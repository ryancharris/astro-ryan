---
title: "Hooks Revisited: useCallback"
publishDate: "2020-04-21"
published: true
layout: "../../layouts/BlogPost.astro"
description: "useCallback is similar to useMemo but returns a memoized function, not a memoized value."
category: "react"
tags:
  - "hooks"
---

### Before you proceed...

If you haven't read my <a href="posts///hooks-usememo">useMemo</a> article yet, I **highly suggest you go back and do so** now! In that article, we covered important concepts like [memoization](https://en.wikipedia.org/wiki/Memoization), which we will continue discussing below.

Since useMemo and useCallback are similar (with one key difference), it will be important to understand how useMemo works before proceeding.

### What's the difference?

Both useMemo and useCallback utilize memoization to optimize performance, however, there is a subtle difference between them. While useMemo returns a memoized value resulting from the logic contained within the hook's body, useCallback returns a memoized version of the **function itself**.

In the code block below, I've taken the useCallback example from the [React docs](https://reactjs.org/docs/hooks-reference.html#usecallback) and placed it next to its useMemo equivalent to better illustrate the difference:

```javascript
// memoizedFunction is a function
const memoizedFunction = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

// memoizedFunction is the value returned from doSomething(a, b)
const memoizedValue = useMemo(() => {
  doSomething(a, b);
}, [a, b]);
```

Here, useMemo and useCallback achieve the same thing: optimizing performance by returning cached values when a function has already been executed using the arguments it receives. Since they return different values, both hooks offer you a different way to leverage memoization based on your specific use-case.

### In practice

useCallback is useful because you can assign a memoized function to a variable and pass it around your application. This allows you to avoid re-creating the caching mechanism that memoization uses to improve performance.

It also makes our lives easier because we do not need to duplicate useMemo logic in multiple places. We also don't need to import/export anything. Instead, we can just pass the memoized function as a prop and allow another component to consume it.

In the sandbox below, I've taken the code from our <a href="/posts/hooks-usecallback">useMemo</a> example and refactored it to use useCallback:

<iframe
  src="https://codesandbox.io/embed/hooksusecallback-89w2p?fontsize=14&hidenavigation=1&theme=dark&view=editor"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useCallback"
  allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr"
  sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

Like with useMemo, our useCallback hook is returning a memoized value. However, in this case, it is a memoized version of the anonymous function passed to it, **not** the function's return value.

For demonstration purposes, we have two map components on this page (i.e. MapOne and MapTwo), which render -- you guessed it -- maps. If we assume they both plot coordinates in the same way, we can now pass createMapCoordinates to both components, allowing them to utilize the memoized function internally without having to recreate it in both places.

```javascript
const myFunction = () => {
  doStuff();
};
```

If you think about it, what we're doing here with useCallback isn't too much different from the snippet above since both of them create a variable and assign a function as its value. Our hook just memoizes the function so we can optimize our applications performance!

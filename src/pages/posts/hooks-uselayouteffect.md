---
title: "Hooks Revisited: useLayoutEffect"
publishDate: "2020-03-02"
published: true
layout: "../../layouts/BlogPost.astro"
description: "useLayoutEffect works exactly the same way useEffect does except it fires prior to the DOM's initial paint."
category: "react"
tags:
  - "hooks"
---

Last time, we learned about the <a href="/posts/hooks-useeffect">useEffect hook</a>, how it works and when to use it. If you have not read that article yet, I **strongly suggest you go back and do so before proceeding any further**. Much of what we will discuss below will be about the similarities and differences between useEffect and useLayoutEffect, which may not make much sense without having a good grip on the former.

### useLayoutEffect and useEffect

> The signature is identical to useEffect, but it fires synchronously after all DOM mutations. [[React docs]](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)

While the underlying implementation and execution of these hooks does differ, you may notice that the code snippets below look quite similar. That's because these two hooks have the same function signature!

```javascript
useEffect(() => {
  function getData() {
    // Define effect logic here
  }

  const apiData = getData();
  setData(apiData);
}, []);
```

```javascript
useLayoutEffect(() => {
  function handleResize() {
    // Define effect logic here
  }

  document.addEventListener("resize", handleResize);
  return () => {
    document.removeEventListener("resize", handleResize);
  };
}, []);
```

To quickly recap, the anatomy of these hooks are comprised of three key pieces:

1. The effect
2. A dependency array
3. A cleanup function

Since we're already pretty familiar with how these hooks are composed, let's look a bit more about what makes them different.

### Difference between useLayoutEffect and useEffect

As we saw above, these two hooks are quite similar in terms of their syntax and function signatures, however, the difference between them is quite subtle and has everything to do with timing.

The useEffect hook fires **after render** so as not to block the DOM from painting and effecting your application's performance. Due to this behavior, the React documentation advises that when writing new effects you start with useEffect and only reach for useLayoutEffect when absolutely necessary.

> ...we recommend starting with useEffect first and only trying useLayoutEffect if that causes a problem... [[React docs]](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)

Unlike useEffect, useLayoutEffect fires after all DOM mutations but **before the DOM paints**. While this is the **only** difference between these two hooks, it is an important distinction because of when their effects are performed.

But when would you want to use one versus the other? The general rule of hand is as follows:

1. Use useLayoutEffect when reading, manipulating or observing the DOM
2. Use useEffect for all other effects that don't require interaction with the DOM

If you're still a bit confused, don't worry! It can be hard to wrap your head around not only the difference between these two hooks but their specific use cases, as well. Below, we'll look at a practical example, which will help illustrate the difference a bit more clearly.

### In practice

In both of the examples below, we have a simple application that renders a few HTML elements, namely two divs and a main, which we can see in App.js:

```javascript
return (
  <div className="App">
    <main className="App__main">
      <div className="App__square" />
    </main>
  </div>
);
```

Above each App's return, you will see an effect defined with either useEffect or useLayoutEffect. The snippet below shows them side-by-side:

```javascript
useLayoutEffect(() => {
  const greenSquare = document.querySelector(".App__square");
  greenSquare.style.transform = "translate(-50%, -50%)";
  greenSquare.style.left = "50%";
  greenSquare.style.top = "50%";
});

useEffect(() => {
  const greenSquare = document.querySelector(".App__square");
  greenSquare.style.transform = "translate(-50%, -50%)";
  greenSquare.style.left = "50%";
  greenSquare.style.top = "50%";
});
```

As I am sure you've noticed by now, the effect function passed to both hooks are **exactly the same**. Again, the big difference here is the timing of when these effects run.

To start, let's look at the sandbox using eEffectYou should see the purple circle appear in the top left-hand corner of the screen before it is quickly repositioned and moved to the center of the screen. This happens because **useEffect runs after render**, so the effect isn't executed till after the DOM paints, which is what causes the unwanted flash of content.

<iframe
  src="https://codesandbox.io/embed/hooksuselayouteffect-useeffect-vgkbd?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useLayoutEffect (useEffect)"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

Now, let's look at the seLayoutEffecthook. If you refresh the page, you should always see the purple circle in the center of the screen and no longer see the circle get quickly repositioned. This is because **useLayoutEffect runs before the DOM paints**, so the circle has already been positioned correctly before we see the first visual representation of our page. In this example, it would be more appropriate to use seLayoutEffectbecause we would not want our users to see what looks like a visual bug.

<iframe
  src="https://codesandbox.io/embed/hooksuselayouteffect-uselayouteffect-jxvzg?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useLayoutEffect (useLayoutEffect)"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

### Next steps

In the examples above, we are using vanilla JavaScript to access and modify the document, which is considered an anti-pattern in React. A more appropriate way to do this would be to use a ref instead of accessing the DOM directly. Luckily, we'll be covering that in the next article about useRef!

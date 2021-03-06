---
title: "Hooks Revisited: useMemo"
publishDate: "2020-03-24"
published: true
layout: "../../layouts/BlogPost.astro"
description: "useMemo allows you to avoid recomputing expensive values by leveraging memoization under the hood."
category: "react"
tags:
  - "hooks"
---

Up until this point in <a href="/posts/hooks-revisited">the series</a>, I have been generally familiar with the hooks we've covered and have used them before at work. It wasn't until I recently started working in a new codebase that I came across [useMemo](https://reactjs.org/docs/hooks-reference.html#usememo). Not understanding how it worked or how to debug it was a large part of why I chose to write this series in the first place.

### What is "memoization"?

If you look at the React docs, they say that the useMemo hook "returns a memoized value". [Memoization](https://en.wikipedia.org/wiki/Memoization) was **not** a term I was familiar with when I first read that, so don't worry if you haven't heard of it either. We're in the same boat!

Memoization is an optimization strategy that returns cached values from functions that have previously been invoked with the same arguments. In other words, instead of recalculating its return value, the function will return a cached value. This is useful when you have functions doing memory intensive operations and want to minimize how often they're invoked.

Here's my mental model for how this works:

```javascript
// Value must be calculated
add(1, 2);

// Value must be calculated
add(3, 4);

// Cached value returned
add(1, 2);
```

If you want to read more about memoization, checkout [this article](https://scotch.io/tutorials/understanding-memoization-in-javascript#toc-a-functional-approach) on [Scotch.io](https://scotch.io) by [Philip Obosi](https://twitter.com/worldclassdev). He takes a deeper look at memoization and how to implement your own memoized functions using plain JavaScript.

### Anatomy of useMemo

As mentioned, the useMemo hook returns a "memoized value" and takes two arguments:

1. A function
2. A dependency array

Here's an example of how it looks directly from the React docs:

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

If you have read my articles on <a href="/posts/hooks-useeffect">useEffect</a> and <a href="/posts/hooks-uselayouteffect">useLayoutEffect</a>, you'll probably recognize this function signature. Just like those hooks, seMemoexecutes the logic inside the function passed to it **only** when one of the values in the dependency array changes. If no array is passed, seMemowill re-calculate its return value on **every** render.

The difference here is that seMemois not intended to cause side effects -- those should be handled in the appropriately named seEffector seLayoutEffecthooks. seMemosimply calculates and returns a value based on the function and dependency array passed as arguments and helps handle expensive computations that may cause performance issues.

### Optimization

According to the React docs, seMemois meant to be a **performance optimization**. They suggest you get your code to work without seMemoand implement it after the fact.

One thing to note, however, is that you can't really guarantee that seMemowill return a cached value when expected. Read the following sentence carefully:

> In the future, React may choose to ???forget??? some previously memoized values and recalculate them on next render, e.g. to free memory for offscreen components. [[React docs](https://reactjs.org/docs/hooks-reference.html#usememo)]

To keep things performant and properly manage memory, React may get rid of a cached value it's not actively using in order to save room for other operations. In some cases, that will cause useMemo to recalculate its return value even though it had been previously in our cache.

### In practice

In the example below, I've created a demo to better illustrate how useMemo works. For our purposes, I have stubbed out some functions in order to make the example work properly; however, pay attention to the comments as they will provide further context.

_Note: If you're unfamiliar with <a href="/posts/hooks-useeffect">useEffect</a> or <a href="/posts/hooks-usestate">useState</a>, take a moment and check out the previous articles in this series before continuing. Otherwise, these should look pretty familiar to you._

<iframe
  src="https://codesandbox.io/embed/hooksusememo-kfgvu?expanddevtools=1&fontsize=14&hidenavigation=1&theme=dark&view=editor"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="hooks/useMemo"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

Here, our App component does three things:

1. Makes a call to the NASA API in a useEffect hook and fetches a list of NASA facilities, which we store in useState

```javascript
useEffect(() => {
  fetch("https://data.nasa.gov/resource/gvk9-iz74.json")
    .then((res) => res.json())
    .then((json) => {
      setNasaLocations(json);
    })
    .catch((err) => console.log("Error fetching data", err));
}, []);
```

2. Observes the input in our return and stores its value in another useState hook

```javascript
const [inputValue, setInputValue] = useState("")

<input
  name="search"
  type="search"
  onChange={event => setInputValue(event.currentTarget.value)}
/>
```

3. Passes a filtered array to MapView via the coordinates prop, which represents location information for each facility

```javascript
<MapView coordinates={mapCoordinates} />
```

Technically, we could achieve these three goals without using useMemo, however, the NASA API returns to us a list of 484 facilities, which we iterate over in createMapCoordinates().

This is **expensive** work, especially if the function runs frequently. On top of that, it calls another function for each item in the array (i.e. createCoordsForLocation()), which currently returns a plain JavaScript object.

```javascript
function createCoordsForLocation(lat, long) {
  return {
    latitude: lat,
    longitude: long,
  };
}
```

But imagine that createCoordsForLocation() called a backend service for each item in the list to compute its geographic coordinates. This would make our already expensive createMapCoordinates() call that much more memory intensive. Since we need to do this to make our app function properly, we can leverage useMemo to optimize performance.

Let's look at our use case:

```javascript
const mapCoordinates = useMemo(() => {
  return createMapCoordinates(inputValue, nasaLocations);
}, [inputValue, nasaLocations]);
```

First, look at the dependency array (i.e. [inputValue, nasaLocations]). Like useEffect, this tells useMemo only to run when either of those values change. Right now, we only make a call for nasaLocations on initial render, so its value will only change once, which triggers the hook.

Our other value (i.e. inputValue), represents the value the user has entered in the input. Anytime the user adds or removes characters from the input, the inputValue will change in our useState hook and cause useMemo to run again.

The trick here is that since we filter our nasaLocations list based on the inputValue, we can use useMemo to reduce computations. Since the hook will return a cached value whenever it receives inputs it has used for computation before, we will avoid re-running all the logic in createCoordsForLocation() and createMapCoordinates() if the inputValue and nasaLocations array we're passing in have already been processed.

Of all the hooks we've covered thus far, useMemo is one of the harder ones to illustrate as its effect on your application is not necessarily visual, but performance based. Like the React docs say, get your application logic working **without** useMemo to confirm the proper functionality. Then, go through your component code and identify any expensive computations as these may be great candidates for useMemo!

In the next article, we'll be covering useCallback, which also leverages memoization, but is subtly different from useMemo. Stay tuned to find out how!

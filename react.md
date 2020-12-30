## **React Cheat sheet**
***
>framework/library, frontend, fast, JSX, component based

React is a fast, Declarative Javascript library that allows simple interactive user interfaces to be designed with relative ease and reason. 

**New JSX Enhancements with React 17**

- React 17 compiler imports a new dependency from react/jsx-runtime and this now handles the JSX transformations 

``` import React from 'react';

function App() {
  return <h1>Hello World</h1>;
} 
```
compiles to
```
import React from 'react';

function App() {
  return React.createElement('h1', null, 'Hello world');
}
```
Problem...
- JSX is compiled into `React.createElement,` `React` needs to be within scope to use the JSX
- Performance and simplicity suffers from this implementation
Solution...
React 17 has two entry points to the React package that are used by compilers like Babel and Typescript. This compilation transformation is now handled at these access points.
Result...
```
function App() {
  return _jsx('h1', { children: 'Hello world' });
}
```
though this is...
```
// Inserted by a compiler (don't import it yourself!)
import {jsx as _jsx} from 'react/jsx-runtime';
```

>Note how our original code did not need to import React to use JSX anymore! (But we would still need to import React in order to use Hooks or other exports that React provides.)

[The Technical Request for Comments (RFC) on this implementation](https://github.com/reactjs/rfcs/blob/createlement-rfc/text/0000-create-element-changes.md#detailed-design)

- React 17 allows gradual updates so apps don't get left behind if they are using older React versions in part of their code (previously React operated on a all or nothing, you either updated or you stay on the old version)

[automated script to remove imports](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#removing-unused-react-imports)

[**Perf** react performance tool](https://reactjs.org/docs/perf.html)

    Note this is depreciated as of React 16, but still good to know the tool exists
  
  React is known to be very fast, but sometimes certain portions of code need to be optimized. **Perf** is a tool used as a microscope to zero in on problem areas. The implementation provides shouldComponentUpdate() method where you can add optimization hints to React's diff algorithm. In addition it provides you with exactly where to implement these methods.

  **Only works on development build**
  
  browsers profiling tooling is the preferred method for optimization 
    - ie Chrome Performance Tab


![pic of chromes devtool](https://reactjs.org/static/64d522b74fb585f1abada9801f85fa9d/1ac66/react-perf-chrome-timeline.png)


**Development vs. Production Builds**

  when benchmarking, remember to use the minified production build as the development build includes extra warnings and performance bottle-necking bookkeeping.


  **Profiling Components with the Chrome Performance Tab**

1. Temporarily disable all Chrome extensions, especially React DevTools. They can significantly skew the results!
2. Make sure you’re running the application in the development mode.
3. Open the Chrome DevTools Performance tab and press Record.
4. Perform the actions you want to profile. Don’t record more than 20 seconds or Chrome might hang.
5. Stop recording.
6. React events will be grouped under the User Timing label.

[Profiling React component performance with chrome devtools](https://calibreapp.com/blog/react-performance-profiling-optimization)

[React Profiling walkthrough video](https://www.youtube.com/watch?v=nySib7ipZdk)

**React Pure Components**
resources
[pure component](https://www.youtube.com/watch?v=X94LfGK7I9Y&feature=emb_logo)
Do not depend or modify state of variables outside their scope. 

they have huge performance impact, for example:

Application calls for the same function multiple times with the same parameters — assume “add(10, 20)”. After executing it multiple times, the Chrome V8 Engine tries to optimize the code further by storing the execution result of the following function call. On the next call to the same function, with the same parameter, instead of executing the function again, the cached result is returned. Hence enhancing the Application Performance.

Pure components have some performance improvements and render optimizations since React implements the shouldComponentUpdate() method implicitly for them with a shallow comparison for props and state.

>However, functional components cannot leverage the performance improvements and render optimizations that come with React.PureComponent since they are not classes by definition.
In fact, if you have a functional component and you want React to treat it as a pure component, you will have to convert the functional component to a class component that extends React.PureComponent.

```
// FUNCTIONAL COMPONENT
function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}


// CONVERTED TO PURE COMPONENT
class PercentageStat extends React.PureComponent {

  render() {
    const { label, score = 0, total = Math.max(1, score) } = this.props;

    return (
      <div>
        <h6>{ label }</h6>
        <span>{ Math.round(score / total * 100) }%</span>
      </div>
    )
  }
```

Problem...
I don't really like working with class base components... just a preference
Solution...(SEE BELOW FOR ANOTHER SOLUTION USING REACT 16.6)
Using the { pure } HOC from [**Recompose**](https://github.com/acdlite/recompose) (HOC - Higher Order Component) which is useful when working with functional components.


the { pure } HOC tries to prevent re-renders unless there is a change in props using a `shallowEqual()` to test for changes

example of using { pure }
```
import React from 'react';
import { pure } from 'recompose';

function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

// Wrap component using the `pure` HOC from recompose
export default pure(PercentageStat);
```

**BUT**
React 16.6 introduced **`React.memo()`**

`React.memo()` basically memoizes functional components
>memoizes - To store (the result of a computed expression) so that it can be subsequently retrieved without repeating the computation.

**Implementation details**

1. `React.memo()` is a higher-order component. It takes a React component as its first argument and returns a special kind of React component.
2. `React.memo()` returns a special React component type. That allows the renderer to render the component while memoizing the output — thus, bailing out of updates if the component’s props are shallowly equal.
3. `React.memo()` works with all React components. The first argument passed to React.memo() can be any type of React component. However, for class components, you should use React.PureComponent instead of using React.memo().
4. `React.memo()` also works with components rendered from the server using ReactDOMServer.

**Using `React.memo()`**

```
import React, { memo } from 'react';

function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{ label }</h6>
      <span>{ Math.round(score / total * 100) }%</span>
    </div>
  )
}

// Wrap component using `React.memo()`
export default memo(PercentageStat);
```
>**Custom bailout condition**
The React.memo() API can take a second argument, which is the arePropsEqual() function.
The default behavior of React.memo() is to shallowly compare the component props. However, with the arePropsEqual() function, you can customize the bailout condition for component updates.
The arePropsEqual() function is defined with two parameters: prevProps and nextProps.
The arePropsEqual() function returns true when the props are compared to be equal (thereby preventing the component from re-rendering) and returns false when the props are not equal.
The arePropsEqual() function acts much like the shouldComponentUpdate() lifecycle method in class components, but in the reverse manner.

**Conditional Rendering**
[source](https://www.digitalocean.com/community/tutorials/7-ways-to-implement-conditional-rendering-in-react-applications)


**Security**
[source](https://react.christmas/2020/3)

Out of the box react provides security against XSS (Cross Site Scripting) which is a vulnerability that allows un-trusted sources to place malicious data or scripts into a web application. Browsers interpret these scripts as legitimate and run them, allowing data and control to be given to the illegitimate source. By default React will escape all data embedded in JSX. This just removes or replaces any characters that could be interpreted as code. Caveats in tow, you can find yourself outside Reacts auto-escaping, to solve this is use `dangerouslySetInnerHtml`.

URLs should never be from an external source. Do not let users input their own URLS.

You should always keep your framework updated to keep security patches inline.

Never trust data from an external source. USer, API or address bar in browser. Handle data as malicious and take security measures based on where the data is.

**Intelligent fetching and caching with SWR**
[source](https://react.christmas/2020/4)

>SWR has quickly become one of my favorite tools in my toolbox. The name SWR refers to the caching strategy stale-while-revalidate, and in its essence, the library provides just this, a caching strategy. The library is made by Vercel, the creators of Next.js, and has excellent documentation.

You provide the data fetching, SWR handles the caching, and stores it for you.

**Getting the Data**
Fetching data through the SWR is simple. The `useSWR` hook is a key tool.
`const { data, error } = useSWR(URL, fetcher);`
first argument is the fetcher function use to fetch data. The data object is either `undefined` (this is what happpens when initially loading) or data from the cache. `error` is `undefined` unless the fetcher function has thrown an exception or rejected the promise.
typical fetcher function: `const fetcher = (url) => fetch(url).then((res) => res.json());`

The data is `undefined` the first time `useSWR` hook is used until it is loaded. When the same key is used again, the stale data is immediately returned. The SWR revalidates the data and once it is finished the cache is refreshed and the new data is served. 

`const { data, error, isValidating } = useSWR(URL, fetcher);` useSWR hook exposes the revalidation status through the hook, which can be used to show some loading state.

**Mutation**
If you want to change some of the data, you can do so using the `mutate(key, data?, shouldRevalidate?)`

the `key` is the cache key, `data` is the changed data object, and `shouldRevalidate` is whether or not to trigger revalidation of the data against the resource after mutation the data.

For an even tidier usage, you can pull out the Swiss army knife again, as the mutate function with a pre-set key is given through the useSWR hook; all you've got to do is ask!

`const { data, error, isValidating, mutate } = useSWR(URL, fetcher);`

Combining these to achieve the initial goal of a fast change while updating a resource, your function would look like this:

```
const changeUsername = (username) => {
  // Mutate local data
  mutate('/api/user', { ...data, username}, false)
  // Update resource
  await updateUserName(username)
  // Optional: Trigger revalidation to ensure we have correct data
  mutate()
}
```
the first revalidation in the first mutation. This is because revalidation would load the

**still need to finish notes for this**



###**Code-Splitting**
[source](https://reactjs.org/docs/code-splitting.html)
**Bundling**
Most React apps will have their files “bundled” using tools like [Webpack](https://webpack.js.org/), [Rollup](https://rollupjs.org/guide/en/) or [Browserify](http://browserify.org/).  Bundling is when imported files are merged into a single file or a couple of files: a "bundle". This bundle is then used to load and entire app at once

**Code Splitting**

Bundling is an effective solution to many problems, but as apps scale especially when including third party libraries the app code base can become very large. The solution to this problem is splitting your code. **code-splitting** is a feature supported by bundlers like Webpack, Rollup and Browserify (via factor-bundle) which can create multiple bundles that can be dynamically loaded at runtime.

**Lazy Loading**

(source)[https://www.freecodecamp.org/news/how-to-optimize-react-applications-with-lazy-loading-232183e02768/]
is an old technique to optimize applications. The technique sets aside non-critical assets to be rendered following all the critical ones


##**State**

**Recoil**
[source](https://www.telerik.com/blogs/recoil-in-react)
[source](https://recoiljs.org/)
State management library
- Minimal and Reactish
- Data-flow Graph
- Shared state
  - the same state in different components in the React tree in a way that's really performant and consistent
- Derived data and queries 
  - Compute things based on ing state efficiently in a very robust way. Derived data are things that are computed or related to the state in some way.
- App-Wide state observation 
  - Observe changes, time-travel debugging, persistence, logging-observe everything happening in the app from some component

  **Atoms**
  An Atom is a changeable, subsribeable unit of the state. Changing the state will re-render every components that are subscribed to that atom.

  **Selectors**
  a Selector is a pure function that can receive an atom or a selector as an input. given an input, the selector returns a modified state every time the upstream atoms or selectors are updated.


####**Context**
[source](https://reactjs.org/docs/context.html)

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

think of Context as a global variable for state

you can even pass context setter into the context

**When to use Context**

anything that is considered "Global" such as authenticated, theme or preferred language.

rule of thumb, usually you shouldn't use Context, unless you really need to use it.

####**Side effects and useEffect**

useEffect lets us perform side effects in function components. So what are side effects?

- side effects are where we need to reach into the outside world. For example, fetching data from an API or working with DOM
- side effecs are actions that can change our component state in an unpredictable fashion (that have caused 'side effects').

useEffect accepts a callback function (called the 'effect' function), which will by default run every time there is a re-render. It runs once our component mounts, which is the right time to perform a side effect in the component lifecycle.

```
// what does our code do? Picks a color from the colors array
// and makes it the background color
function App() {
  const [colorIndex, setColorIndex] = React.useState(0);
  const colors = ["blue", "green", "red", "orange"];

  // we are performing a 'side effect' since we are working with an API
  // we are working with the DOM, a browser API outside of React
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
  });
  // whenever state is updated, App re-renders and useEffect runs

  function handleChangeIndex() {
    const next = colorIndex + 1 === colors.length ? 0 : colorIndex + 1;
    setColorIndex(next);
  }

  return <button onClick={handleChangeIndex}>Change background color</button>;
}
```

To avoid executing the effect callback after each render, we provide a second argument, an empty array:

```
function App() {
  ...
  // now our button doesn't work no matter how many times we click it...
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
  }, []);
  // the background color is only set once, upon mount

  // how do we not have the effect function run for every state update...
  // but still have it work whenever the button is clicked?

  return (
    <button onClick={handleChangeIndex}>
      Change background color
    </button>
  );
}
```

useEffect lets us conditionally perform effects with the dependencies array.

The dependencies array is the second argument, and if any one of the values in the array changes, the effect function runs again.

```
function App() {
  const [colorIndex, setColorIndex] = React.useState(0);
  const colors = ["blue", "green", "red", "orange"];

  // we add colorIndex to our dependencies array
  // when colorIndex changes, useEffect will execute the effect fn again
  useEffect(() => {
    document.body.style.backgroundColor = colors[colorIndex];
    // when we use useEffect, we must think about what state values
    // we want our side effect to sync with
  }, [colorIndex]);

  function handleChangeIndex() {
    const next = colorIndex + 1 === colors.length ? 0 : colorIndex + 1;
    setColorIndex(next);
  }

  return <button onClick={handleChangeIndex}>Change background color</button>;
}
```

useEffect lets us unsubscribe from certain effects by returning a function at the end:

```
function MouseTracker() {
  const [mousePosition, setMousePosition] = useState({ x: 0, y: 0 });

  React.useEffect(() => {
    // .addEventListener() sets up an active listener...
    window.addEventListener("mousemove", event => {
      const { pageX, pageY } = event;
      setMousePosition({ x: pageX, y: pageY });
    });

    // ...so when we navigate away from this page, it needs to be
    // removed to stop listening. Otherwise, it will try to set
    // state in a component that doesn't exist (causing an error)

    // We unsubscribe any subscriptions / listeners w/ this 'cleanup function'
    return () => {
      window.removeEventListener("mousemove", event => {
        const { pageX, pageY } = event;
        setMousePosition({ x: pageX, y: pageY });
      });
    };
  }, []);

  return (
    <div>
      <h1>The current mouse position is:</h1>
      <p>
        X: {mousePosition.x}, Y: {mousePosition.y}
      </p>
    </div>
  );
}

// Note: we could extract the reused logic in the callbacks to
// their own function, but I believe this is more readable
```

- Fetching data with useEffect

Note that handling promises with the more concise async/await syntax requires creating a separate function. (Why? The effect callback function cannot be async.)

```
const endpoint = "https://api.github.com/users/codeartistryio";

// with promises:
function App() {
  const [user, setUser] = React.useState(null);

  React.useEffect(() => {
    // promises work in callback
    fetch(endpoint)
      .then(response => response.json())
      .then(data => setUser(data));
  }, []);
}

// with async / await syntax for promise:
function App() {
  const [user, setUser] = React.useState(null);
  // cannot make useEffect callback function async
  React.useEffect(() => {
    getUser();
  }, []);

  // instead, use async / await in separate function, then call
  // function back in useEffect
  async function getUser() {
    const response = await fetch("https://api.github.com/codeartistryio");
    const data = await response.json();
    setUser(data);
  }
}
```

**Performance and useCallback**

useCallback is a hook that is used for improving our component's performance.
2
If you have a component that re-renders frequently, useCallback prevents callback functions within the component from being recreated every single time the component re-renders (which means the function component re-runs).

useCallback re-runs only when one of it's dependencies changes.

```
// in Timer, we are calculating the date and putting it in state a lot
// this results in a re-render for every state update

// we had a function handleIncrementCount to increment the state 'count'...
function Timer() {
  const [time, setTime] = React.useState();
  const [count, setCount] = React.useState(0);

  // ... but unless we wrap it in useCallback, the function is
  // recreated for every single re-render (bad performance hit)
  // useCallback hook returns a callback that isn't recreated every time
  const inc = React.useCallback(
    function handleIncrementCount() {
      setCount(prevCount => prevCount + 1);
    },
    // useCallback accepts a second arg of a dependencies array like useEffect
    // useCallback will only run if any dependency changes (here it's 'setCount')
    [setCount]
  );

  React.useEffect(() => {
    const timeout = setTimeout(() => {
      const currentTime = JSON.stringify(new Date(Date.now()));
      setTime(currentTime);
    }, 300);

    return () => {
      clearTimeout(timeout);
    };
  }, [time]);

  return (
    <div>
      <p>The current time is: {time}</p>
      <p>Count: {count}</p>
      <button onClick={inc}>+</button>
    </div>
  );
}
```


**Javascript Computed Property Names**
[source](http://eloquentcode.com/computed-property-names-in-javascript#:~:text=Computed%20Property%20Names%20is%20an,be%20determined%20dynamically%2C%20i.e.%20computed.)

This is an ES6 feature, it allows names of objects properties to be dynamically computed through object literal notation

in the past we would have to do this 
```
var STATUS_SUCCESS = "STATUS_SUCCESS";
var STATUS_FAILURE = "STATUS_FAILURE";

var messages = {};

messages[STATUS_SUCCESS] = "Updated";
messages[STATUS_FAILURE] = "Error";
```
example
```
const myPropertyName = 'c'

const myObject = {
  a: 5,
  b: 10,
  [myPropertyName]: 15
} 

console.log(myObject.c) // prints 15
```
```
const fieldNumber = 3

const myObject = {
  field1: 5,
  field2: 10,
  ['field' + fieldNumber]: 15
}

console.log(myObject.field3) // prints 15
```

```
const fieldNumber = 3

const myObject = {
  field1: 5,
  field2: 10,
  [`field${fieldNumber}`]: 15
}

console.log(myObject.field3) // prints 15
```
before
```
function objectify (key, value) {
  let obj = {}
  obj[key] = value
  return obj
}

objectify('name', 'Tyler') // { name: 'Tyler' }
```
now
```
function objectify (key, value) {
  return {
    [key]: value
  }
}
```

###**React DOM**

[source](https://scrimba.com/articles/react-interview-questions/)

[source](https://reactjs.org/docs/faq-internals.html)

the DOM represents theh HTML doucment as a tree structure, with each note representing an element, element attribute or text
![DOM](https://scrimba.com/articles/content/images/size/w1000/2020/11/image-2.png)

any node on the tree can be accessed using JavaScript and the DOM API. When you do this, the browser traverses the DOM and re renders each node, even if that node hasn't changed since the previous render. This is inefficient.

>Imagine a scenario where you need to update only one tr of 10,000 in a table. Rendering all 10,000 rows will almost certainly lead to a drop in frames, potentially causing the table to flicker and interrupt the user's experience.

React's DOM is a virtual representation of the DOM

Every time your app's state updates, React builds a new VDOM and diffs with the previous VDOM to determine what changes are necessary before updating the DOM directly and efficiently:

![](https://scrimba.com/articles/content/images/size/w1000/2020/11/2.png)

**reconciliation** (React reconciles the newly-built VDOM with the previous one)

**VDOM is a programming concept**

[the shadow DOM](https://www.youtube.com/watch?v=K5i9zMzVlzM&feature=youtu.be)

**Prop Drilling**
[source](https://scrimba.com/articles/react-interview-questions/)
[source](https://kentcdodds.com/blog/prop-drilling)

Prop Drilling is when you pass props down through a component that doesn't use the prop but passes it down to a deeper component.

also known as **threading**.

solution to prop drilling is compound components or react context


###**React Lifecycle**
[source](https://tsh.io/blog/react-component-lifecycle-methods-vs-hooks/)
[source](https://reactjs.org/docs/hooks-intro.html)
[source](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
The Basics
- **Mounting:** inserting elements into the DOM
- **Updating:** using methods to update components in the DOM
- **Unmounting:** removing a component from the DOM

[**Really useful Lifecycle diagram**](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

##**Lifecycle Methods**

#####**Mounting**
- **constructor()**
- static getDerivedStateFromProps()
- **render()**
- **componentDidMount()**

#####**Update**
- static getDerivedStateFromProps()
- shouldComponentUpdate()
- **render()**
- getSnapshotBeforeUpdate()
- **componentDidUpdate()**

#####**Unmounting**
- **componentWillUnmount()**

**Handling exceptions**
- static getDerivedStateFromError()
- componentDidCatch()


**Other API**
Each component also provides several other APIs:

- setState()
- forceUpdate()
Properties of the class
- defaultProps
- displayName
Instance properties
- props
- state

#####render()
this is the only method required in a class component. When first invoked it should check this.props and this.state and return one of the following types:
- React element
- Arrays and fragments 
- Portals
- Strings and numbers
- Boolean or null
the function `render()` should be pure, it doesn't modify the components state, returns the same result every time it is invoked, and doesn't interact with the browser
>attention: the function `render()` will not be called if `shouldComponentUpdate()` returns `false`

#####constructor(props)
unless you initilize state and bind methods, you don't need to implement a constructor in your React component

typically, constructors are used for two purposes:
- Initializing local state by assigning an object to this.state.
- Binding event handler methods to an instance.

don't call setState() in the `constructor()`. Instead assign the initial state to this.state
```
constructor(props) {
  super(props);
  // Don't call this.setState() here!
  this.state = { counter: 0 };
  this.handleClick = this.handleClick.bind(this);
}
```
#####componentDidMount()
Is invoke immediately after a component is mounted (inserted into the tree). Initialization that requires the DOM nodes should go here.

This method is a good place to set up any subscriptions. If you do that, don’t forget to unsubscribe in componentWillUnmount().

#####componentDidUpdate(prevProps, prevState, snapshot)
Is invoked immediately after updating occurs. This method isn't called for the initiate render.
>attention: componentDidUpdate() will not be invoked if shouldComponentUpdate() returns false.

#####componentWillUnmount()
Is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, cancelling network requests, or cleaning up any subscriptions that were created in `componentDidMount()`



####**React component lifecycle with hooks**
Using `useEffect()` hook to achieve the same results as with the componentDidMount, componentDidUpdate and componentWillUnmount methods. `useEffect()` accepts two parameters, a callback that runs after the render, a dependency array. If you want it to run on mount and unmount only, pass an empty array [].

to clean up, return the callback in useEffect: 
```
useEffect(
  () => {
    document.addEventListener(“click”, someFunc);
    
    return () => {
      document.removeEventListener(“click”, someFunc);
    };
  },
  []
);
```

#TO ADD
- unidirectional dataflow
- server-side rendering


#####Questions
1. What is React?
- JS frontend library that is used to make interactive applications for the web or mobile, often single page. It is used for handing the view layer for the application.
2. What are the major features of React?
- The lifecycle methods for tracking state, and the need to re-render individual components within a DOM tree, using the virtual DOM. Server side rendering. Unidirectional data flow. Composition with components /reuseable components
3. What is JSX?
- syntactic sugar, looks like html, provides us with a way to express our apps with javascript along with html like template.
4. What is the difference between Element and Component?
- elements are plain objects, describing what you want to appear on the DOM. Creating a react element is cheap. Once created, it is never mutated. components are complex and can be mutated over time.
5. How to create components in React?
- functional or class based definition
6. When to use a Class Component over a Function Component?
- since 16.8 it's preference
7. What are Pure Components?
- React.PureComponent and React.Component are the same except it handles the shouldComponentUpdate() method for you. when props or state changes, PureComponent will do a shallow comparison on both state and props, Component won't compare either, thus will re-render by default whenever shouldComponentUpdate()
8. What is state in React?
- an object that may be mutated within the components, usually we want to build as simple an concise state, while keeping as many components stateless as possible
9. What are props in React?
- are values that are passed down from parent component that the child component can use to either pass to its children, or effect the way the component works
10. What is the difference between state and props?
- props are immutable values provided from the parent component or to the child component. state is mutable values manipulated by the local component
11. Why should we not update the state directly?
- manipulating state directly doesn't allow react to track the re-render needed by the change to state.
12. What is the purpose of callback function as an argument of setState()?
- it invokes after setState finishes, though its recommend to use a life cycle method
13. What is the difference between HTML and React event handling?
- react you use camelCase, you call preventDefault() to do that and you do not append () after to invoke a function such as `onClick='doThisFunc' `
14. How to bind methods or event handlers in JSX callbacks?
15. How to pass a parameter to an event handler or callback?
16. What are synthetic events in React?
17. What are inline conditional expressions?
18. What is "key" prop and what is the benefit of using it in arrays of elements?
19. What is the use of refs?


###**React and SEO**
[source](https://rubygarage.org/blog/seo-for-react-websites)
- [Google gets about 90% of all search requests](https://gs.statcounter.com/search-engine-market-share)
- [The first five links that appear in search results always get the most traffic](https://www.impactplus.com/blog/seo-statistics)


#####1. Slow and Complex indexing process

![Indexing an HTML Webpage](https://rubygarage-production-production-bucket.s3.amazonaws.com/uploads/article_image/file/3467/indexing-html-page-1x.png)

![Indexing a JavaScript Webpage](https://rubygarage-production-production-bucket.s3.amazonaws.com/uploads/article_image/file/3469/indexing-javascript-page-1x.png)

#####2. Errors in JavaScript code 

HTML and JavaScript have absolutely different approaches to processing errors. A single error in JavaScript code can make indexing impossible.
This is because the JavaScript parser is completely intolerant of errors. 
A single error with stop the parsing immediately, thus the bot will see an empty page

#####3. Exhausted crawling budget

A crawling budget is the maximum number of pages that search engine bots can crawl in a specific period of time (usually five seconds for one script).

A lot of websites built on JavaScript experience indexing problems because Google has to wait too long (more than five seconds) for scripts to load, parse, and execute. Slow scripts mean the Google bot will quickly run out of its crawling budget for your site and leave it before indexing it.

#####4. Challenges of indexing SPAs

Single-page applications (SPAs) are web apps created with React. These web apps consist of only one page that’s loaded once.

All other information is loaded dynamically.If a bot is crawling the page when the content hasn’t been loaded, the bot will see an empty page. A significant part of the site won’t be indexed. Therefore, your site will get a much lower ranking in search results. 


####Making React website SEO-friendly

#####Pre-rendering

Pre-rendering is used when search bots can’t render your pages correctly. In these cases, you can use pre-renderers: special programs that intercept requests to your website and, if the request is from a bot, pre-renders send a cached static HTML version of your website. If the request is from a user, the usual page is loaded. 

- Pre-rendering programs are able to execute all types of modern JavaScript and transform it into static HTML.
- Pre-renderers support all the latest web novelties.
- This approach requires minimal codebase modifications or no modifications at all.
- It’s simple to implement.
However, there are also some drawbacks to this approach:
- It isn’t suitable for pages that display frequently changing data.
- Pre-rendering can take too long if the website is large and contains a lot of pages.
- Pre-rendering services aren’t free.
- You need to rebuild your pre-rendered page every time you change its content.

#####Server-side rendering

Client-side rendering means that a browser and Google bot get empty HTML files or files with little content. Then JavaScript code downloads the content from the server and users see it on their screens.

In terms of SEO, client-side rendering is a problem, as Google bots don’t get any content or get a little content they can’t index properly.

With server-side rendering, browsers and Google bots get HTML files with all the content. Google bots can index the page properly and rank it higher.
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

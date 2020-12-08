## **React Cheat sheet**
***
>language, frontend, fast, JSX

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
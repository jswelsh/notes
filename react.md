## **React Cheat sheet**



[**Perf** react performance tool](https://reactjs.org/docs/perf.html)

    Note this is depreciated as of React 16, but still good to know the tool exists
  
  React is known to be very fast, but sometimes certain portions of code need to be optimized. **Perf** is a tool used as a microscope to zero in on problem areas. The implementation provides shouldComponentUpdate() method where you can add optimization hints to React’s diff algorithm. In addition it provides you with exactly where to implement these methods.

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
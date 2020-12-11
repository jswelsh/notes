##**Webpack**
***
**sources**
[what is webpack](https://flaviocopes.com/webpack/#what-is-webpack)

**What is webpack**
What problem is Webpack solving?
and app usually has tons of javascript files, usually you would need to include script tags to reference each javascript file. This is made difficult if the scripts are out of order or you make a spelling mistake. Webpack does this for you.

Webpack is a module bundler, it compiles Javascript modules for an application. Often taking many files and generating a single or a few files that are used to run your app.

Webpack examines all your modules and (optionally ) transform them, then intelligently put all of them together into one or more bundles

other functions of Webpack
- helps you bundle your resources
- watches for changes and re-runs the tasks
- can run Babel transpilation to ES5
- can convert inline images to data URIs
- allows you to use require() for CSS files
- can run a development webserver
- can handle hot module replacement
- can split the output files into multiple files, tp avoid having a huge js file to load in the first page hit
- can perform tree shaking

Webpack is also useful for backend Node.js development 

To use Webpack you specify an entry point to your app(it could be an HTML file w/ script tags) and webpack analyzes the files and bundles all you need to run the app in a single Javascript file(or more if you use code splitting)

Webpack also has **Plugins** which are incredibly powerful tools that enhance Webpack

These Plugins allow you to execute certain tasks following a bundle has been created.
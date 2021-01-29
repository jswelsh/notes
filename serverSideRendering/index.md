## Server Side Rendering
***

### Server Rendering
Server rendering generates the full HTML for a page on the server in response to navigation. This avoids additional round-trips for data fetching and templating on the client, since it’s handled before the browser gets a response.

Server rendering generally produces a fast First Paint (FP) and First Contentful Paint (FCP). Running page logic and rendering on the server makes it possible to avoid sending lots of JavaScript to the client, which helps achieve a fast Time to Interactive (TTI). This makes sense, since with server rendering you’re really just sending text and links to the user’s browser. This approach can work well for a large spectrum of device and network conditions, and opens up interesting browser optimizations like streaming document parsing.

<img src="01.png
" alt="forked commit history" width="500"/>


#### Terminology
##### Rendering

**- SSR**: Server-Side Rendering - rendering a client-side or universal app to HTML on the server.
**- CSR**: Client-Side Rendering - rendering an app in a browser, generally using the DOM.
**Rehydration**: “booting up” JavaScript views on the client such that they reuse the server-rendered HTML’s DOM tree and data.
**- Prerendering**: running a client-side application at build time to capture its initial state as static HTML.

#####Performance

**- TTFB**: Time to First Byte - seen as the time between clicking a link and the first bit of content coming in.
**- FP**: First Paint - the first time any pixel gets becomes visible to the user.
**- FCP**: First Contentful Paint - the time when requested content (article body, etc) becomes visible.
**- TTI**: Time To Interactive - the time at which a page becomes interactive (events wired up, etc).


##### Static Rendering

React users may be familiar with Gatsby, Next.js static export or Navi - all of these make it convenient to author using components. However, it’s important to understand the difference between static rendering and prerendering: static rendered pages are interactive without the need to execute much client-side JS, whereas prerendering improves the First Paint or First Contentful Paint of a Single Page Application that must be booted on the client in order for pages to be truly interactive.

#### Server Rendering vs Static Rendering
Server rendering is not a silver bullet - its dynamic nature can come with significant compute overhead costs. Many server rendering solutions don't flush early, can delay TTFB or double the data being sent (e.g. inlined state used by JS on the client). In React, renderToString() can be slow as it's synchronous and single-threaded. Getting server rendering "right" can involve finding or building a solution for component caching, managing memory consumption, applying memoization techniques, and many other concerns. You're generally processing/rebuilding the same application multiple times - once on the client and once in the server. Just because server rendering can make something show up sooner doesn't suddenly mean you have less work to do.

Server rendering produces HTML on-demand for each URL but can be slower than just serving static rendered content. If you can put in the additional leg-work, server rendering + HTML caching can massively reduce server render time. The upside to server rendering is the ability to pull more "live" data and respond to a more complete set of requests than is possible with static rendering. Pages requiring personalization are a concrete example of the type of request that would not work well with static rendering.

Server rendering can also present interesting decisions when building a PWA. Is it better to use full-page service worker caching, or just server-render individual pieces of content?

#### Client-Side Rendering (CSR)
Client-side rendering (CSR) means rendering pages directly in the browser using JavaScript. All logic, data fetching, templating and routing are handled on the client rather than the server.

Client-side rendering can be difficult to get and keep fast for mobile. It can approach the performance of pure server-rendering if doing minimal work, keeping a tight JavaScript budget and delivering value in as few RTTs as possible. Critical scripts and data can be delivered sooner using HTTP/2 Server Push or <link rel=preload>, which gets the parser working for you sooner. Patterns like PRPL are worth evaluating in order to ensure initial and subsequent navigations feel instant.

#### Hydration
[source](https://en.wikipedia.org/wiki/Hydration_(web_development))

hydration or rehydration is a technique in which client-side JavaScript converts a static HTML web page, delivered either through static hosting or server-side rendering, into a dynamic web page by attaching event handlers to the HTML elements. Because the HTML is pre-rendered on a server, this allows for a fast "first contentful paint" (when useful data is first displayed to the user), but there is a period of time afterward where the page appears to be fully loaded and interactive, but is not until the client-side JavaScript is executed and event handlers have been attached.

Frameworks that use hydration include Next.js and Nuxt.js. React v16.0 introduced a "hydrate" function, which hydrates an element, in its API.


#### Progressive rehydration
In progressive rehydration, individual pieces of a server-rendered application are “booted up” over time, rather than the current common approach of initializing the entire application at once. This can help reduce the amount of JavaScript required to make pages interactive, since client-side upgrading of low priority parts of the page can be deferred to prevent blocking the main thread. It can also help avoid one of the most common server-side rendering rehydration pitfalls, where a server-rendered DOM tree gets destroyed and then immediately rebuilt – most often because the initial synchronous client-side render required data that wasn't quite ready, perhaps awaiting Promise resolution.

#### Partial rehydration
Partial rehydration has proven difficult to implement. This approach is an extension of the idea of progressive rehydration, where the individual pieces (components/views/trees) to be progressively rehydrated are analyzed and those with little interactivity or no reactivity are identified. For each of these mostly-static parts, the corresponding JavaScript code is then transformed into inert references and decorative functionality, reducing their client-side footprint to near-zero. The partial hydration approach comes with its own issues and compromises. It poses some interesting challenges for caching, and client-side navigation means it cannot be assumed that server-rendered HTML for inert parts of the application will be available without a full page load.[2]

One framework that supports partial rehydration is Elder.js, which is based on Svelte.

#### Trisomorphic rendering
Trisomorphic rendering is a technique which uses streaming server-side rendering for initial/non-JS navigations, and then uses service worker to take on rendering of HTML for navigations after it has been installed. This can keep cached components and templates up to date and enables SPA-style navigations for rendering new views in the same session. This approach works best when one can share the same templating and routing code between the server, client page, and service worker.
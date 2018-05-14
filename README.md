# Path to React 16

Notes on features in React 16 and upgrading from v15

## React 15.0.0

**DOM Interactions improvements**

* Gets rid of the`data-reactid` attribute on every node and makes the DOM lighter.
    * Note: data-reactid is still present for server-rendered content, however it is much smaller than before and is simply an auto-incrementing counter.

* Now uses `document.createElement` instead of setting `innerHTML` when mounting components.
    * Using document.createElement is also faster in modern browsers and fixes a number of edge cases related to SVG elements and running multiple copies of React on the same page. 

* Support for all SVG attributes supported by the browser.

* No longer emits extra <span> nodes around the text, making the DOM output much cleaner.

* Functional components can now return null too

* Rendering null now uses comment nodes
    * Rendering to null was a feature we added in React 0.11 and was implemented by rendering `<noscript>` elements. This has improved React performance for many typical applications.

Blog post: https://reactjs.org/blog/2016/04/07/react-v15.html#major-changes


## React 15.2.0

* React Error Code System - Makes debugging in production easier
    * A gulp script that collects all invariant error messages and folds them to a JSON file, and at build-time Babel uses the JSON to rewrite our invariant calls in production to reference the corresponding error IDs. 
    Now when things go wrong in production, the error that React throws will contain a URL with an error ID and relevant information. The URL will point you to a page in our documentation where the original error message gets reassembled.
    * Eg. https://reactjs.org/docs/error-decoder.html?invariant=109&args%5B%5D=Foo
    
Blog Post: https://reactjs.org/blog/2016/07/11/introducing-reacts-error-code-system.html    
    
## React 15.4.0

* Separating React and ReactDOM
    * More than a year ago, we started separating React and React DOM into separate packages. 
    We deprecated React.render() in favor of ReactDOM.render() in React 0.14, and removed DOM-specific APIs from React completely in React 15. 
    However, the React DOM implementation still [secretly lived inside the React package](https://www.reddit.com/r/javascript/comments/3m6wyu/found_this_line_in_the_react_codebase_made_me/cvcyo4a/).
    * In React 15.4.0, we are finally moving React DOM implementation to the React DOM package. 
    The React package will now contain only the renderer-agnostic code such as React.Component and React.createElement().
    
* Profiling Components with Chrome Timeline
    * You can now visualize React components in the Chrome Timeline. This lets you see which components exactly get mounted, updated, and unmounted, how much time they take relative to each other.
        * To do this load your app with `?react_perf` in the query string and record perf with Chrome Dev tools.
        
* Mocking Refs for Snapshot Testing
    * If you’re using Jest snapshot testing, you might have had issues with components that rely on refs. With React 15.4.0, we introduce a way to provide mock refs to the test renderer.             

Blog Post: https://reactjs.org/blog/2016/11/16/react-v15.4.0.html


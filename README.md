# Path to React 16

Notes on features in React 16 and upgrading from v15

### React Fiber



### React 16

* New render return types: fragments and strings
    * You can now return an array of elements from a component’s render method. Like with other arrays, you’ll need to add a key to each element to avoid the key warning
    Starting with React 16.2.0, there is support for a special fragment syntax to JSX that doesn’t require keys.
    * There is now support for returning strings, too.
    
* Better error handling via Error boundaries
    * Previously, runtime errors during rendering could put React in a broken state, producing cryptic error messages and requiring a page refresh to recover. 
    To address this problem, React 16 uses a more resilient error-handling strategy. 
    By default, if an error is thrown inside a component’s render or lifecycle methods, the whole component tree is unmounted from the root. 
    This prevents the display of corrupted data. However, it’s probably not the ideal user experience.
    * Instead of unmounting the whole app every time there’s an error, you can use error boundaries. Error boundaries are special components that capture errors inside their subtree and display a fallback UI in its place. 
    Think of error boundaries like try-catch statements, but for React components.

* Portals
    * Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.
    * ```javascript
          render() {
            // React does *not* create a new div. It renders the children into `domNode`.
            // `domNode` is any valid DOM node, regardless of its location in the DOM.
            return ReactDOM.createPortal(
              this.props.children,
              domNode,
            );
          }

        ```      
* Better server-side rendering
    * Includes a completely rewritten server renderer. It’s really fast. It supports streaming, so you can start sending bytes to the client faster.
    * Is better at hydrating server-rendered HTML once it reaches the client. It no longer requires the initial render to exactly match the result from the server. 
    Instead, it will attempt to reuse as much of the existing DOM as possible. No more checksums! 
    In general, we don’t recommend that you render different content on the client versus the server, 
    but it can be useful in some cases (e.g. timestamps). However, it’s dangerous to have missing nodes on the server render as this might cause sibling nodes to be created with incorrect attributes.
    * According to Sasha’s synthetic benchmarks, server rendering in React 16 is roughly three times faster than React 15.
    
* Support for custom DOM attributes
    * Instead of ignoring unrecognized HTML and SVG attributes, React will now pass them through to the DOM. 
  This has the added benefit of allowing us to get rid of most of React’s attribute whitelist, resulting in reduced file sizes.
  
* Reduced file size
    * react is 5.3 kb (2.2 kb gzipped), down from 20.7 kb (6.9 kb gzipped).
    * react-dom is 103.7 kb (32.6 kb gzipped), down from 141 kb (42.9 kb gzipped).
    * react + react-dom is 109 kb (34.8 kb gzipped), down from 161.7 kb (49.8 kb gzipped).
      
      That amounts to a combined 32% size decrease compared to the previous version (30% post-gzip).
    * The size difference is partly attributable to a change in packaging. React now uses Rollup to create flat bundles for each of its different target formats, resulting in both size and runtime performance wins. 
    The flat bundle format also means that React’s impact on bundle size is roughly consistent regardless of how you ship your app, whether it’s with Webpack, Browserify, the pre-built UMD bundles, or any other system.
    
* Breaking changes list [here](https://reactjs.org/blog/2017/09/26/react-v16.0.html#breaking-changes)    
    
* Deprecations
    * Hydrating a server-rendered container now has an explicit API. If you’re reviving server-rendered HTML, use ReactDOM.hydrate instead of ReactDOM.render. 
    Keep using ReactDOM.render if you’re just doing client-side rendering.
    * Support for React Addons has been discontinued.
    * `react-addons-perf` no longer works in React 16. In the meantime, we can use Chrome's performance audity tools to profile React components.          
    
Blog post: https://reactjs.org/blog/2017/09/26/react-v16.0.html      

### React 15.0.0

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


### React 15.2.0

* React Error Code System - Makes debugging in production easier
    * A gulp script that collects all invariant error messages and folds them to a JSON file, and at build-time Babel uses the JSON to rewrite our invariant calls in production to reference the corresponding error IDs. 
    Now when things go wrong in production, the error that React throws will contain a URL with an error ID and relevant information. The URL will point you to a page in our documentation where the original error message gets reassembled.
    * Eg. https://reactjs.org/docs/error-decoder.html?invariant=109&args%5B%5D=Foo
    
Blog Post: https://reactjs.org/blog/2016/07/11/introducing-reacts-error-code-system.html    
    
### React 15.4.0

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

### React 15.5.0

* React.PropTypes and React.createClass are now in their own packages
    * The propTypes, contextTypes, and childContextTypes APIs will work exactly as before. The only change is that the built-in validators now live in a separate package.
    * You may also consider using Flow to statically type check your JavaScript code, including React components. 

* Discontinuing support for React Addons - listed [here](https://reactjs.org/blog/2017/04/07/react-v15.5.0.html#discontinuing-support-for-react-addons)

* React Test Utils moved
    * Currently, the React Test Utils live inside `react-addons-test-utils`. As of 15.5, we’re deprecating that package and moving them to `react-dom/test-utils` instead.
    This reflects the fact that what we call the Test Utils are really a set of APIs that wrap the DOM renderer.
    * The exception is shallow rendering, which is not DOM-specific. The shallow renderer has been moved to `react-test-renderer/shallow`.

* **Note:** If your app produces zero warnings in 15.5, it should continue to work in 16 without any changes.

Blog Post: https://reactjs.org/blog/2017/04/07/react-v15.5.0.html

### React 15.6.0

* Improving Inputs
    * the `onChange` event for inputs is a little bit more reliable and handles more edge cases.

* Less Noisy Deprecation Warnings
    * Downgraded deprecation warnings to use `console.warn` instead of `console.error` since this was causing noise and failing tests.

Blog Post: https://reactjs.org/blog/2017/06/13/react-v15.6.0.html

### React 15.6.2

* Fixes regressions for `onChange` event handling updates from last release.

Blog Post: https://reactjs.org/blog/2017/09/25/react-v15.6.2.html

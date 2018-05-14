# Path to React 16

Notes on features in React 16 and upgrading from v15

## React 15

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
    

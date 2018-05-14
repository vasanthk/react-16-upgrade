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

Blog post: https://reactjs.org/blog/2016/04/07/react-v15.html#major-changes


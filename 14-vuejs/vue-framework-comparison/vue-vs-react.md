#Vue vs React

React and Vue share many similarities. They both:

* utilize a virtual DOM
* provide reactive and composable view components
* maintain focus in the core library, with concerns such as routing and global state management handled by companion libraries


##Virtual DOM - Performance

Traditional JS renders the entire DOM more often than necessary, so DOM manipulation is usually resource-heavy and can be time-consuming. Both React and Vue make use of an abstraction of the DOM, or virtual DOM, to minimise the number of necessary DOM mutations, as nothing is rendered (yet). Updating the virtual DOM is a comparatively more efficient process; only the changed objects get updated in the real DOM. Vue exhibits better render/update performance over React as:

* it makes use of a more light-weight (i.e. less Javascript computation overheads) implementation of the Virtual DOM;
* Vue's functional components require less overhead than React's;
* re-render of the entire component in React vs render of only components that require re-rendering.

###Developer Experience

There is a trade-off between error checking and performance in development. While both Vue and React perform sufficiently well in most cases, React lags behind Vue when developing resource-intensive applications. However, Vue's emphasis on performance mean that its checks are not as comprehensive as React's.


##HTML & CSS

###JSX vs Templates

In React and Vue, components can express their UI within render functions using JSX (this is mandatory for React), so developers can make use of the 'power of a full programming language to build their view'. However, Vue introduces templates as the default option for components. These templates are HTML-compliant, and are easier to read and write.

###Component-Scoped CSS

Certain features of CSS-in-JS solutions for React may not be supported, or may require the re-invention of existing CSS methods. They may also contribute to a decrease in performance if not optimised properly. Single-file components (.vue files) do away with these issues as developers are able to author normal CSS within these files.


##Scale

Both Vue and React offer routing solutions for large applications, and also offer innovative state management solutions that integrate well for superior development experience. However, while Vue's companion libraries for state management and routing are officially supported and kept up-to-date with the core library, React's richer offerings are developed by the wider community.

React developers need to be informed about JSX and ES2015+ (related to React's class syntax), and build systems (for production), while much less time is required for a developer to get started with Vue.

##Native Rendering

React Native enables you to write native-rendered apps for iOS and Android using the same React component model so a developer can apply his/her knowledge of a framework across multiple platforms. While Vue has an equivalent of React Native (Weex), it is still in active development and is not as mature and battle-tested as React Native.

## References
* [Source](https://vuejs.org/v2/guide/comparison.html)
* [Virtual DOM](https://www.codecademy.com/articles/react-virtual-dom)

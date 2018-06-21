# Advanced View and Templates

### Layouts
Sometimes you want the same surrounding HTML for each template - for example your head or body tags don't change with each different page.

First, pass the relevant props to a layout component.

`views/layouts/default.jsx`:
```js
var React = require('react');

class DefaultLayout extends React.Component {
  render() {
    return (
      <html>
        <head><title>{this.props.title}</title></head>
        <body>{this.props.children}</body>
      </html>
    );
  }
}

module.exports = DefaultLayout;
```

We can use `this.props.children` to magically fill in things from our other file.

Then use that surrounding component inside your template.

`views/index.jsx`:
```js
var React = require('react');
var DefaultLayout = require('./layouts/default');

class HelloMessage extends React.Component {
  render() {
    return (
      <DefaultLayout title={this.props.title}>
        <div>Hello {this.props.name}</div>
      </DefaultLayout>
    );
  }
}

module.exports = HelloMessage;
```

### Sub Component Files

Components can be used to modularize views and reduce repetition. A common pattern is to move the header and footer of a page into separate views, or partials, then render them on each page.

#### Example

**views/components/header.jsx**
```
var React = require('react');

class Header extends React.Component {
  render() {
    return (
      <div class="header">
        <h1>This is the header</h1>
      </div>
    );
  }
}

module.exports = Header;
```

**views/hello-message.jsx**
```
var React = require('react');
var Header = require('./components/header');

class HelloMessage extends React.Component {
  render() {
    return (
      <Header/>
      <div>Hello {this.props.name}</div>
    );
  }
}

module.exports = HelloMessage;
```

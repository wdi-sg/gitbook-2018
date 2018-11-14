# webpacker

Rails was not concieved for an asset pipeline any more complex than it already is. What if you have to include other (npm) library dependencies before you compile the assets? What about sass that is declared within the component?

We need another system to run webpack on top of rails that could integrate with the asset pipeline.

From the official webpacker documentation: [https://github.com/rails/webpacker](https://github.com/rails/webpacker)


#### Generate a normal rails app, but with the required webpacker files:

Install the libraries you need:
```
brew install yarn
gem install webpacker
```

Create and initialize the app: (this one includes all the models and views)
```
rails new blog --webpack=react -d postgresql
cd blog
rails generate scaffold Post name:string title:string content:text
rails db:create
rails db:migrate
```

Check out your app:
```
rails s
```

You should have a working rails CRUD app à la unit 3.

#### Setting up the react app:

Now check out the app directory structure:

You should have a set of files in `app/javascript`
```yml
app/javascript:
  └── packs:
      # only webpack entry files here
      └── application.js
      └── hello_react.jsx
```

Create the root controller and view:
```
rails g controller onepage index
```

Set the root route in `config/routes.rb` to be the controller you just created:
```
root 'onepage#index'
```

Create the script tag for them in your view file `app/views/onepage/index.html.erb`:

The argument to javascript_pack_tag corresponds to the file in the packs folder.

```
<%= javascript_pack_tag 'hello_react' %>
```

If you are using react styles you can do this:
```
<%= stylesheet_pack_tag 'hello_react' %>
```

Your basic react setup is done.

Run the app to see it work:

Open up *two* terminals:

1.
```
./bin/webpack-dev-server
```

2.
```
rails server
```


### Start a complete react app:

In order to mimic what we had in the react setup we can have one file for `ReactDOM.render` and another containing our root component `App`.

Change `hello_react.jsx` to import the an `App` component.

hello_react.jsx
```
import React from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'
import App from './components/app'

document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <App/>,
    document.body.appendChild(document.createElement('div')),
  )
})

```

Make a directory that will contain the rest of your react app.
```
cd app/javascripts
mkdir components
touch app.jsx
```

app.jsx
```
import React from 'react'

export default class App extends React.Component{

  render(){
    return(<div>
            <h1>APPPPPPP!</h1>
          </div>);
  }
}
```

`app.jsx` is the place where you start writing your actual react app.

All other parts of your react app will go in the `components` directory. `packs` is just for the very top level files.

### Other Notes

#### Other assets in the rails asset pipeline:
Refer to this page of the webpacker docs: [https://github.com/rails/webpacker/blob/master/docs/assets.md](https://github.com/rails/webpacker/blob/master/docs/assets.md)

#### Production Build
```
./bin/webpack
```

See where these files are being built to:
```
ls -la public/packs
```

### Pairing Exercise
Create and run a react app inside a rails app, as above.

#### further
*Use ajax with rails:*

Create a post using the rails form.

You can get rails to send back json instead of rendering the view by doing `.json` at the end of the url.

```
http://localhost:3000/posts/1.json
```

In your react code, add a button or a `componentDidMount` method that makes an ajax request to that route. When the response comes back, render it.

```
componentDidMount() {

  var reactThis = this;

  var responseHandler = () => {
      var response = JSON.parse( this.responseText );
      reactThis.setState({stuff: response});
  };

  var request = new XMLHttpRequest();

  request.addEventListener("load", responseHandler);

  request.open("GET", "http://localhost:3000/posts/1.json");

  request.send();
}
```

#### further
Create a series of inputs and a button in your react app to make an AJAX call that will *create* a new post.

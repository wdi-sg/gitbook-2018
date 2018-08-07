# webpacker

Rails was not concieved for an asset pipeline any more complex than it already is. What if you have to include other (npm) library dependencies before you compile the assets? What about sass that is declared within the component?

We need another system to run webpack on top of rails that could integrate with the asset pipeline.

From the official webpacker documentation: [https://github.com/rails/webpacker](https://github.com/rails/webpacker)

```
gem install webpacker
rails new blog --webpack=react -d postgresql
cd blog
rails generate scaffold Post name:string title:string content:text
rails g controller onepage index
rails db:create
rails db:migrate
```

You should have a set of files in `app/javascript`
```yml
app/javascript:
  ├── packs:
  │   # only webpack entry files here
  │   └── application.js
  └── src:
  │   └── application.css
  └── images:
      └── logo.svg
```

Create the root view in `config/routes.rb`:
```
root 'onepage#index'
```

Create the script tag for them in your view file `app/views/onepage/index.html.erb`:
```
<%= javascript_pack_tag 'hello_react' %>
```

If you are using react styles you can do this:
```
<%= stylesheet_pack_tag 'hello_react' %>
```

### Start your complete react app:
```
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

### Other assets in the rails asset pipeline:
Refer to this page of the webpacker docs: [https://github.com/rails/webpacker/blob/master/docs/assets.md](https://github.com/rails/webpacker/blob/master/docs/assets.md)

### Development

Open up *two* terminals:

1.
```
./bin/webpack-dev-server
```

2.
```
rails server
```

### Production
```
./bin/webpack
```

See where these files are being built to:
```
ls -la public/packs
```

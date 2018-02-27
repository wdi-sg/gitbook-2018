# Basic HTML Template

Between custom CSS, custom JavaScript, including Bootstrap
there's now a lot we need to write to get a basic web page going. Here's a
reference for you that you can copy and paste into your `index.html` file
whenever you start a new project.

Features:
- a reference to `main.js` for custom JavaScript!
- a reference to `style.css` for custom CSS styles!
- Bootstrap already all hooked up!
- A simple HTML page already styled up with Bootstrap classes!

Copy and paste! Merry Christmas!!

# The Template

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My Web Page</title>

    <!-- Links to your custom JavaScript and CSS -->
    <link rel="stylesheet" href="style.css">
    <script src="main.js"></script>

    <!-- Bootstrap libraries -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>

  </head>
  <body>
    <div class="container">
      <h1>My Web Page</h1>
      <div class="row">
        <div class="col-xs-12">
          <p>
          Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor
          incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud
          exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure
          dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
          Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt
          mollit anim id est laborum.
          </p>
        </div>
      </div>
      <div class="row">
        <div class="col-xs-6">
          <p>Always 1/2 half page column.</p>
        </div>
        <div class="col-xs-6">
          <p>Always 2/2 half page column.</p>
        </div>
      </div>
      <div class="row">
        <div class="col-lg-3 col-md-6 col-xs-12">
          <p>Column 1 resizes according to page size.</p>
        </div>
        <div class="col-lg-3 col-md-6 col-xs-12">
          <p>Column 2 also resizes.</p>
        </div>
        <div class="col-lg-3 col-md-6 col-xs-12">
          <p>Column 3 resizes too.</p>
        </div>
        <div class="col-lg-3 col-md-6 col-xs-12">
          <p>Column 4 also too does indeed resize.</p>
        </div>
      </div>
    </div>

  </body>
</html>
```

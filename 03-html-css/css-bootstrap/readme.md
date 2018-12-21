# Intro to Bootstrap

---

## Objectives

* Understand what front-end CSS frameworks are
* Utilize a front-end grid system for mobile and desktop layout
* Comprehend documentation and implement an unfamiliar framework

---


## Bootstrap

A little while back, a couple wonderful folks at Twitter created a front end framework called Bootstrap to make responsive web development much easier. Bootstrap is extremely popular and knowledge of at least one CSS framework is a very valuable skill to have. Bootstrap comes with a ton of features including a responsive grid system, buttons, icons and some very nifty JavaScript plugins.

---

### How to include it

You can include Bootstrap multiple ways, the easiest to start is a CDN. As we get to use Rails and more robust tooling, we can also use package managers. Our current setup won't support this, but we'll use it in time!

1. CDN (content delivery network - someone else hosts the library/framework and you access it via a URL) 
```
Stylesheet

<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">

Scripts for Bootstrap animations (order is important)

<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>

```
2. Include the actual CSS and JS files - great for offline development

---

## Starter Template
```
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
  </body>
</html>
```

### Start with a container

To make sure that all your bootstrap styles behave properly, it's always best to put your content inside an element with a class "container" (usually a div). If you would like to use the full width of the screen use `class = "container-fluid"`

---

### Layout/Grid (row class, spans, offset, nesting)

The bootstrap grid is based on 12 columns that can are accessible using by placing the columns in `<div class = "row">` (you must place your columns in a row) and then you use the following classes for these screen sizes.

```
// Extra small devices (portrait phones, less than 576px)
// No media query since this is the default in Bootstrap

// Small devices (landscape phones, 576px and up)
@media (min-width: 576px) { ... }

// Medium devices (tablets, 768px and up)
@media (min-width: 768px) { ... }

// Large devices (desktops, 992px and up)
@media (min-width: 992px) { ... }

// Extra large devices (large desktops, 1200px and up)
@media (min-width: 1200px) { ... }
```

---

Here is an example of an two column layout.

```html
<div class="row">
  <div class="col-md-6">.col-md-6</div>
  <div class="col-md-6">.col-md-6</div>
</div>
```

You can see some more good examples [here](http://getbootstrap.com/css/#grid)

---

You can also offset and nest your columns. When you offset a column, you add a column of whitespace and push the column to the right. Here is a example

```html
<div class="row">
  <div class="col-md-3 col-md-offset-3">
    This column takes 1/4 of the width of the page and is moved to the  right by 1/4 of the page
  </div>
</div>
```

---

Here is an example of nesting columns (putting one row inside another)

```html
<div class="row">
  <div class="col-sm-6">
    Level 1: Column takes 1/2 the width of the page
    <div class="row">
      <div class="col-sm-4">
        Level 2: This column takes 1/3 the width of it's parent column
      </div>
      <div class="col-sm-8">
        Level 2: This column takes 2/3 the width of it's parent column
      </div>
    </div>
  </div>
</div>
```

---

### Buttons/positioning

To align text, use these classes.

```html
<p class="text-left">Left aligned text.</p>
<p class="text-center">Center aligned text.</p>
<p class="text-right">Right aligned text.</p>
<p class="text-justify">Justified text.</p>
<p class="text-nowrap">No wrap text.</p>
```

---

### Typography (lead, muted, warning/error/success/info, small>cite attr -> cite title = "test")

Bootstrap also comes with some nice styles to improve the quality of your typography including:

```html
<p class="lead">This text will stand out in a paragraph</p>

<small>This line of text is meant to be treated as fine print.</small>

<p class="text-lowercase">Lowercased text.</p>
<p class="text-uppercase">Uppercased text.</p>
<p class="text-capitalize">Capitalized text.</p>
```

---

### lists (unstyled class removed padding and bullets class inline to display on the same line")

You can also use Bootstrap to style your lists and remove bullet points and margin

```html
<ul class="list-unstyled">
  <li>I will have no list-style and left-margin</li>
  <li>Me neither!</li>
</ul>
```

You can also style them to be inline (good for navigation)

```html
<ul class="list-inline">
  <li>About</li> |
  <li>Pricing</li> |
  <li>Contact</li>
</ul>
```

---

### Tables

Bootstrap is really awesome at formatting tables for you and with only a couple classes you can have some spiffy looking tables. Add `class="table"` to your table tag to include this and if you would like a striped design include the class `table-striped` to your table tag. The table-striped will only add stripes to whatever is in your `tbody` tag. If you would like borders as well include `table-bordered` in your table tag.

---

### Buttons (link, xs, sm, lg, block, disabled)

Bootstrap comes with quite a few button default sizes and colors, to add these make sure you add a `class = btn btn-___` Bootstrap defines  these as:

```html
<!-- Standard button -->
<button type="button" class="btn btn-default">Default</button>

<!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
<button type="button" class="btn btn-primary">Primary</button>

<!-- Indicates a successful or positive action -->
<button type="button" class="btn btn-success">Success</button>

<!-- Contextual button for informational alert messages -->
<button type="button" class="btn btn-info">Info</button>

<!-- Indicates caution should be taken with this action -->
<button type="button" class="btn btn-warning">Warning</button>

<!-- Indicates a dangerous or potentially negative action -->
<button type="button" class="btn btn-danger">Danger</button>

<!-- Deemphasize a button by making it look like a link while maintaining button behavior -->
<button type="button" class="btn btn-link">Link</button>
```

You can also add .btn-lg, .btn-sm, or .btn-xs for additional sizes.

---

### Images (img-rounded, img-responsive, img-circle)

Bootstrap helps you format images using img-rounded (rounds the corners), img-circle (makes the image a circle) and img-thumbnail (adds a border). You can also add a class of img-responsive to your image to make it scale well when the screen size changes (this sets its max-width to 100% and the height to auto)

---

### Forms

Bootstrap is also very helpful when you need to style your forms. All textual `<input>, <textarea>, and <select>` elements with `.form-control `are set to width: 100%; by default. Wrap labels and controls in .form-group for optimum spacing. You can create horizontal and inline forms and style each of your inputs and validations as well. Read more about form styling [here](http://getbootstrap.com/css/#forms)

---

### JavaScript + Bootstrap

Bootstrap can also do some nifty things for you with it's JavaScript plugins. This includes carousels, modals, popovers, dropdowns and other nice pieces of functionality that will really spruce up your app. Always make sure you understand what the code is doing before copying and pasting it. Fortunately, this is not too challenging and Bootstrap has excellent documentation. As always, if you're confused or things are breaking - google around. Bootstrap is pretty much ubiquitous and it is likely that the problems you have, other people have had (and hopefully solved) as well.

### Further Reading: Naming Conventions
[https://medium.freecodecamp.org/css-naming-conventions-that-will-save-you-hours-of-debugging-35cea737d849](https://medium.freecodecamp.org/css-naming-conventions-that-will-save-you-hours-of-debugging-35cea737d849)

### Pairing Exercise:
Using this HTML:
```HTML
<!DOCTYPE html>
<html>
  <head>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
    <style>
        .left-col{
            background-color:red;
        }
        .center-col{
            background-color:yellow;
        }
        .right-col{
            background-color:green;
        }
    </style>
  </head>
  <body>
    <div class="left-col">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean laoreet at est nec maximus. Etiam scelerisque ante et neque commodo blandit. Integer ut rutrum sapien. Mauris lacus tortor, mattis aliquam velit ac, varius commodo nisi. Cras sit amet est eget nulla sollicitudin convallis. Fusce non nulla ultricies, tincidunt ligula nec, interdum sem. Suspendisse ultricies nec libero ac sodales. Suspendisse lorem purus, bibendum ac tristique nec, congue in metus. Morbi et lobortis massa, vitae fermentum velit. Cras egestas quis lectus ac rutrum. Quisque cursus nibh sed dolor bibendum finibus. Sed aliquet, tellus sit amet sollicitudin laoreet, eros massa condimentum diam, a imperdiet lectus lectus fringilla eros. Fusce lacinia eget metus et condimentum. Aenean diam libero, cursus eu purus id, tincidunt posuere orci. Cras et lobortis purus.

        Maecenas dictum neque at magna bibendum, nec pharetra tortor cursus. Donec in orci augue. In faucibus magna faucibus lobortis lobortis. Donec ornare aliquet dapibus. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Duis ultricies velit sit amet purus fringilla, quis interdum tortor tincidunt. Mauris consequat ut erat sit amet tempor. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Proin et pretium orci. Praesent vel volutpat dui.
    </div>
    <div class="center-col">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean laoreet at est nec maximus. Etiam scelerisque ante et neque commodo blandit. Integer ut rutrum sapien. Mauris lacus tortor, mattis aliquam velit ac, varius commodo nisi. Cras sit amet est eget nulla sollicitudin convallis. Fusce non nulla ultricies, tincidunt ligula nec, interdum sem. Suspendisse ultricies nec libero ac sodales. Suspendisse lorem purus, bibendum ac tristique nec, congue in metus. Morbi et lobortis massa, vitae fermentum velit. Cras egestas quis lectus ac rutrum. Quisque cursus nibh sed dolor bibendum finibus. Sed aliquet, tellus sit amet sollicitudin laoreet, eros massa condimentum diam, a imperdiet lectus lectus fringilla eros. Fusce lacinia eget metus et condimentum. Aenean diam libero, cursus eu purus id, tincidunt posuere orci. Cras et lobortis purus.

        Maecenas dictum neque at magna bibendum, nec pharetra tortor cursus. Donec in orci augue. In faucibus magna faucibus lobortis lobortis. Donec ornare aliquet dapibus. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Duis ultricies velit sit amet purus fringilla, quis interdum tortor tincidunt. Mauris consequat ut erat sit amet tempor. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Proin et pretium orci. Praesent vel volutpat dui.


    </div>
    <div class="right-col">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean laoreet at est nec maximus. Etiam scelerisque ante et neque commodo blandit. Integer ut rutrum sapien. Mauris lacus tortor, mattis aliquam velit ac, varius commodo nisi. Cras sit amet est eget nulla sollicitudin convallis. Fusce non nulla ultricies, tincidunt ligula nec, interdum sem. Suspendisse ultricies nec libero ac sodales. Suspendisse lorem purus, bibendum ac tristique nec, congue in metus. Morbi et lobortis massa, vitae fermentum velit. Cras egestas quis lectus ac rutrum. Quisque cursus nibh sed dolor bibendum finibus. Sed aliquet, tellus sit amet sollicitudin laoreet, eros massa condimentum diam, a imperdiet lectus lectus fringilla eros. Fusce lacinia eget metus et condimentum. Aenean diam libero, cursus eu purus id, tincidunt posuere orci. Cras et lobortis purus.

        Maecenas dictum neque at magna bibendum, nec pharetra tortor cursus. Donec in orci augue. In faucibus magna faucibus lobortis lobortis. Donec ornare aliquet dapibus. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Duis ultricies velit sit amet purus fringilla, quis interdum tortor tincidunt. Mauris consequat ut erat sit amet tempor. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Proin et pretium orci. Praesent vel volutpat dui.


    </div>
  </body>
</html>
```

Create a 3 column layout that uses the bootstrap CSS grid.

Add HTML elements and CSS classes as you see fit.

![Imgur](https://i.imgur.com/6IcKOjZ.png)

Your final page should naturally collapse down when the browser size gets narrow enough.

![Imgur](https://i.imgur.com/cnaI0rc.png)

If you get done with that, try adding more columns and rows, and then change the responsive breakpoint. (sm, md, lg, etc.)

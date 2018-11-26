# Display

Display has to do with the flow of a document and what elements take up what space.

Elements are processed like in a text document, they flow from the top left to the right and then towards the bottom line by line.

Different elements have different kinds of properties for how much space they will take up and how.

Certain elements always have certain default display properties:


| element | display |
|--- | --- |
| p | block |
| div | block |
| h tag (h1) | block |
| input | inline |
| img | inline |
| a | inline |
| span | inline |

## Taking Up Space using Display

![https://media.giphy.com/media/13yeYZJuM4LpjW/giphy.gif](https://media.giphy.com/media/13yeYZJuM4LpjW/giphy.gif)

From here: [https://stackoverflow.com/questions/9189810/css-display-inline-vs-inline-block](https://stackoverflow.com/questions/9189810/css-display-inline-vs-inline-block)

### Inline elements:

  - respect left & right margins and padding, but not top & bottom
  - cannot have a width and height set
  - allow other elements to sit to their left and right.
  - you don't usually style these elements heavily with box model styles

  Example: text styling
```css
.highlight{
  background-color:yellow;
  padding:3px;
}
```

```html
<p>
  To operate your life vest <span class="highlight">pull on the red strap</span>. If it does not inflate, blow on the attached tube.
</p>
```

### Block elements:

    - respect left & right margins and padding & top & bottom
    - force a line break after the block element
    - acquires full-width if width not defined

Example: Layout rows:
```css
.row1{
  background-color:yellow;
}
.row2{
  background-color:blue;
}
.row3{
  background-color:red;
}
```

```html
<div class="row1">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer luctus a nulla in efficitur.
</div>
<div class="row2">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer luctus a nulla in efficitur.
</div>
<div class="row3">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer luctus a nulla in efficitur.
</div>
```

### Inline-block elements:

    - allow other elements to sit to their left and right
    - respect top & bottom margins and padding
    - respect height and width
    - can create a line break if element is longer than width (doesn't break in the middle of the element- like a block)


Example: Nav Bar Layout:
```css
.nav-button{
  display:inline-block;
  margin:0;
  padding:5px;
  width: 200px;
}
.nav1{
  background-color:yellow;
}
.nav2{
  background-color:blue;
}
.nav3{
  background-color:red;
}
```

```html
<div class="nav-button nav1">
  About
</div>
<div class="nav-button nav2">
  Sign Up
</div>
<div class="nav-button nav3">
  Login
</div>
```



## Positioning

Another CSS property, "position", can take `relative` or `absolute` values, among others.

Position is another dimension of how an element gets displayed- generally you use it to override some kind of display type property- or, how an element is fitted into the series of boxes in the document flow.

More specifically you use it when you want to do something very specific with the position of an element.

### How to Position
Setting a position other than `static` on an element means that you get to use more specific position attributes: `top`, `right`, `bottom` and `left` (it's always in that order)

You can set these with:
- px
- em
- rem
- percent

---

#### Relative Positioning

Declaring `position:relative` allows you to position the element top, bottom, left, or right relative to where it would normally occur.

This is mostly only used in combination with absolutely positioned elements.
---


#### Absolute Positioning

Specifying `position: absolute` _removes the element from the document flow_ and places it exactly where you tell it to be.

Example: Modal:
```css
.modal{
  position:absolute;
  background-color:black;
  color:white;
  top:30px;
}
```

```html
<div class="modal">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer luctus a nulla in efficitur. Fusce venenatis velit id leo sollicitudin pretium. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Maecenas aliquet, velit ac tempor bibendum, mi arcu euismod diam, ut vestibulum leo augue vitae metus. Proin finibus dui sed aliquam varius. Integer quis massa urna. Fusce vitae lacus sed nunc tincidunt sagittis. Sed id ligula id lacus sodales vehicula in eget erat. Pellentesque condimentum massa nulla, sit amet ultrices mauris ornare a. Donec dolor elit, pretium sed nulla ac, sollicitudin fringilla nunc. Nunc tincidunt mollis purus sit amet sagittis.
</div>
```

---

#### Using Absolute + Relative Together

When you put an absolutely positioned element inside a relatively positioned one, the absolutely positioned element's position is absolute to the parent element.

This is usually used for two things:

1. In layout when you have fixed width cards, but you want your design to be mobile and desktop responsive, you change the number of cards within the row.

*But* the position of all elements within the card stays the same.

```css
.card{
  width:300px;
  height:350px;
  display:inline-block;
  position:relative;
}
.card-img{
  height: 200px;
  width: 200px;
  margin:0 auto;
}
.card-like{
  font-size:9px;
  position:absolute;
  color:white;
  font-weight:800;
  right:0;
  top:0;
}
```

```html
<div class="card">
  <img class="card-img" src="https://i.imgur.com/6yHmlwT.jpg"/>
  <p class="card-like">
    like
  </p>
</div>
```

1. When you have a shopping cart icon and want to dynamically indicate how many items are inside, absolutely position the number above and over the icon. Or, when you have a like button icon that always sits in the upper right hand corner of the card image.

![Imgur](https://i.imgur.com/I22mFsc.png)

---

#### Fixed Positioning

An element with fixed position is positioned relative to the browser window.  It will not move even if the window is scrolled, so a fixed positioned element will stay right where it is.

Example: Nav Bar Layout:
```css
.nav{
  z-index:1;
  position:fixed;
}
```

```html
<div class="nav">
  <div class="nav-button nav1">
    About
  </div>
  <div class="nav-button nav2">
    Sign Up
  </div>
  <div class="nav-button nav3">
    Login
  </div>
</div>
```

---

#### Static Positioning

HTML elements are positioned static by default. A "static positioned" element is always positioned according to the normal flow of the page and are not affected by the top, bottom, left, and right properties.

Again, the default positioning for all elements is static. This means that no positioning has been applied and the elements occurs where they normally would in the document.

```css
.static-item {
  position: static;
}
```

You almost never explicitly declare `position:static` like this because it is the default.

#### z-index

Explicitly sets an element to overlap on top of another, regardless of what order in the file that element was written.

A higher z-index means that element will appear on top on another element.

```
.gold-box {
  position: absolute;
  z-index: 3; /* put .gold-box above .green-box and .dashed-box */
  background: gold;
  width: 100%;

}
.green-box {
  position: absolute;
  z-index: 2; /* put .green-box above .dashed-box */
  background: lightgreen;
  left: 65%;
  top: -25px;
  height: 7em;
}
```

```
<span class="gold-box">Gold box</span>
<span class="green-box">Green box</span>
```

From: [https://developer.mozilla.org/en-US/docs/Web/CSS/z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/z-index)

### Pairing Exercises

Create an empty html file and css file.

You will be creating a bunch of divs:

```
<div class="box-demo">box demo</div>
```

Set a background-color and border on each div so you can see it's behavior.
```
.box-demo{
  background-color:yellow;
  border:1px solid black;
}
```

##### Display Inline

Create a series of divs as above and add the class `inline-demo`. In your CSS file set these divs to display inline.

Open the chrome console and see these elements behave weird if you put too much padding and margin on them (specifically top and bottom margin /padding)

Resize the width of the screen and see how the boxes flow around the screen and wrap.

How do these divs wrap when you have a long string inside with no spaces? i.e.: `983749012739407129034719023740912837017230971209347129038740192837490128739047182390871290387401923874091287340981723498`

##### Display Inline Block

Create a series of divs as above and add the class `inline-block-demo`. In your CSS file set these divs to display inline-block.

Resize the width of the screen and see how the boxes flow around the screen.

What happens when you put these elements around other inline elements?

```
====== inline element ======
====== inline element ======
====== inline element ======
=== inline-block element ===
=== inline-block element ===
====== inline element ======
=== inline-block element ===
```

##### Display Block
Create a series of divs as above and add the class `block-demo`. In your CSS file set these divs to display block.

Resize the width of the screen and see how the boxes flow around the screen.

In the chrome console, explicitly set the width of the block elements to less than 100%. See what happens when you make them narrower and narrower.

What happens when you put these narrow elements around other kinds of display elements?

```
====== inline element ======
====== inline element ======
====== block  element ======
====== block  element ======
====== block  element ======
====== inline element ======
=== inline-block element ===
=== inline-block element ===
====== inline element ======
=== inline-block element ===
```

##### Position Relative
Use the chrome console to position an element relative. Set the top, bottom, left or right properties. Set them with pixels or also percentage.

##### Position Relative w/ Absolute
Run the above code for position relative / absolute.

##### Position Fixed
Using the chrome console, apply position fixed to an element.

##### Further
Recreate the examples shown on the CSS layout page.

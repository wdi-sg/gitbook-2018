# CSS Layouts
Creating modern web page layouts usually takes a couple of patterns:

## Column Widths

![https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/GA.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/GA.png?raw=true)


Column widths use percentages to split up inline-block divs into equal (or offset to predetermined dinmension) columns.

`display:inlne-block` makes the divs sit next to each other.

In this example we set a proportional width and margin so that we can have 2 columns that are equal width.

In this layout the text will flow to whatever height it wants.

```
.column{
  display:inline-block;
  width:48%;
  margin-left:1%;
  margin-right:1%;
}
```

## Variable Count Card Row

![https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/airbnb.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/airbnb.png?raw=true)

Card layouts use fixed pixel width cards to arrange a number of the across the width of the screen.

Fixed width and `display:inline-block` makes the cards take up a variable number per row.
```
.card{
  display:inline-block;
  width:100px;
}
```

## Card with position relative - absolute

Cards with visual elements inside can be absolutely positioned when the width is fixed.

```
.card-button{
  position:absolute;
  top: 3px;
  right: 3px;
  width:10px;
  height:10px;
}

.card{
  position:relative;
  ...
```

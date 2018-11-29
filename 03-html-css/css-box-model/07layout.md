# CSS Layouts
Creating modern web page layouts usually takes a couple of patterns.


![https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/GA.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/GA.png?raw=true)

Each part of the page can be split into separate visual boxes that need to be created inside the HTML. These boxes will take up certain space or be positioned according to CSS `display` and/or position.


The first task of creating a layout is to decide what boxes the layout is composed of.

![https://github.com/wdi-sg/gitbook-2018/blob/master/images/GA-b.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/master/images/GA-b.png?raw=true)


Let's look at a couple of CSS techniques that you can use to create common web layouts:

## Column Widths


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

### Pairing Exercise

Create a web layout.

Make up a page to create.

Some suggestions:
- a pizza restaurant
- a hair salon
- a dentist office
- or you can just use [lorem ipsum](https://www.lipsum.com/)

1. Create a 2 column layout using percentage widths.
![https://github.com/wdi-sg/gitbook-2018/blob/master/images/basic.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/master/images/basic.png?raw=true)

2. Add a header.
![https://github.com/wdi-sg/gitbook-2018/blob/master/images/header.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/master/images/header.png?raw=true)

3. Add a footer.
![https://github.com/wdi-sg/gitbook-2018/blob/master/images/footer.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/master/images/footer.png?raw=true)

4. In one column create 2 or more cards that have a title and some text in them.

5. Use position absolute/relative to put this smiley emoji in the upper right corner of the card.
[https://github.com/wdi-sg/gitbook-2018/blob/master/images/smile.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/master/images/smile.png?raw=true)

5. Create new content for a 3 column layout.

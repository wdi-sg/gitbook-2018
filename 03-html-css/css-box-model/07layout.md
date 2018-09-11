# CSS Layouts
Creating modern web page layouts usually takes a couple of patterns:

## Column Widths

![https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/GA.png?raw=true](https://github.com/wdi-sg/gitbook-2018/blob/e91332410ceabb969dea3cddca611811fbe29678/images/GA.png?raw=true)

`display:inlne-block` makes the divs sit next to each other.
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

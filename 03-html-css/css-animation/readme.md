# CSS ANIMATIONS

## CSS Transitions
CSS Transitions are settings on an element that tell the browser how to animate *into* another state.

#### Example:

Element in it's natural state
```
.box{
  background-color:red;
}
```

Element in it's new state
```
.box:hover{
  background-color:green
}
```

Add a CSS transition in order to say how the element should transition out of it's *CURRENT* state into another.
```
.box{
  background-color:red;
  transition: all 0.5s ease-out 1s;
}
```

### Transitions Syntax

Similar to `border` transitions are normally set in a shorthand.
```
/*          property name | duration | timing function | delay */

transition: all             0.5s       ease-out          1s;
```

#### transition properties
You can set which css properties this transition applies to
```
.box{
  background-color:red;
  transition: background-color 0.5s ease-out 1s;
}
```

#### transition timing function
You can change the speed function of the transition.

The main ones are keyword values, although you are able to set number values as well.
Transition timing named values:
- ease
- ease-in
- ease-out
- ease-in-out
- linear
- step-start
- step-end

#### Resources
[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)


## CSS Animation Keyframes
If your animation is more than just a transition from one state to another, you can use CSS `keyframe`s to specify intermediate states.

All of the above attributes still apply.

*Example:*
As a box moves from one side of the screen to the next, it will dip and come back up.
```
<div class="box">
  hello
</div>
```
Generic styles for a box
```
div{
  height:100px;
  width:100px;
  background-color:#E9BAB0;
  position:absolute;
}
```
Set the animation style properties for this box
```
.box{
  animation-duration: 3s;
  animation-name: slider;
  animation-direction: alternate;
  animation-iteration-count: infinite;
}
```
If we wanted the box to move from left to right, we could have just used a transition.

If we want it to dip we can add a `50%` keyframe that specifies a `top` property that will happen in the middle of the transition / animation.
```
@keyframes slider {
  0% {
    right:calc(100% - 100px);
    left:0;
    top:0;
  }
  50% {
    top:290px;
  }
  100% {
    top:0;
    left:calc(100% - 100px);
    right:0;
  }
}
```

#### animation-name
Name your animation using `animation-name`, then refer to it in an `@keyframes` declaration.

#### Resources
[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)

#CSS Notes

## centering

### horizontal
* inline or inline-* elements width is less than it's container
```css
.horizontal-center-singleline { text-align: center; }
```

* block element
```css
.horizontal-center { maring: 0 auto; }
```

* inline-block or block element's width is bigger than its container
  1. using wrapper 
  ``` html
    <div class="container">
      <div class=“wrapper">
        <img src="image.png">
      </div>
    </div>
  ```
  ```
  .container {
    width: 100%;
    overflow: hidden;
  }
  .wrapper {
    display: inline-block; /* make wrapper width equal img's width */
    position: relative;
    right: -50%; /*or left: 50%; move wrapper position to 50% container's width position */
  }
  .wrapper img {
    position: relative; 
    left:-50%;  /* move img position to 50% wrapper or img width position backwords */
   }
  ```
 2. using CSS3 transform, 
 ```css
 img {
  margin-left: 50%;
  transform: translateX(-50%);
 }
 ```
Above ways is using the left side to align the middle. If using right, must handle the img width unknown, `position: absolute; right: 50%` will reposition image, but there another problem, the wrapper height is not cotain the image's height, and there may be other problems as the container css properties different.

3. using invisible ghost element and `vertical-align`
```
.ghost-center {
  position: relative;
}
.ghost-center::before {
  content: " ";
  display: inline-block;
  height: 100%;
  width: 1%;
  vertical-align: middle;
}
.ghost-center p {
  display: inline-block;
  vertical-align: middle;
}
```

### vertical
* using <table> element
```html
<table style="width: 100%;">
  <tr>
     <td style="text-align: center; vertical-align: middle;">
          Unknown stuff to be centered.
     </td>
  </tr>
</table>
```
* using `display: table & table-cell`
```html
<div class="something-semantic">
   <div class="something-else-semantic">
       Unknown stuff to be centered.
   </div>
</div>
```
```css
.something-semantic {
  display: table;
  width: 100%;
}
.something-else-semantic {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
```
* CSS3 transform
```css
div.container3 {
   height: 10em;
   position: relative
} 
div.container3 p {
   margin: 0;
   position: absolute;  
   top: 50%; 
   transform: translate(0, -50%);
}
```

### CSS3 Flex
```
.container {
  display: flex;
  justify-content: center; /* main axis center */
  align-items: center;
}

/* vertical only */
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
}
```

* [CENTERING THINGS](https://www.w3.org/Style/Examples/007/center.en.html)
* [Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)
* [Centering in the Unknown](https://css-tricks.com/centering-in-the-unknown/)
* [How do I center an image if it's wider than its container?](https://stackoverflow.com/questions/3300660/how-do-i-center-an-image-if-its-wider-than-its-container)
* [Centering Mixin](https://css-tricks.com/snippets/sass/centering-mixin/)

## Responsive

* [移动端页面开发适配 rem布局原理](https://segmentfault.com/a/1190000007526917)
* [移动端页面布局及字体大小该如何设置](https://segmentfault.com/a/1190000004344753)

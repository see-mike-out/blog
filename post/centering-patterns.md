# Centering DOMs in CSS

Centering is always desired but has not been easy.

A popular CSS pattern is using `position`, `top`, `left`, and `transform`/`translate`. For example,
```css
.centered {
  position: fixed; /* or absolute if needs to be bounded by the parent */
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
.h-centered {
  position: fixed; /* or absolute if needs to be bounded by the parent */
  left: 50%;
  transform: translateX(-50%);
}
.v-centered {
  position: fixed; /* or absolute if needs to be bounded by the parent */
  top: 50%;
  transform: translate(-50%);
}
```

These styles apply to a "child" element that has to be centered.

However, sometimes you just have to modify the parent element of the one that you want to center. In that case, use "flex" instead.
```css
.parent.centered {
  display: flex;
  flex-direction: row; /* default */
  justify-content: space-around; /* adds spaces to left and right to the child(ren) */
  align-items: center; /* vertically centers the child(ren) */
}
.parent.h-centered {
  display: flex;
  flex-direction: row; /* default */
  justify-content: space-around; /* adds spaces to left and right to the child(ren) */
}
.parent.v-centered {
  display: flex;
  flex-direction: row; /* default */
  align-items: center; /* vertically centers the child(ren) */
}
```

If `flex-direction` has to be `column`, then swap `align-items` and `justify-content`.

Note, `.parent.centered` only works nicely when there is a single child.

For more about CSS flex: [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

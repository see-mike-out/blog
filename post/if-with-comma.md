# `if` statements with a comma opeartor `(,,...)`.

Sometimes, you want to check the value of a variable (or a property of an object) before further operations on it. For example,

```js
if (object?.items[0]?.size > 3) { 
  object.items[0].sizePlusFour + object.items[0].size + 4; 
}
// the 'size' property of the first element ('0') 
// of the 'items' property of 'object' is greater 
// than 3, then assign "it" + 4 to the 'sizePlusFour' 
// property of the same parent.
```

(Note: `?` is [optional chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining).)

This can be better done by

```js
let size = object?.items[0]?.size
if (size > 3) {
  object.items[0].sizePlusFour + size + 4;
}
```

Another method is using a [comma operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator).

```js
let size;
if ((size = object?.items[0]?.size, size) > 3) {
  object.items[0].sizePlusFour + size + 4;
}
```

The comma operator does operation from left to right, and outputs the value computed in the last element.
That is, the code above first does the assignment and then returns the assigned variable `size`.
Below is also possible (but may not be on some compilers).

```js
if ((size = object?.items[0]?.size, size) > 3) {
  object.items[0].sizePlusFour + size + 4;
}
```

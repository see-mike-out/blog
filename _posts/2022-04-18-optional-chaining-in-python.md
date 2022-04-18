---
title: "Optional Chaining in Python"
date: 2022-04-18 13:20 +0900
categories: python
---

# Optional Chaining in Python

JavaScript's addition of the [optional chaining (`?.`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
has been useful since its beginning.
The optional chaining helps us to make our codes more concise by obviating the needs for membership checks.
For example,

```javascript
let phone = { "name": "iPhone 13 Pro", "dimension": 15, "screen_size": { "width": 390, "height": 450 } };
console.log(phone.name); // "iPhone 13 Pro"
console.log(phone.screen.width); // error! because you are getting the "width" of undefined
console.log(phone.screen?.width); // undefined
```

This further helps us when we need a default value for an undefined attribute. For instance,
```javascript
let phone_width = phone.screen?.width || 350;
console.log(phone_width); // 350
phone_width = phone.screen_size.width || 350;
console.log(phone_width); // 390
```

However, this is so difficult in Python! To do the same thing in Python, you should have multiple lines (particularly Python requires "indent" to be perefect)
```python
phone = { "name": "iPhone 13 Pro", "dimension": 15, "screen_size": { "width": 390, "height": 450 } }
if "screen_size" in phone:
    if "width" in phone["screen_size"]:
        phone_width = phone["screen_size"]["width"]
```

One possible workaround is using a recursive function.
```python
phone_width = getDictValue(phone, "screen_size.width", 350)
```
where the first argument is the `dict` variable, the second is a key chain, 
and the third is an alternative value to provide in case the intended value is unavailable.

To implement, we could do the following:

```python
def getDictValue(d, k, alt=None):
    # @d: a dictionary
    # @k: a key chain (str ("a.b.c") or list ["a", "b", "c"])
    # @alt: an alternative value to return when the key is not in the dict
    #      (default: None)

    # if the key chain @k is str, convert to a list
    if type(k) == str:
        k = k.split(".")
    # if the first key in @k is not in the dict @d, return @alt
    if d.get(k[0]) is None:
        return alt
    # else if the key chain @k's length is 1, return its value
    # implicitly, it assumes that the value is found
    elif len(k) == 1:
        return d[k[0]]
    # else (recursion)
    else:
        return getDictValue(d[k[0]], k[1:], alt)
```

The first `if` statement is to normalize the key chain to a list.
The first two conditions in the other `if-else` statement are the stop conditions for the recursion,
and the rest does the recursion.

---
title: "Heuristic-based Generation of a Linear Color Gradient"
date: 2022-11-18 15:30 -0600
categories: js
---

# Heuristic-based Generation of a Linear Color Gradient

Given a color value, finding a good color gradient scale is not easy. 
Simply increasing lightness in HSL value may cause the resulting color to be too bright or in a bad tone.
In this post, I introduce how to generate a linear color grident given a key color value.

For each rgb value, `v`, of your key color that ranges from 0 to 255, do the below linear transformation:
`v = v + (1 - v / 255) / a`,
where `a` is some value that you want to maximize (range between 0 to 255).
I suggest 225 for `a`. 
If you just multiply some rate, than 0 rgb value always remain 0, which causes it to be in a weird tone.
In a rgb color space, when you make a color lighter, the three rgb value should move together.
This helps that way.

For the entire rgb value, you can do something like below:

```js
function makeGradient(key_color) {
  let light_color = {};
  ['r', 'g', 'b'].forEach((c) => {
    light_color[c] = Math.round(key_color[c] + (1 - key_color[c] / 255) * 225)
    // 225 is a.
  });
  return [light_color, key_color];
}

let my_key_color = { r: 90, b: 0, g: 200 }; // this is the input shape. == #5a00c8
makeGradient(my_key_color); // [{r: 236, g: 249, b: 225}, { r: 90, b: 0, g: 200 }] == ['#ecf9ff', '#5a00c8']
```

This is something like below.

<div style="width: 200px; height: 30px; background: linear-gradient(0.75turn, #ecf9ff, #5a00c8); color: white; text-align: right;">
rgb(90, 0, 200) → rgb(236, 249, 225)
</div>

More examples: 

<div style="width: 300px; height: 30px; background: linear-gradient(0.75turn, #e1f6f4, #009eb3); color: white; text-align: right;">
rgb(0, 158, 179) → rgb(225, 246, 244)
</div>

<div style="width: 300px; height: 30px; background: linear-gradient(0.75turn, #f6e1eb, #b35900); color: white; text-align: right;">
rgb(179, 89, 0) → rgb(246, 225, 235)
</div>

<div style="width: 300px; height: 30px; background: linear-gradient(0.75turn, #e7f1e5, #308727); color: white; text-align: right;">
rgb(48, 135, 39) → rgb(231, 241, 229)
</div>


The end!

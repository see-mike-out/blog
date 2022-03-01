---
title: "How to keep an widget that interacts with a form open while working on the form"
date: 2022-03-02 04:56 +0900
categories: js html
---

# How to keep an widget that interacts with a form open while working on the form

Sometimes, your `<input>` element needs an widget along itself for a richer user interaction.
Frustration occurs when sometimes the widget should be closed when the interaction is done.
When you try to use the `blur` event, then clicking the widget fires the `blur` event of the `<input>` element.
To fix, use `setTimeout` and `DOM.hasFocus()`.

In pure HTML and vanilla JS,

```html
<script>
function showWidget() {
  // shows the widget
  document.getElementById("widget").classList.remove("hidden");
}
function hideWidget() {
  setTimeout(() => {
    if (!document.getElementById("form").hasFocus()) {
      // if the input element is no longer the active element (lost focus)
      document.getElementById("widget").classList.add("hidden");
    }
  }, 1500);
}
function focusBack() {
  // interaction gives the focus back to the input element
  document.getElementById("form").focus();
}
</script>
<style>
.hidden { display: none; }
</style>
...
<input id="form" type="..." onfocus="() => { showWidget() }" />

<!-- default: hidden -->
<div id="widget" class="hidden">
  ...
  <button type="button" onclick="() => { focusBack(); ... }">Button</button>
</div>
```

More simply, using Svelte

```svelte
<script>
  let showWidget = false;
  let formDOM;
</script>

...
<input type="..." bind:this={formDOM} on:focus={() => { showWidget = true; }} on:blur={() => {
  setTimeout(() => {
      if (!formDOM.hasFocus()) {
        // if the input element is no longer the active element (lost focus)
        showWidget = false;
      }
    }, 1500);
  }}
/>

{#if showWidget}
  <!-- default: hidden -->
  <div>
    ...
    <button type="button" on:click={() => { formDOM.focus(); }}>Button</button>
  </div>
{/if}
```

## Readings:
- [DOM.hasFocus()](https://developer.mozilla.org/en-US/docs/Web/API/Document/hasFocus)
- [document.activeElement](https://developer.mozilla.org/en-US/docs/Web/API/Document/activeElement) (to compare)
- [DOM.focus()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus)
- [setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)

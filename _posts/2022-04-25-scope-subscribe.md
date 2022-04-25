---
title: "Scoping store subscription in a Svelte component"
date: 2022-04-25 15:00 +0900
categories: js svelte
---

# Scoping store subscription in a Svelte component

Hello!

You often fail to scope Svelte store's subscription, which may cause further problem when the component is destroyed.
Here's a simple tactic: [REPL](https://svelte.dev/repl/72025c9b18bd420ea51e6582ea09acfc?version=3.47.0)
This will give you an error unless you uncomment the commented lines

## Explained

### `App.svelte`
```html
<script>
	import Child from "./Child.svelte";
	import { keep } from './storage.js';
	function giveValue(value) {
		keep.set(value);
	}
	function unsetValue() {
		keep.set(undefined);
	} 
	let showChild = false;
</script>

<div>
  <!--  Toggle the Child component  -->
	<button on:click={() => {showChild = !showChild }}>
		Show
	</button>
  <!--  Set the values for the store  -->
	<button on:click={() => giveValue([1,2,3]) }>
		Give values
	</button>
  <!--  Unset the values for the store  -->
	<button on:click={() => unsetValue() }>
		Unset values
	</button>
</div>
{#if showChild}
  <Child />
{/if}
```

### `Child.svelte`
```html
<script>
	import { onMount, onDestroy } from 'svelte';
	import { keep } from './storage.js';
  let on = false; // scoping variable
	let values = [];
  // when this compenent is mounted, then the scoping variable is set `true`.
 	onMount(() => { on = true; })
 	onDestroy(() => { on = false;	})
	keep.subscribe((v) => {
    // the store only updates the values when the scoping varible is `true`.
 		if (on) 
			values = v;
	})
</script>
<ul>
	{#each values as k}
		<li>{k}</li>
	{/each}
</ul>
```

### storage.js
```js
import { writable } from 'svelte/store';

export const keep = writable(undefined);
```

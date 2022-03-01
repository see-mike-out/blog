---
title: "Implementing a simple search form using Svelte"
date: 2022-03-22 04:05 +0900
categories: js svelte
---

# Implementing a simple search form using Svelte

Hi!

Sometimes you just need to implement a simple search form using which people can search from a given list.

Here's code:
[REPL](https://svelte.dev/repl/9af1ab3784b6486b8d3b71fffd007070?version=3.46.4)

## Search alg (commented)
The search algorithm is not really good. If you can find a better one, just replace!

```js
function searchMatch(ref, key, rank) {
  // @ref: reference item (from a search candidate list)
  // @key: a key to search
  // @rank: if true, it gives a ranking score; if false it gives true/false.
  // This function matches @ref and @key and gives a match score
  // or true/false results. The lower the score, the higher the priority.
  // There are four cases: (1) @ref starts with @key; (2) @ref inclues @key;
  // (3) @ref contains letters from @key (actually does in a reversed way 
  // to utilize regex); (4) not above.
  // While case (3) includes both (1) and (2), separating this could make 
  // the search a bit faster (when there are too many to search through).

  // case (1): starts with
  if (ref.startsWith(key)) return (rank ? 1 : true); 
  // case (2): includes
  else if (ref.includes(key)) return (rank ? 2 : true);
  // case (3): contains letters
  else if (key.match(new RegExp('^['+ref+']+$'))?.length > 0) {
    // how many letters are matched?
    if (rank) return 2 + 1 / (ref.match(new RegExp('['+key+']+', 'gi')?.reduce((c,a) => a + c.length, 0)) || 1);
    else return true;
  }
  // case (4): no match
  else return (rank ? Infinity : false);
}
function search (value, cands) {
  // @value: from event.target.value (String! may be an intermediate value)
  // This considers multi-outcome search.
  // @cands: where to search from (an Array of Strings)
  // This function runs the searchMatch function after simple diagnosis
  let sp = ("" + value).split(","); // to avoid intermediate value error
  let searchKeyword = sp[sp.length-1]; // pick the last keyword.
  
  // if value is not specified no results
  if (!value || value?.length < 0) return false; 
  // otherwise
  return cands.filter((v) => {
    // if already included in the selected values
    if (searchSelect.includes(v)) return false;
    // otherwise
    return searchMatch(v, searchKeyword, false);
  }).sort((a,b) => {
    // get the ranking
    return searchMatch(a, searchKeyword, true) - searchMatch(b, searchKeyword, true);
  });
}
```


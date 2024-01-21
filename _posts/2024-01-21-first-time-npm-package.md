---
title: "First time publishing an NPM package?"
date: 2024-01-21 11:55 -0600
categories: js npm
---

# Making a project 

## Case 1: You are literally about to start making an npm package

1. Initiate your project using `esm`, a "simple, babel-less, bundle-less ECMAScript module loader" (more [here](https://www.npmjs.com/package/esm)).

```shell
npm init esm
```

| Note. the `esm` pacakge hasn't been updated since 5 years ago. So some modern ECMA updates are not reflected (e.g., optional chaining). In that case use `esm-wallaby` instead (it is MIT license afaik).

2. Write your code. The importing statement is `import ... from ...`.

3. By the step 1, you'll get top-level `index.js` and `main.js`. Don't touch `index.js` unless instructed otherwise. Import your stuff in `main.js`.

4. Once you are done, run `node -r esm main.js` on the terminal to enable local runs. If you are using `esm-wallaby` then use `node -r esm-wallaby main.js`. 

## Case 2: You already written a project

1. Create a new project under the same name, following the above instructions.

2. Copy your codes and paste them to the `src` directory in the new project. 

3. Install dependencies. Copy the `dependencies` and `devDependencies` from the old `package.json`, paste those in the new `package.json`, and then run `npm i`),

4. Rest is same.

# Testing your pakcakge

I suggest separate your package code and some other testing project code separately. So that you can actually import the package and see how they work.
To do so, in your "testing project," install the package via the absolute path.

```shell
npm install [path]
```

If you are on the dev mode of svelte, react, or nodemon or whatever that reruns the server with local updates, 
your updates to the package is also reflected real time except a new dependency addition/removal. 
If you have new dependencies or removal of some, you should kill and run again your testing project. 

# Build

1. Install rollup. 

```shell
npm install --global rollup
```

2. Create a `build` directory at the top-level (i.e., `your-package/build`).

3. Add this statement under the `scripts` property of your package's `package.json`

- IIFE (supporting web browser) with raw code
`rollup main.js --file build/[name].js --format iife`

- IIFE (supporting web browser) with minimization
`rollup main.js --file build/[name].min.js --format iife --compact`

- CommonJS (supporting Node.js) with raw code
`rollup main.js --file build/[name]-node.js --format cjs`

- CommonJS (supporting web browser) with minimization
`rollup main.js --file build/[name]-node.min.js --format cjs --compact`

- UMD (supporting Node.js and web browser) with raw code
`rollup main.js --file build/[name]-umd.js --format umd --name \"[package-name]\"`

- UMD (supporting Node.js and web browser) with minimization
`rollup main.js --file build/[name]-umd.min.js --format cjs  --name \"[package-name]\" --compact`

| Note: For UMD, `[package-name]` is some object name that people can call on a browser.

Those statements can be concatenated using `&&`

For example, if your project name is `Bread` and you want filename `bread.js` then, the full set is something like this.

```js
{ ...
  "scripts": {
    ...
    "build": "rollup main.js --file build/bread.js --format iife && rollup main.js --file build/bread.min.js --format iife --compact && rollup main.js --file build/bread-node.js --format cjs && rollup main.js --file build/bread-node.min.js --format cjs --compact && rollup main.js --file build/bread-umd.js --format umd --name \"Bread\" && rollup main.js --file build/bread-umd.min.js --format cjs  --name \"Bread\" --compact",
    ...
  }
  ...
}
```

4. Then, run the build script by `npm run build`.


# What is this?

This is a simple experiment to test NPM workspaces.

# How to use

1. Clone this repo
2. Run `npm install`
3. Running `node packages/b/index.js`
4. You will see the following output:
```
/Users/pangratios/Work/subzero10/npm-workspaces-demo/node_modules/vue-template-compiler/index.js:10
  throw new Error(
  ^

Error: 

Vue packages version mismatch:

- vue@3.3.4 (/Users/pangratios/Work/subzero10/npm-workspaces-demo/node_modules/vue/index.js)
- vue-template-compiler@2.7.14 (/Users/pangratios/Work/subzero10/npm-workspaces-demo/node_modules/vue-template-compiler/package.json)

This may cause things to work incorrectly. Make sure to use the same version for both.
If you are using vue-loader@>=10.0, simply update vue-template-compiler.
If you are using vue-loader@<10.0 or vueify, re-installing vue-loader/vueify should bump vue-template-compiler to the latest.

    at Object.<anonymous> (/Users/pangratios/Work/subzero10/npm-workspaces-demo/node_modules/vue-template-compiler/index.js:10:9)
    at Module._compile (node:internal/modules/cjs/loader:1159:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1213:10)
    at Module.load (node:internal/modules/cjs/loader:1037:32)
    at Module._load (node:internal/modules/cjs/loader:878:12)
    at Module.require (node:internal/modules/cjs/loader:1061:19)
    at require (node:internal/modules/cjs/helpers:103:18)
    at Object.<anonymous> (/Users/pangratios/Work/subzero10/npm-workspaces-demo/packages/b/index.js:1:18)
    at Module._compile (node:internal/modules/cjs/loader:1159:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1213:10)
```

# What does it mean?

The hoisting feature of NPM workspaces is not working as expected. 
The `vue-template-compiler` package is hoisted to the root `node_modules` folder, and there's a `vue` package also hoisted at the root, but it's not the right version.
This causes the error above.

**What's worse is that this error is semi-random**:
If you had installed vue@2 first, then vue@2 would end up in the root `node_modules` and vue@3 would be inside the `packages/a/node_modules` folder
and `vue-template-compiler` would end up with the right version of vue (since vue@2 would be closer to it).
If you want to try this yourself, just move the contents of package a to package b and vice versa, and you will see that the error will go away. 

# Tested with

- npm 8.19.2
- npm 8.19.4

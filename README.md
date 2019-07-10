<!-- markdownlint-disable first-line-h1 line-length -->

[![npm package version](https://badge.fury.io/js/%40asd14%2Fpluginus.svg)](https://badge.fury.io/js/%40asd14%2Fpluginus)
[![Coverage Status](https://coveralls.io/repos/github/asd14/pluginus/badge.svg)](https://coveralls.io/github/asd14/pluginus)

# pluginus

> Dependency injection with promise support - Things that get ran after other things.

<!-- vim-markdown-toc GFM -->

* [Install](#install)
* [Use](#use)
* [Develop](#develop)
* [Changelog](#changelog)
  * [0.7 - 10 July 2019](#07---10-july-2019)
    * [Change](#change)

<!-- vim-markdown-toc -->

## Install

```bash
npm i @asd14/pluginus
```

## Use

`plugins/thing.js`

```js
exports default {
  create: props => () =>
    new Promise(resolve => {
      setTimeout(() => {
        resolve({
          foo: props.foo,
        })
      }, 50)
    }),
}
```

`plugins/second-thing.js`

```js
module.exports = {
  // First "Thing" is resolved to { foo: "bar" }
  depend: ["Thing"],

  // After dependencies are resolved, the current constructor is called
  create: props => Thing => ({
    ThingContent: `ipsum ${Thing.foo}`,
    ...props,
  }),
}
```

`index.js`

```js
import glob from "glob"
import { pluginus } from "@asd14/pluginus"

pluginus({
  props: {
    foo: "bar",
  },
})(glob.sync("./plugins/*.js", { absolute: true })).then(
  ({ Thing, SecondThing }) => {
    // Thing
    // => { foo: "bar" }
  
    // SecondThing
    // => { ThingContent: "ipsum bar", foo: "bar" }
  }
)
```

## Develop

```bash
git clone git@github.com:asd14/pluginus.git && \
  cd pluginus && \
  npm run setup
```

Run all `*.test.js` in `src` folder

```bash
npm test
```

Watch `src` and `examples` folder for changes and re-run tests

```bash
npm run tdd
```

## Changelog

History of all changes in [CHANGELOG.md](/CHANGELOG.md)

### 0.7 - 10 July 2019

#### Change

* Update packages

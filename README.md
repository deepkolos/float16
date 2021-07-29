# <a href="https://git.io/float16" target="_blank">@petamoriken/float16</a>

half precision floating point for JavaScript  
see [ES Discuss Float16Array topic](https://esdiscuss.org/topic/float16array)

[![npm](https://img.shields.io/npm/v/@petamoriken/float16.svg?style=flat-square)](https://www.npmjs.com/package/@petamoriken/float16)
[![npm downloads](https://img.shields.io/npm/dt/@petamoriken/float16.svg?style=flat-square)](https://www.npmjs.com/package/@petamoriken/float16)
[![license](https://img.shields.io/npm/l/@petamoriken/float16.svg?style=flat-square)](https://github.com/petamoriken/float16/blob/master/LICENSE)

[![Sauce Labs browser matrix](https://saucelabs.com/browser-matrix/petamoriken.svg?)](https://saucelabs.com/u/petamoriken)

## Supports (at least)

This library's `Float16Array` uses `Proxy` object, so IE11 is never supported.

`lib/` and `browser/` directories in the npm package have JavaScript files already built, whose target are

* Firefox: last 2 versions and ESR
* Chrome: last 2 versions
* Edge: last 2 versions
* Safari: last 2 versions
* Node.js: latest version

When you build by yourself using webpack or rollup.js for old browsers support, please transpile JavaScript files in `src/` directory.

## Install

```console
yarn add @petamoriken/float16
```

```console
npm install @petamoriken/float16 --save
```

## Require

### Node.js or Bundler (webpack, rollup.js)

```js
// ES Modules
import { Float16Array, getFloat16, setFloat16, hfround } from "@petamoriken/float16";
```

```js
// CommonJS
const { Float16Array, getFloat16, setFloat16, hfround } = require("@petamoriken/float16");
```

### Browser

Copy `browser/float16.js` file to your project directory.

```html
<script src="DEST/TO/float16.js"></script>
<script>
    const { Float16Array, getFloat16, setFloat16, hfround } = float16;
</script>
```

### Deno (Skypack CDN)

```ts
import { Float16Array, getFloat16, setFloat16, hfround } from "https://cdn.skypack.dev/@petamoriken/float16?dts";
```

## API

* `Float16Array`

    This API is similar to `TypedArray` such as `Float32Array`.

    ```js
    const float16 = new Float16Array([1.0, 1.1, 1.2]);
    for(const val of float16) {
        console.log(val); // => 1, 1.099609375, 1.19921875
    }

    float16.reduce((prev, current) => prev + current); // 3.298828125
    ```

* `getFloat16(view: DataView, byteOffset: number, littleEndian?: boolean): number`
* `setFloat16(view: DataView, byteOffset: number, value: number, littleEndian?: boolean): void`

    These APIs are similar to `DataView` methods such as `DataView#getFloat32` and `DataView#setFloat32`.

    ```js
    const buffer = new ArrayBuffer(10);
    const view = new DataView(buffer);

    view.setUint16(0, 0x1234);
    getFloat16(view, 0); // 0.0007572174072265625

    // You can append methods to DataView instance
    view.getFloat16 = (...args) => getFloat16(view, ...args);
    view.setFloat16 = (...args) => setFloat16(view, ...args);

    view.getFloat16(0); // 0.0007572174072265625

    view.setFloat16(0, Math.PI, true);
    view.getFloat16(0, true); // 3.140625
    ```

* `hfround(x: number): number`

    This API is similar to `Math.fround` ([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/fround)).
    This function returns nearest half precision float representation of a number.

    ```js
    Math.fround(1.337); // 1.3370000123977661
    hfround(1.337); // 1.3369140625
    ```

## Build

First, download devDependencies.

```console
yarn
```

Build `lib/`, `browser/` files.

```console
yarn run build
```

Build `docs/` files (for browser test).

```console
yarn run docs
```

## Test

First, download devDependencies.

```console
yarn
```

### Node.js Test

```console
yarn build:lib
yarn test
```

### Browser Test

```console
yarn build:browser
yarn docs
```

Access `docs/test/index.html` with browsers.

You can access current [test page](https://petamoriken.github.io/float16/test) ([power-assert version](https://petamoriken.github.io/float16/test/power)) in `master` branch.

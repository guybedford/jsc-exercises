# Transpile Exercises

## Exercise 1

> Intro to jco transpile

Use `jco transpile` to convert a component to JavaScript.

- Feel free to try any of the reactors. Suggestion: `jco transpile reactors/hello.wasm -o hello`.
- Create a `test.js` file to import the `hello` component:

```
import * as m from './hello/hello.js';
console.log(m.hello());
```

- Run it with Node.js via `node test.js`

## Exercise 2

> Running component modules in the browser

This can be achieved with a build tool, or an import map, to map the preview2 implementation in the browser.

Run one of the reactors in a web browser, with the following HTML page and a local server (eg `npx http-server -c-1`):

```html
<!DOCTYPE html>
<script type="importmap">
  {
    "imports": {
      "@bytecodealliance/preview2-shim/cli": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/cli.js",
      "@bytecodealliance/preview2-shim/clocks": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/clocks.js",
      "@bytecodealliance/preview2-shim/filesystem": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/filesystem.js",
      "@bytecodealliance/preview2-shim/http": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/http.js",
      "@bytecodealliance/preview2-shim/io": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/io.js",
      "@bytecodealliance/preview2-shim/logging": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/logging.js",
      "@bytecodealliance/preview2-shim/poll": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/poll.js",
      "@bytecodealliance/preview2-shim/random": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/random.js",
      "@bytecodealliance/preview2-shim/sockets": "./node_modules/@bytecodealliance/preview2-shim/lib/browser/sockets.js"
    }
  }
</script>
<script type="module">
  import * as component from "./hello/hello.js";

  console.log(component.hello());
</script>
```

## Exercise 3

> Imports & map configuration

Transpile `reactors/say-name.wasm`, which has an import to `get-name`.

Use `jco transpile reactors/say-name.wasm --map get-name=../get-name.js -o say-name` to map the get name dependency to a local implementation.

Implement `get-name.js`:

get-name.js

```
export function getName () {
  return 'your name';
}
```

Update `test.js` to import `hello` from `say-name/say-name.js`.

Run `node test.js`.

If feeling adventurous, try using `--map get-name=../get-name.js#get-name` to see how nested interface mappings can be supported.

## Exercise 4

> Instantiation output

Instead of using map configuration, use direct instantiation output for jco:

```
jco transpile reactors/say-name.wasm --instantiation -o say-name
```

Using the custom instantiation code:

```
import { readFile } from 'node:fs/promises';
import { instantiate } from './say-name/say-name.js';

const instance = await instantiate(async coreName => {
  const wasmModule = await readFile(new URL(`./say-name/${coreName}`, import.meta.url));
  return WebAssembly.compile(wasmModule);
}, {
  'get-name': {
    getName () {
      return 'myname'
    }
  }
});

console.log(instance.hello());
```

This output mode is useful for custom integration instantiation workflows.

> If completed, try variations of the above workflows against other components in the reactors or commands folders.

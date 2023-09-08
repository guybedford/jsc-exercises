# ComponentizeJS Exercises

## Exercise 1

> Create a WASI command from JavaScript

The command world can be found in `wit/deps/cli/command.wit`.

It exports a `run` interface.

Write the JavaScript file `command.js`:

command.js
```
export const run = {
  run () {
    console.log('hello world');
  }
};
```

To create the component, use `jco componentize`:

```
jco componentize command.js -o command.wasm --wit wit --world-name wasi:cli/command --enable-stdout
```

> We have to enable stdout explicitly when using `console.log`, otherwise we would need to import the raw WASI interfaces to write to the console.

To test the command we can use `jco run`:

```
jco run command.wasm
```

## Exercise 2

> Log an environment variable from a WASI command

Following on from example 1, try adding a WASI import to this component, to read the environment variables:

command.js
```
import { getArguments, getEnvironment } from 'wasi:cli/environment';

export const run = {
  run () {
    const env = Object.fromEntries(getEnvironment());
    console.log(`HOME is ${env.HOME}`);
  }
};
```

Building with the same script:

```
jco componentize command.js -o command.wasm --wit wit --world-name wasi:cli/command --enable-stdout
```

And the same `jco run command.wasm` to test.

Try passing arguments from the `getArguments()` call.

## Exercise 3

> Create a reactor

Finally, we can create a reactor component from JS.

A reactor world is already defined in `wit/various.wit`. Try and match the expected component interface in a `reactor.js` implementation.

```
jco componentize reactor.js --wit wit -o reactor.wasm
```

After creating the component, try transpiling it back into JS and running JS on JS to test:

```
jco transpile reactor.wasm -o reactor
```

test.js
```
import * as reactor from './reactor/reactor.js';

console.log(reactor.variousArgs({
  num: 25,
  str: 'test',
  opt: null,
  buffer: new Uint8Array([1,2,3])
}));
```

Reactors can also use WASI imports, add `--enable-stdout` and use console.logging if necessary to debug.

Try also throwing an error inside of the `various` function.

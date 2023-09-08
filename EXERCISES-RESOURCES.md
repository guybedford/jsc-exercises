# Resources Exercises

## Exercise 1

> Exported resources

Transpile the `reactors-resources/point.wasm` binary.

See the definition in [reactors-resources/point.wit](reactors-resources/point.wit).

Create a `test.js` importing this point component, importing the point class via:

```
import { objects } from './point/point.js';

const { Point } = objects;
```

Try creating some point instances and using them as classes:

* Create a point class instance via `var a = new Point(x, y)`
* Verify the `a.getX()`, `a.getY()` methods on the point
* Create two points and check if they are equal via `point.eq(otherPoint)`
* Apply the dot product between two points via `point.dotProduct(other)`
* Create an origin point using the static class method `Point.origin()`

> Exported resource classes integrate with the JS GC finalization registry, with destructors handled automatically through the combination of the JS GC and ownership model.

## Exercise 2

> Imported resources

This exercise uses the same resource class, but this time as an imported resource, which means you have to implement the point class and verify the implementation.

Transpile the `reactors-resources/point-import.wasm` binary, with a map configuration to supported the imported resource file:

```
jco transpile reactors-resources/point-import.wasm --map local:point-import/objects=../local-point-object.js
```

Look at the `reactors-resources/point-import.wit` file to see the shape of the resource that needs to be implemented.

You can implement it in the following JS class in `local-point-object.js`:

local-point-object.js
```js
export class Point {
  constructor (x, y) {
    // ...
  }
  getX () {
    // ...
  }
  getY () {
    // ...
  }
  eq (other) {
    // ...
  }
  dotProduct (other) {
    // ...
  }
  static origin () {
    // ...
  }
}
```

The rest of the implementation is up to you to fill in.

To verify that you have implemented the point resource class correctly, you can run the implementation verification test:

test.js
```js
import { checkImplementation } from './point-import/point-import.js';

// try check the implementation!
console.log(checkImplementation());
```

Finally, try creating two local points and passing them into the `translate` method to get the point `(5, 5)`:

test.js
```
import { translate } from './point-import/point-import.js';
import { Point } from './local-point-object.js';

// ...

translate(a, b).eq(new Point(5, 5));
```


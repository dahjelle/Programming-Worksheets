# Equality of Objects and Arrays in JavaScript

Related to the question of [reference and value in calling functions](./PassByReferenceAndValue.markdown) is the question of equality: when are two values `===` to each other? (I'm restricting myself to `===` in JavaScript to simplify things a bit; [you can read up the rules if you want](https://dorey.github.io/JavaScript-Equality-Table/).)

```javascript
1 === 1; // true
[1] === [1]; // false
{a: 1} === {a: 1}; // false
```

Why is this?

In JavaScript, "primitive" values are compared "by value". That means that strings, numbers, and booleans are all compared by the value that they contain, as in line 1 above.

## `===` on Objects and Arrays

On the other hand, objects and arrays (basically everything else in JavaScript) are compared "by reference." That means that the computer doesn't look at all in the values *contained* in the array, but instead simply looks at the memory location that the data is stored in. If the memory location is the same, they are equal, but if the memory location is different, they are *not* equal. While you know that two objects must be equal if they point to the same memory location, the inverse isn't true at all.

In other words, since we created **two** separate arrays in line 2 above, they point to two separate memory locations, and so are not equal.

## Multiple Names for the Same Data

This is also confusing in other ways. For instance:

```javascript
const a = [1];
const b = a;
a.push(2);
a === b; // true
console.log(a, b); // [1, 2], [1, 2]
```

Even though it looks like we only `push`ed on to `a`, both `a` and `b` point to the same memory location and are two names for the same thing. So they are `===` equal, and will always contain the same information.

## How do I copy an object or array?

It depends. A couple ideas come to mind:

- `_.clone` will create a shallow clone.
- `_.cloneDeep` will create a deep clone.
- `for` loops can work (shallow).
- `.map` and friends (on arrays) always returns a new array (shallow).
- `JSON.parse(JSON.stringify(x))` can work in a pinch. A deep clone, but is limited to JSON data structures, and I've no idea how the performance compares. But it is easy.

### What's with "deep" and "shallow" clones?

A "deep" clone is what you'd expect: a complete copy without anything shared with the original data structure. However, this can be slower than a shallow clone for nested data structures.

A "shallow" clone only clones the first level deep of the data structure. This means that some of the values in the object (or array) might still be shared. A simple example:

```javascript
const a = [[1]];
const b = _.clone(a);
a[0].push(2);
console.log(a, b); // [[1, 2]], [[1, 2]]
```

The first element of `a`, which is *also* an array, is shared, so pushing on to it also affects `b`. A deep clone would keep them entirely separate.

```javascript
const a = [[1]];
const b = _.cloneDeep(a);
a[0].push(2);
console.log(a, b); // [[1, 2]], [[1]]
```

## How do I see if arrays or objects are equal?

Just as with the clones, there are "deep" and "shallow" variations. Do you care only about the top level? Do you care about every level deep? For that matter, do you care if the elements in an array are in the same order? Do you care about sets or other JavaScript data structures?

All that said, a few places to get started:

- `_.isEqual` will do a deep comparison of a lot of JavaScript data structures.
- A `for…in` or `for…of` loop can do a quick shallow comparison.
- `JSON.stringify(x) === JSON.stringify(y)` can be handy if you don't care about the performance and need a quick test while debugging.
- `Immutable.is` works well for Immutable.js data structures (deep, but I'm not sure of the performance characteristics).

As with the clone, performance depends on how much data you are comparing.

Here's an example of a loop to compare two objects shallowly (roughly from [Facebook's implementation](https://github.com/facebook/fbjs/blob/4369c7dfeb1ab0de27feb3e9245f599c8edd6a5c/packages/fbjs/src/core/shallowEqual.js#L34-L67):

```javascript
function shallowEqual(objA, objB) {
  if (is(objA, objB)) {
    return true;
  }

  if (typeof objA !== 'object' || objA === null ||
      typeof objB !== 'object' || objB === null) {
    return false;
  }

  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  if (keysA.length !== keysB.length) {
    return false;
  }

  // Test for A's keys different from B.
  for (let i = 0; i < keysA.length; i++) {
    if (
      !hasOwnProperty.call(objB, keysA[i]) ||
      !(objA[keysA[i]] === objB[keysA[i]])
    ) {
      return false;
    }
  }

  return true;
}
```


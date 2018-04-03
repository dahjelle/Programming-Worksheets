## Pass by Reference and Value

> **Note**: The examples here are in JavaScript, but can affect many other languages such as PHP.

It can be really easy to get confusing results when your functions change ("mutate") the objects that you pass in to them.

For instance, let's say that you want to write a function that takes in some data of the following form:

```javascript
const data = [{ a: 1, b: 2 }, { a: 3, b: 4 }];
```

It will take each object in the array, calculate the total of its values, and add that total onto the object. In other words, it should return an array like:

```javascript
const desired_result = [{ a: 1, b: 2, total: 3 }, { a: 3, b: 4, total: 7 }];
```

Seems straightforward.

```javascript
function totalValues(arr) {
  let new_array = [];
  for (const obj of arr) {
    const values = Object.values(obj); // [1,2] or [3,4]
    let total = 0;
    for (const el of values) {
      total += el;
    }
    obj.total = total;
    new_array.push(obj);
  }
  return new_array;
}
```

Let's see if it works!

```javascript
const totals = totalValues(data);
console.log(JSON.stringify(totals) === JSON.stringify(desired_result)); // true
console.log(totals); // "[{a:1, b:2, total:3}, {a:3, b:4, total:7}]"
```

Looks like it worked perfectly! Hooray!

But there **is** a lurking problem. What happens if we run the same thing again?

```javascript
const totals2 = totalValues(data);
JSON.stringify(totals2) === JSON.stringify(desired_result); // false
```

False?!?! Why did it fail? Let's look more closely…

```javascript
console.log(totals2); // "[{a:1, b:2, total:6}, {a:3, b:4, total:14}]"
```

Where are those totals coming from?

Aaargh!!!!

What's my data again?

```javascript
console.log(data); // "[{a:1, b:2, total:6},{a:3, b:4, total:14}]"
```

Hey! Who put `totals` in my data?

And therein lies the problem: since `totalValues` **changes** the original objects in `data`, every time we run total, it uses the **most recent** version of `data`, rather than the one originally specified. **Even though we are carefully creating a new array to return!**

This is where immutable data structures really shine—this problem would not happen. But how do you fix this in plain ol' JavaScript?

Probably something like:

```javascript
new_array.push({
  ...obj,
  total
});
```


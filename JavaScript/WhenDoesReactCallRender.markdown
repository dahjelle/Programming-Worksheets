# When Does React Call Render?

## Summary of guidelines

1. Keep your `render` function as fast as possible. You can do lots of computation here, but it isn't the place for computationally-heavy processes like indexing of something.
2. Extend `PureComponent` instead of `Component` if you can. This will do a [shallow comparison]() of props and state, and **only** call `render` if they are different. This may require you to design `props` or `state` in a way that complies.
3. Use `shouldComponentUpdate(nextProps, nextState)`  to do the comparison of `props` and `state` yourself (for instance, if they are more complicated than the shallow comparison of `PureComponent` would allow) to avoid re-rendering. Be careful, though, that your check doesn't take more time than just `render`ing would.

## When Does React Call Render?

In general, every time you call `setState` within a component (changing a component's state) or a parent component (changing a component's props). Of course, React may "batch" calls to `setState`, so that multiple `setState` calls could result in a single `render`, but `render` will get called regardless.

**Additionally**, a component's render will get called **if a parent's `render` method is called**. This can yield `render` getting called much more than you expect.

If you have a `PureComponent` or you've implemented `shouldComponentUpdate`, then the `setState`→`render` rules are more complicated.

## `shouldComponentUpdate(nextProps, nextState)`

`shouldComponentUpdate` is a lifecycle method that gets called immediately before `render`. If it returns `false`, React will avoid calling the `render` method and assume that the result of `render` is the same as it was last time `render` was called…no change.

This can be a way to get faster performance. If, for instance, you know your `render` function only depends on `this.props.x`, which is a string, you can implement something like:

```javascript
shouldComponentUpdate(nextProps, nextState) {
    return nextProps.x !== this.props.x;
}
```

to make sure we re-render **only** if the prop we care about has changed, and **not** if any other props or state have changed, and **not** just because the parent component's `render` method was called.

Be aware, of course, that if you implement this function incorrectly, you can easily **not** render when you needed to, and get a UI that doesn't respond properly.

## `PureComponent`

Probably the most common `shouldComponentUpdate` implementation does a shallow object comparison of props and state: if your state and props are simple and not complicated/nested data structures, this will work fine to prevent unnecessary re-renders. So, React provides an implementation of this out-of-the-box: extend `PureComponent` instead of `Component` and you'll be on your way!


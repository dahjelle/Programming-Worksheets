# Introductory React

This will provide a gradual implementation of the 'todo' application on [React's website](https://facebook.github.io/react/) (or a close approximation).

## Boilerplate

This assumes you have an `index.html` that includes both React and `index.js` (which we'll mention in a moment) and a `index.jsx` or similar that is getting compiled to the `index.js` file previously mentioned. We'll technically be using `browserify`, but `webpack` or similar should work fine.

We don't discuss the preparation of those files in this tutorial.

## Simplest React Application

To render HTML to the screen, use `React.render`:

```JavaScript
var React = require('react'); // we'll assume this line is present for all future code snippets

React.render(
  <p>Hello, world!</p>,
  document.body
);
```

`React.render` takes the "JSX" (an HTML-like language that lives in your JavaScript) that you pass it as the *first* parameter, and fills the HTML element you give it as the *second* parameter with the result. In this case, putting the `<p>` inside the `<body>` tag on your page.

JSX can also be assigned to variables.† In other words, we can refactor the above example to:

```JavaScript
var contents = <p>Hello, world!</p>;

React.render(
  contents,
  document.body
);
```

You can also nest JSX tags, just like in HTML.

```JavaScript
var contents = <p>Hello, <small>small</small> world!</p>;

React.render(
  contents,
  document.body
);
```

† In the background, each JSX tag actually corresponds to a function, but you mostly don't need to worry about that.

**Exercise 1**: Try make a few variations of HTML tags of your own display using JSX and React.

## Combining JSX and JavaScript

You can also grab values from JavaScript and place them in your HTML. This is done by surrounding the JavaScript expression in `{}`. For instance:

```JavaScript
var name = 'Samuel';
var contents = <p>Hello, <b>{name}</b>!</p>;

React.render(
  contents,
  document.body
);
```

The JavaScript values can also be JavaScript expressions or other React elements.

```JavaScript
var name = 'Samuel';
var bold_name = <b>{name + ' the person'}</b>;
var contents = <p>Hello, {bold_name}!</p>;

React.render(
  contents,
  document.body
);
```

You can also use JavaScript expressions as attribute values in the HTML tags.

```JavaScript
var name = 'Samuel';
var contents = <input value={name} />; // you won't be able to edit this field; we'll discuss that later

React.render(
  contents,
  document.body
);
```

**Exercise 2**: Render an unordered list `<ul>` of several items using JSX and JavaScript variables.

## Arrays in JSX

In the last exercise, you probably noticed it was a bit of a pain to create multiple list items. Wouldn't it be nice if JSX would accept arrays? It turns out, it will.

```JavaScript
var paragraphs = ['one', 'two', 'three'];
paragraphs = paragraphs.map(function(paragraph) {
  return <p>
    {paragraph}
  </p>;
});
var content = <div>
  {paragraphs}
</div>;

React.render(
  content,
  document.body
);
```

Why did we need the extra `<div>` tag to make `content`? Unfortunately, while JSX itself accepts arrays, `React.render` will only accept JSX components. This is usually not a problem in practice, but does require a container `<div>` in this instance.

Now, strictly speaking, even though the above code renders the paragraphs just fine, you might notice a warning about 'unique "key" prop' in your console. (Or you might not.) This means that React wants each React component inside an array like this to have a `key` property that is unique amongst all array elements. Let's fix it:

```JavaScript
var paragraphs = ['one', 'two', 'three'];
paragraphs = paragraphs.map(function(paragraph, index) {
  return <p key={index}>
    {paragraph}
  </p>;
});
var content = <div>
  {paragraphs}
</div>;

React.render(
  content,
  document.body
);
```

**Exercise 3**: Do exercie #2 again, except with arrays. Make sure you don't get React errors in your console!


# Answers

1. Up to you—if it displayed as you expected on the screen, you pass!
2. Something like:

```JavaScript
var item_one = <li>One</li>;
var item_two = <li>Two</li>;
var item_three = <li>Three</li>;
var list = <ul>
  {item_one}
  {item_two}
  {item_three}
</ul>;

React.render(
  list,
  document.body
);
```
3. Something like:

```JavaScript
var items = ['One', 'Two', 'Three'].map(function(item, key) {
  return <li key={key}>{item}</li>;
});
var list = <ul>
  {items}
</ul>;

React.render(
  list,
  document.body
);
```

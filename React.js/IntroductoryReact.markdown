# React Bit-by-Bit: An Introduction for Beginners

This will provide a gradual implementation of the 'todo' application on [React's website](https://facebook.github.io/react/) (or a close approximation).

## Boilerplate

If you are just starting, I'd suggest using [JSBin](http://jsbin.com/tawaqaviyo/1/edit?js,console,output) to play around; you can learn React without having to install anything locally.

If you want to develop locally, this assumes you have an `index.html` that includes both React and `index.js` (which we'll mention in a moment) and a `index.jsx` or similar that is getting compiled to the `index.js` file previously mentioned. We'll technically be using `browserify`, but `webpack` or similar should work fine.

We don't discuss the preparation of those files in this tutorial.

## Simplest React Application

To render HTML to the screen, use `React.render`:

```JavaScript
var React = require('react'); // we'll assume this line is present for all future code snippets; it is ONLY needed if you are using the browserify or webpack setup

React.render(
  <p>Hello, world!</p>,
  document.body
);
```

`React.render` takes the "JSX" (an HTML-like language that lives in your JavaScript) that you pass it as the *first* parameter, and fills the HTML element you give it as the *second* parameter with the result. In this case, putting the `<p>` inside the `<body>` tag on your page. (The differences between HTML and JSX are [documented on the React site](https://facebook.github.io/react/docs/jsx-gotchas.html).)

JSX can be assigned to variables.† In other words, we can refactor the above example to:

```JavaScript
var contents = <p>Hello, world!</p>;

React.render(
  contents,
  document.body
);
```

You can nest JSX tags, just like in HTML.

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

You can grab JavaScript values and place them in your JSX. This is done by surrounding the JavaScript expression in {}. For instance:

```JavaScript
var name = 'Samuel';  // Regular JavaScript variable
var contents = <p>Hello, <b>{name}</b>!</p>;  // drop the variable into your HTML using '{}'  

React.render(
  contents,
  document.body
);
```

The JavaScript values can be either regular JavaScript expressions or other React elements.

```JavaScript
var name = 'Bob';
var bold_name = <b>{name + ' - the man, the myth, the legend'}</b>;
var contents = <p>It's time to meet…{bold_name}.</p>;

React.render(
  contents,
  document.body
);
```

You can use JavaScript expressions as attribute values in the JSX tags.

```JavaScript
var name = 'Samuel';
var contents =
  <p>
    <span>Name Box: </span>
    <input value={name} />
  </p>; // You will not be able to edit the input field's contents in the browser when this renders. We will discuss that later.

React.render(
  contents,
  document.body
);
```

**Note**: JSX always requires a single parent element with all other elements wrapped inside of it. So, for the above example, it wouldn't work to add another `<p>...</p>` to the beginning; you would need to wrap the whole thing in `<div>...</div>`, `<span>...</span>`, etc.

```JavaScript
//WRONG
var name = 'Samuel';
var contents =
  <p>Put your name below</p>
  <p>
    <span>Name Box: </span>
    <input value={name} />
  </p>;

//RIGHT
var name = 'Samuel';
var contents =
  <div>
    <p>Put your name below.</p>
    <p>
      <span>Name Box: </span>
      <input value={name} />
    </p>
  </div>; //Notice the opening <div> at the beginning of the contents variable and the closing </div> at the end of it.
```

**Exercise 2**: Render an unordered list `<ul>` of several items using JavaScript variables and JSX.

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

**Exercise 3**: Do exercise #2 again, except with arrays. Make sure you don't get React errors in your console!

## Embrace Modularity: Basic React Components

Programming in a modular way—i.e. packaging bits of functionality into distinct components or modules—tends to make our code more eloquent, flexible and maintainable.

So far, we've seen how JSX can be combined with all the logic ability of JavaScript to do some interesting things. It'd be nice now if we could package up pieces of JSX functionality into unique components. React lets us do this as well—basically allowing us to add our own unique HTML tags to the browser!

For instance, let's package up the previous paragraphs into a React component.

```JavaScript
var Paragraphs = React.createClass({
  render: function() {
    var paragraphs = ['one', 'two', 'three'];
    paragraphs = paragraphs.map(function(paragraph, index) {
      return <p key={index}>
        {paragraph}
      </p>;
    });
    return <div>
      {paragraphs}
    </div>;
  }
});

React.render(
  <Paragraphs />,
  document.body
);
```

In this case, we've created a component called `Paragraphs` (traditionally starting with a capital letter) with the `React.createClass` function. This function accepts an object, with a single required key of `render`. The `render` key has to be a function that will `return` JSX.

Once we create a component, we can use it in JSX directly, as if it was an HTML tag.

So in the example above...
First, we create the `Paragraphs` component with the `React.createClass` function.
Next, in the `render` key of `createClass`, we use JavaScript and JSX to define our variables as we did previously.
We then `return` the output we want for our `Paragraphs` component.
Now we can call `<Paragraphs />` as a unique HTML tag whenever we need it.

We can call our new `Paragraphs` component directly in `React.render`, or we can use it inside other components.

```JavaScript
var Paragraphs = React.createClass({
  render: function() {
    var paragraphs = ['one', 'two', 'three'];
    paragraphs = paragraphs.map(function(paragraph, index) {
      return <p key={index}>
        {paragraph}
      </p>;
    });
    return <div>
      {paragraphs}
    </div>;
  }
});

var MoreOfThem = React.createClass({
  render: function() {
    return <div>
      <Paragraphs />
      <Paragraphs />
      <Paragraphs />
    </div>;
  }
})

React.render(
  <MoreOfThem />,
  document.body
);
```

Above, we're using our `Paragraphs` component inside of our new `MoreOfThem` component. In `MoreOfThem`, look at what's happening in the `render` key. We're returning the `Paragraphs` key three times, so when we call `MoreOfThem` in `React.render`, `Paragraphs` is repeated three times.

If you are using a build tool like `browserify`, you can also stick your components in other files.

```JavaScript
// paragraphs.jsx
var React = require('react');
var Paragraphs = React.createClass({
  render: function() {
    var paragraphs = ['one', 'two', 'three'];
    paragraphs = paragraphs.map(function(paragraph, index) {
      return <p key={index}>
        {paragraph}
      </p>;
    });
    return <div>
      {paragraphs}
    </div>;
  }
});
module.exports = Paragraphs;
```

The `module.exports` assignment makes your `Paragraphs` component available in other JSX files when you require your `paragraphs.jsx` file.

```JavaScript
// index.jsx
var React = require('react');
var Paragraphs = require('./paragraphs.jsx');
React.render(
  <Paragraphs />,
  document.body
);
```

**Exercise 4**: Render your unordered list via a component.


## Interacting with Your Components: State

This has been interesting so far, but there isn't yet much that we could do with React that wouldn't have been done in plain old HTML or in the various HTML templating languages. (And, to be fair, React is far from the only way of doing what we'll be doing here, either.) But we want to move beyond just rendering HTML tags and create a user interface that's *interactive*. Let's make a component with some interactivity!

First, we need to introduce the concept of 'state'. State is a value that can change over time, and is local to the component. For instance, imagine that we have a `<Clock/>` component that displays an hour hand and a minute hand, representing the current time. The position of those hands would likely be state: they change over time, but no other component using the `<Clock/>` is likely to need or want to know that position. State allows us to keep such values local to a component. Keeping changing states local - set apart for a certain component - will help us program in a modular way.

So how do we use state? See below and pay special attention to the `getInitialState` key.

```JavaScript
var Stateful = React.createClass({
  getInitialState: function() {
    return {
      backcolor: '#0000FF'
    };
  },
  render: function() {
    var divStyle = {
      backgroundColor: this.state.backcolor,
      color: '#FFFFFF'
    };
    return <div style={divStyle}>
      Hello, world!
    </div>;
  }
});

React.render(
  <Stateful />,
  document.body
);
```

First, notice that we added a second function to our React component as the value for `getInitialState`. If you provide this function (and you should if you are using state), you should return a single object. The keys correspond to the names of the state variables you'll use, and the values are their initial values (which can change later). In this case, we are setting the `backcolor` state variable to a CSS color of `#0000FF` (a bright blue). While we only have a single state variable in this example, you can have as many as you need.

Second—a brief digression. As mentioned earlier, JSX allows us to put JavaScript values inside our JSX tags. In the case of CSS styles, it does something additional: it will accept a JavaScript object that will get converted to the appropriate CSS string. There is [React documentation](https://facebook.github.io/react/tips/inline-styles.html) on this sort of thing, but accept it for now that `backgroundColor` affects the background color of an HTML element, and `color` affects the *text* color of an element when they are passed to the `style` attribute of an HTML tag.

Third, onward. So, in this case, we have an initial `state` with a `backcolor` of `#0000FF`. In the `render` function (or other functions we may create ourselves), we can access that `state` via `this.state.VARIABLE_NAME`—in this case, `this.state.backcolor`. We use `this.state.backcolor` to create a `divStyle` object, which we use to set the `style` of the div that we render.

**BUT!** We still haven't made anything interactive? How can we do that?

Before we change the state based on user interaction, let's explore how we can handle events initiated by the user. The basic mechanics are very similar to old-school HTML events: adding event handlers to our JSX tags. (And [React has documentation on the specifics of what events can be handled in JSX](https://facebook.github.io/react/docs/events.html).) So, for example, we can run a function when the mouse is clicked on our `<div/>`:

```JavaScript
var Events = React.createClass({
  clickHandler: function() {
    console.log('You got me!');
  },
  render: function() {
    return <div onClick={this.clickHandler}>
      Hello, world!
    </div>;
  }
});

React.render(
  <Events />,
  document.body
);
```

In this case, we print to the `console` whenever the `<div/>` is clicked on. We attach an `onClick` event handler to the `<div>` JSX element, and that event handler is `this.clickHandler`, the `clickHandler` function we defined in the object given to `React.createClass`. The name of the handler function can be anything (clickHandler, stateChanger, userClick ... whatever you want).

While the React documentation as a complete list of events that can be handled, consider for now `onClick`, `onMouseOver`, and `onMouseOut`.

Let's recap.
1. We can create an initial, changeable state for our component by assigning a function to `getInitialState` (like `backcolor: '#0000FF'`).
2. We can use that state in our `render` with `this.state.VARIABLE` (which is `this.state.backcolor` in our example).
3. We can record what the user is doing with event handlers - `onClick`, `onMouseOver`, `onMouseOut` and such - and respond by calling functions.

So how do we change our state based on user interaction? Glad you asked! Enter the puzzle piece that brings all this together... `this.setState()`.

```JavaScript
var Stateful = React.createClass({
  getInitialState: function() {
    return {
      backcolor: '#0000FF'
    };
  },
  clickHandler: function() {
    this.setState({  // Here's where the interactivity comes to life
      backcolor: '#DD3131'
    });
  },
  render: function() {
    var divStyle = {
      backgroundColor: this.state.backcolor,
      color: '#FFFFFF'
    };
    return <div style={divStyle} onClick={this.clickHandler}>
      Hello, world!
    </div>;
  }
});

React.render(
  <Stateful />,
  document.body
);
```

When `this.setState` is called by your `clickHandler` function, you pass an object whose keys are the state variables that you want to change, and the values are the corresponding new values you want to see in the browser. You can set as many or as few state variables as necessary for to make the interactive changes you want in your application. 

When you call `this.setState`, it tells React: 'Hey, I have some new state. Can you please re-render me?' And React will call your `render` function again, this time referencing the new state.

Note that you *cannot* call `this.setState` inside a `render` function! That would be putting the cart before the horse. It's `this.setState` that
1. assigns the new values to `state` and
2. reruns `render` to display the changes.

**Exercise 5**: Create a component whose background color changes when the mouse moves over it, and changes back when the mouse leaves.
**Extra Credit**: Create a component that experiences three different changes in appearance when you click on it.

## Form Fields and State: Controlled and Uncontrolled

Form fields (such as text input boxes) have an interesting property: if you set their `value` attribute inside a `render` function, that value **completely** controls the value of the text box—even when the user types inside of it! This is called a 'controlled' input element. This is actually very desirable in many applications—such as, for instance, only allowing certain characters to be typed in an input field. (If you don't directly set the `value` attribute, you get an 'uncontrolled' component, which is also useful.)

The end result is that you need to use `this.setState` to keep the value of the text box updated while the user types, like this:

```JavaScript
var InputBox = React.createClass({
  getInitialState: function() {
    return {
      text: 'Type in here!'
    };
  },
  updateText: function(evt) {
    this.setState({
      text: evt.currentTarget.value
    });
  },
  render: function() {
    return <input type='text' value={this.state.text} onChange={this.updateText} />;
  }
});

React.render(
  <InputBox />,
  document.body
);
```

**Exercise 6**: Create an uncontrolled input box.

**Exercise 7**: Modify the controlled input box example I gave you to (1) have no initial text and (2) prevent the user from typing the letter 'e'. (Hint: use the JavaScript `String.replace` function.)

**Exercise 8**: Create a React component that renders a text box and a button. (Use a controlled text box.) When you click the button, the the current contents of the text box should be logged to the console, and the text box should be cleared.

## Passing Values from One Component to Another: Props

Simple Example
```JavaScript
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello {this.props.name}</div>; //Using the props passed value ("John")
  }
});

React.render(
  <HelloMessage name="John" />,  //The passed value to the HelloMessage class. 
  document.getElementById('container')
);
```
More Complex Example.
Since the state of one component might end up being useful in another component, React also allows you to pass data from one component to a child component using `props`. Let's use the `<Paragraphs/>` from earlier as an example.

```JavaScript
var Paragraphs = React.createClass({
  render: function() {
    var paragraphs = this.props.text.map(function(paragraph, index) {
      return <p key={index}>
        {paragraph}
      </p>;
    });
    return <div>
      {paragraphs}
    </div>;
  }
});

var paragraphs = ['one', 'two', 'three'];
React.render(
  <Paragraphs text={paragraphs} />,
  document.body
);
```

Here, we added a attribute to the `<Paragraphs>` JSX tag to pass the paragraph data to the component. Within the `render` method, we can access the data as `this.props.text` (where the label in `this.props.LABEL` corresponds to the attribute name in the JSX tag). You can have as many `props` as needed in a component, and [you can validate that a component has the `props` you expect](https://facebook.github.io/react/docs/reusable-components.html).

Note what this allows you to do: you can now create components that consume state from other components. For instance, here's an example that will add a new paragraph on each click.

```JavaScript
var Paragraphs = React.createClass({
  render: function() {
    var paragraphs = this.props.text.map(function(paragraph, index) {
      return <p key={index}>
        {paragraph}
      </p>;
    });
    return <div>
      {paragraphs}
    </div>;
  }
});

var App = React.createClass({
  getInitialState: function() {
    return {
      paragraphs: ['one', 'two', 'three']
    };
  },
  clickHandler: function() {
    var paragraphs = this.state.paragraphs;
    console.log(paragraphs);
    paragraphs.push(paragraphs.length + 1); // changes the paragraphs array in-place
    this.setState({
      paragraphs: paragraphs
    });
  },
  render: function() {
    return <div onClick={this.clickHandler}>
      <Paragraphs text={this.state.paragraphs} />
    </div>;
  }
});

React.render(
  <App />,
  document.body
);
```

It is not uncommon to have a single top-level component contain all the state for an app, and pass pieces of it as necessary to child components that may have state of their own.

**Exercise 9**: Create a simple to-do list app with multiple components, state, and props. To show the to-do list, modify the component you created in exercise 4 to accept an array of items as `props`. Modify the component you created in Exercise 8 to 1) display the to-do list component from the previous sentence, 2) pass an array of to-dos to the to-do list component, and 3) update the array of to-dos after the user clicks add.



## Further Topics (not discussed here)

- How to update a parent component's state from a child component.
- [Component lifecycle](https://facebook.github.io/react/docs/component-specs.html).

----

# Answers

**Answer 1**. Up to you—if it displayed as you expected on the screen, you pass!

**Answer 2**. Something like:

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
**Answer 3**. Something like:

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

**Answer 4**. Something like:

```JavaScript
var List = React.createClass({
  render: function() {
    var items = ['One', 'Two', 'Three'].map(function(item, key) {
      return <li key={key}>{item}</li>;
    });
    return <ul>
      {items}
    </ul>;
  }
});

React.render(
  <List />,
  document.body
);
```

**Answer 5**. Something like:

```JavaScript
var blue = '#0000FF';
var red = '#FF0000';

var Stateful = React.createClass({
  getInitialState: function() {
    return {
      backcolor: blue
    };
  },
  inHandler: function() {
    this.setState({
      backcolor: red
    });
  },
  outHandler: function() {
     this.setState({
      backcolor: blue
    });
  },
  render: function() {
    var divStyle = {
      backgroundColor: this.state.backcolor,
      color: '#FFFFFF'
    };
    return <div style={divStyle} onMouseOver={this.inHandler} onMouseOut={this.outHandler}>
      Hello, world!
    </div>;
  }
});

React.render(
  <Stateful />,
  document.body
);
```

**Answer 6**: Something like:

```JavaScript
var InputBox = React.createClass({
  render: function() {
    return <input type='text' />;
  }
});

React.render(
  <InputBox />,
  document.body
);
```

**Answer 7**: Something like:

```JavaScript
var InputBox = React.createClass({
  getInitialState: function() {
    return {
      text: ''
    };
  },
  updateText: function(evt) {
    this.setState({
      text: evt.currentTarget.value.replace('e', '')
    });
  },
  render: function() {
    return <input type='text' value={this.state.text} onChange={this.updateText} />;
  }
});

React.render(
  <InputBox />,
  document.body
);
```

**Answer 8**: Something like:

```JavaScript
var AddToDo = React.createClass({
  getInitialState: function() {
    return {
      text: ''
    };
  },
  updateText: function(evt) {
    this.setState({
      text: evt.currentTarget.value
    });
  },
  clickHandler: function() {
    console.log(this.state.text);
    this.setState({
      text: ''
    });
  },
  render: function() {
    return <div>
      <input type='text' value={this.state.text} onChange={this.updateText} />
      <button onClick={this.clickHandler}>Add</button>
     </div>;
  }
});

React.render(
  <AddToDo />,
  document.body
);
```

**Answer 9**: Something like:

```JavaScript
var List = React.createClass({
  render: function() {
    var items = this.props.items.map(function(item, key) {
      return <li key={key}>{item}</li>;
    });
    return <ul>
      {items}
    </ul>;
  }
});

var AddToDo = React.createClass({
  getInitialState: function() {
    return {
      text: '',
      items: []
    };
  },
  updateText: function(evt) {
    this.setState({
      text: evt.currentTarget.value
    });
  },
  clickHandler: function() {
    var new_item = this.state.text;
    var items = this.state.items;
    items.push(new_item);
    this.setState({
      text: '',
      items: items
    });
  },
  render: function() {
    return <div>
      <List items={this.state.items} />
      <input type='text' value={this.state.text} onChange={this.updateText} />
      <button onClick={this.clickHandler}>Add</button>
     </div>;
  }
});

React.render(
  <AddToDo />,
  document.body
);
```

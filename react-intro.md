# New to React? You Need to Understand These Key Concepts Before Anything Else

That online tutorial you just finished didn't teach you the "big
picture" of React. Don't make the same mistakes I made...

## Getting the Big Picture

React stumped me at first.

Online tutorials and even the official documentation sometimes do a poor
job of explaining how React works and why it works that way. 

If you're learning React, you likely don't fully understand how 
everything fits together. Without that understanding, you're gonna hit
roadblocks and it's gonna take you longer than necessary to get up to
speed.

React is the most-popular frontend library in the world for a reason,
but you'll likely have a hard time at first learning to think in React's
terms. Especially if you've never worked with other fronted
libraries/frameworks.

When I started learning React, it was confusing. Components, props,
constructors, component state, and JSX all conspired to jumble my brain. 
It wasn't until I took a step back to look at the big picture and do some
research that I finally understood how everything fits together.

In this post, I'll save you that reading and research. I've put
together all my notes from learning how React works and distilled it
down to the barebones essentials to help you grok React.

React can be complex, and most tutorials make it too convoluted, too 
fast.  If you're learning React, however, you only need to know a few 
key concepts in order to unlock its power. I'll share those concepts 
here.

## React Isn't a Framework, so Don't Call It One

React often gets lumped in with Angular and called a "frontend
framework."

It may seem like a small distinction, but that term--"framework"--comes
with a lot of baggage. Frameworks imply an ecosystem of tools that rely
on one another. When you build an Angular app, you have to do it
Angular's way.

But React isn't like that.

It doesn't rely on a specific ecosystem to make it work. It's much
simpler. React is a "library," not a framework.

The distinction has profound implications for how you can use React. It
can work atop different kinds of backends, and you can deploy it
alongside other tools in the same app. 

If you already have an existing app, adding React to that app slowly
over time is totally possible. Just build it a component at a time!

### Ah-ha moment:

When you build something with React, you're not really building a "React 
application." 

Instead, you're just building a piece of your web page in React, and
React takes care of how that one piece of the page functions and
renders.

## Components are Cool!

Since React is a library, not a framework, we're building pieces of web
pages in React, not entire React applications.

Of course, if you want to combine multiple pieces into a complete
application, that's up to you. But you can also just place little bits
of React inside your normal HTML page if you want.

These "pieces of a web page" are React's "components."

When you use React, you're using JavaScript to create components that
you can place in your web page.

That relationship usually takes the form of creating a component:

```javascript
class Hello extends React.Component {
  render() {
    return <h1>Hello World</h1>;  
  }
}
```

And then  use that component almost as you would an html tag. We use
JavaScript to tell the browser where to place our new component.

```javascript
ReactDOM.render(<Hello />, document.getElementById('root'));
```

The code above tells React to place the new <Hello /> component into our
HTML file wherever there's an element with an id of 'root'.

(If you're new to React, chances are you're scratching your head about
mixing HTML tags directly into JavaScript. Not to worry, it's not really
HTML but a special markup for React called JSX. More on that in the
section "Your Problem with JSX is You Don't Understand JSX".)

### Ah-ha Moment

You could build a completely static site in React that's not interactive
at all. You'd just use JSX to create components and then place them in
your website. React doesn't have to be interactive, it just CAN be and
is good at it.

To be clear, for static sites, React and JSX aren't good choices as far
as templating syntaxes go. You'd probably wan't to pick something else
that's less complicated and has less overhead.

Still, at its most basic, React is a way to define a piece of a website
using JavaScript and then place that component into the website.

Components are reusable as well, so if we've defined <Hello /> we can
use it as many times as we want anywhere in our site. 

## The Deal with the DOMs

Seems cool, right? Templating and reusable components are far from
groundbreaking, but at a basic, intuitive level that's what React is
doing.

But how does React use JavaScript to place components into a web page?

When you use JavaScript to do something--anything--on a web page, the
JavaScript you've written needs a way to interact with the HTML and
other documents that make up your web page.

JavaScript does this through an API known as the Document Object Model
(DOM). If you've written JavaScript, chances are you've heard of the DOM
and interacted with it.

However, for complicated websites, managing and making changes to the
DOM can quickly become a challenge. When multiple scripts need access to
multiple DOM objects, maintaining a consistent state of the DOM requires
all kinds of manipulations and checks, and some things you might want to
do are downright impossible.

Managing the DOM is where React is a breakthrough technology.

Instead of trying to manage the DOM directly, React creates a new
version of it known as the React virtual DOM. When you write React code,
you're making changes to the virtual DOM, not the real DOM.

That's why, in the Hello example I gave, we rendered <Hello /> to the
ReactDOM, not to the actual DOM directly:

```javascript
ReactDOM.render(<Hello />, document.getElementById('root'));
```

When a user clicks a button or enters a value in a React component, 
React compares the state of the virtual DOM to the real DOM. If they're 
the same, React doesn't need to do anything. If they're different, React 
updates the real DOM to match the virtual one.

### Ah-ha Moment:

React "reacts" to changes in the virtual DOM and makes those changes in
the real DOM.

Mind blown???

In summary, React is a library that allows you to create reusable 
components. 

It's also a DOM manager that compares the virtual and real versions of 
your site's document object tree.

The implication of this combination is you can write complicated
interactions within and among components that render seamlessly without
having to reload the page.

In addition, you no longer have to worry about the DOM and making
changes to it. Instead, React takes care at compile time to warn you of
any issues.


## Please Pass the Props

Okay, so we've got the core concepts of React: creating components and
rendering them in a virtual DOM. Understanding those basic concepts is
key to getting why and how React works when things get more complicated.

In React, components can call and talk to each other. When they do, they
can pass information between themselves as properties.

```javascript
class Board extends React.Component {
  
  ...

  renderSquare(squareValue) {
    return (
      <Square 
        value={squareValue}
      />
    );
  }
  
  ...
}
```

I've left out much of the details of the Board component, above, for the
sake of highlighting how one component can call another.

In Board's `renderSquare()` method, it returns a `<Square />`. This is a
common pattern in React apps.

Likely, elsewhere in the Board method, the Board will call
`renderSquare()` multiple times, creating many different Square
components that are all children of the Board.

With each call of `renderSquare()` the variable `squareValue` can
change. So, each new Square might have a different value. 

These values that get passed from parent to child are what's known as
"props" in React (short for properties).

Within the declaration of the Square class, we can now access whatever
information the Board passes in as a prop.

```javascript
class Square extends React.Component {
  render () {
    return (
      <button>{props.value}</button>
    );
  }
}
```

Using `props.value` React knows to use the `value` that we passed in
when we created the Square.

(This example comes from the React official tutorial where you build a
Tic Tac Toe game. The Board component renders and manages 9 Squares as
child components. See this [working example on
Codepen](https://codepen.io/gaearon/pen/gWWZgR?editors=0010).

### Ah-ha Moment

React makes it really simple to pass information between components. But
one thing to realize early on is that props only work in one direction.

Parents can pass info to their children, but if a child needs to pass
info back to its parent, you'll need to define a function in your parent
that allows the child to move information back up the chain.

Passing props the wrong direction or trying to pass props up to a parent
is one of the big mistakes new learners of React make.

## Move Power Up the Chain

Let's imagine we want to do something when a button is clicked.

In vanilla JavaScript, we'd use an onClick event to update the DOM.

```html
<button onClick="myFunction()">Click me</button>
```

React also uses onClick, but now we can use it to call other methods
from our components.

```javascript
class Button extends React.Component {
  handleClick() {
    // do something
  }
  render() {
    return (
      <button onClick={() => this.handleClick()}>
        {buttonValue}
      </button>
    )
  }
}
```

If we want to pass information back to a parent component, we can define
the `handleClick()` method in the parent component and then pass that
method as a prop to the the child.

Now the Button component gets simplified to:

```javascript
class Button extends React.Component {
  render() {
    return (
      <button onClick={props.onClick}>
        {buttonValue}
      </button>
    );
  }
}
```

And the parent of Button now passes in the `onClick` method as a prop:

```javascript
class ButtonList extends React.Component {
  handleClick() {
    // do something
  }
  render() {
    return (
      <Button onClick={() => this.handleClick()} />
    );
  }
}
```

The astute reader will notice that we just took ~20 lines of React to do
something that could have been done in one or two lines of JavaScript if
we just managed the button directly.

And you'd be right! If all you need is a single function for a single
button in your application, don't use React!

The structure above becomes valuable only when we need to render
multiple Buttons, each with their own unique identifiers, but all of
whom need a shared `handleClick` method.

Imagine a `ButtonList` component that manages multiple buttons:

```javascript
class ButtonList extends React.Component {
  renderButton() {
    return (
      <Button onClick={() => this.handleClick()} />
    );
  }
  renderButtonList() {
    let buttonList = [];
    for (let i=0; i<10; i++) {
      buttonList.push(this.renderButton());
    }
    return buttonList;
  }
  handleClick() {
    // do something
  }
  render() {
    return <div>{this.renderButtonList()}</div>;
  }
}
```

By moving `handleClick` up a level to the parent component, we do at
least two things:

1. We don't repeat ourselves (DRY) when we create a new Button. The
   `handleClick` method is standardized across all buttons that are
   children of `ButtonList`.
2. We gain the ability to see a complete list of the current state of
   all `Buttons`, because they will pass back information every time they
   get clicked. Instead of needing to view the state of each Button
   independently, we can now track the state of all Buttons and even
   revert changes (see "const vs let, below" for more details).

Of course, readers experienced in writing React code will note that my
example above is not correct. We're still missing a *key* part of
managing multiple child components ;).

Experienced React users will also point out that I could seriously clean 
up both these code examples with shorter syntax. These examples don't 
NEED to be this long.

I'm keeping it verbose for educational purposes, but not to
worry! I will shorten some stuff! (See Function Components, below).

## Don't Forget the "Keys"

We've seen that React makes it easy to create multiple child components,
pass information to those children via props, and even track the state
of those children from the parent component.

Part of the challenge for React, though, is remembering which children
have changed. This is especially true if your application involves
manipulating the order of the child components.

For example, imagine if we allowed users to drag and reorder the
position of the ten Buttons in ButtonList. How would React be able to
keep the state of each button when the list indices are constantly
changing? This is especially dangerous if we give users the ability to
delete Button components they don't need. React might delete the wrong
information by mistake!

To fix this problem, React likes you to provide a unique identifier for
each child component you create. This unique id is called a `key` in
React.

Since ours is a simple listof Buttons, adding the keys is easy enough:

```javascript
class ButtonList extends React.Component {
  renderButton(i) {
    return (
      <Button 
        key={i} // The Button now has a key!
        onClick={(i) => this.handleClick(i)} 
      />
    );
  }
  renderButtonList() {
    let buttonList = [];
    for (let i=0; i<10; i++) {
      buttonList.push(this.renderButton(i));
    }
    return buttonList;
  }
  handleClick(i) {
    // do something to the button where key===i
  }
  render() {
    return <div>{this.renderButtonList()}</div>;
  }
}
```

### Ah-ha Moment

In the example above, you can see that we added a key to the calls to
`renderButton`. But we also added the key, `i`, to all the calls to
`onClick` and `handleClick`. That way, React knows exactly which
`Button` component to update when there's a click event.

Keys are crazy important, and they can get a lot more complicated, but
they're what allows React to keep track of the unique state of tons of
different components on a page.

## Constructors, super(), Local State: Open Pandora's Box

Speaking of keeping track of state, React allows you to store some local
information about the state of a component. This is useful if you want
to keep history or information about a Component's progress through
a workflow.

For instance, in our `ButtonList` example, if we wanted to keep a
register of how many times buttons have been clicked and in what order,
we'd need to keep some information on the state of the list inside the
`ButtonList` component.

Do do so, we use a constructor to initialize the component state:

```javascript
class ButtonList extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      clickCount: 0,
    };
  }

  ... // The rest of the component that we've already seen.

}
```

All React components have a state built in, so we need to call
super(props) to be able to access the parent class's (React.Component)
constructor to be able to update the state.

Now that we have built our constructor, we can modify and use the state:

```javascript
  ...

  handleClick(i) {
    const clickCount = this.state.clickCount;
    
    ... // Do something

    this.setState({
      clickCount: clickCount + 1,
    });
  }

  ...
```

We now have a counter that increases every time there's a click on a
`Button`.

This is a simple example to illustrate state. I don't recommend you use
it as a counter in a real application. Instead, you'd probably want to
create a `this.state.history` variable that stores exactly which button
was clicked at which point as an array of the current state at different
points in time.

Then, if you need a click count, you can access it with
`this.state.history.length`, and that click count will be correct even
if you undo past changes.

### Ah-ha Moment

State and history open up a whole new world of potential applications.
Now you can build some complex stuff with React, simply by storing state
information.

Want to add/remove items from a shopping cart? React state can handle
that.

Undoing moves in a browser Chess game? Component state has you covered.

Rendering components differently based on user toggles? State can help
you there as well.

Understanding and getting good at `this.state` (along with `this.props`)
is the key to getting good at React.

## Use Function Components for the Simple Stuff

Since we give all the power to the parent components in React, child
components can end up being pretty dumb.

Consider the final version of our basic `Button` component:


```javascript
class Button extends React.Component {
  render() {
    return (
      <button onClick={props.onClick}>
        {props.buttonValue}
      </button>
    );
  }
}
```

All that component does is render a button. Since it's so simple, we
don't actually have to create a whole class for it in React.

Instead, just make it a function:

```javascript
function Button(props) {
  return (
    <button onClick={props.onClick}>
      {props.buttonValue}
    </button>
  );
}
```

We've removed two lines of code, but we've also avoided extending
`React.Component`. `Button` just got a lot simpler to maintain and use.

## Your Problem with JSX Is You Don't Understand JSX

I've been avoiding it thus far, but if you're new to React you'll notice
that React mixes in something that looks like HTML directly with
JavaScript notation.

This confuses many newcomers to React because they think it IS HTML.

It's not. The markup in React is actually called JSX, and it has some
key differences from HTML that impact how you use it.

The most important thing to understand about JSX is that Babel (a
JavaScript compiler that React uses) is changing JSX over to JavaScript
object notation behind the scenes.

When you write in JSX:

```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

Babel converts it to:

```javascript
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement` is how the ReactDOM knows what to render.

You could write your entire React components using
`React.createElement` directly instead of JSX. But JSX is just nicer to
look at and since all web devs know HTML, it's more comprehensible what
will happen with that element.

Babel's conversion of JSX has some important implications, though.

First, `React.createElement()` only creates one element. That element
can have child elements nested within it, but Babel doesn't know what to
do with more than one element.

For example, this won't work in JSX:

```javascript
return (
  <h1>Hello World</h1>
  <p>Nice to meet you</p>
);
```

However, if we nest those elements inside another, it will work:

```javascript
return (
  <div>
    <h1>Hello World</h1>
    <p>Nice to meet you</p>
  </div>
);
```

If you find yourself needing to return multiple elements like this
often, you might consider using `React.Fragment` instead:

```javascript
return (
  <React.Fragment>
    <h1>Hello World</h1>
    <p>Nice to meet you</p>
  </React.Fragment>
);
```

React.Fragment allows you to add multiple elements without adding a
bunch of unnecessary <div>s to the DOM.

JSX also allows you to embed JavaScript directly into your syntax to be
evaluated after compilation:

```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

JSX is also safe against injection attacks. You can create JSX variables
from user inputs and then use those variables throughout your app
without any problems. React escapes all values embedded in JSX and it
converts everything to strings before rendering.

One more challenge that trips up newcomers is that JSX is closer to
JavaScript than to HTML. As such, it uses camelCase for all naming
schemes.

Notice I've used `onClick` in our `ButtonList` examples instead of the
standard HTML `onclick`.

Since JSX will become JavaScript, JavaScript keywords are off limits.
Therefore, if you need to specify a CSS class in JSX, you'll need to
call it `className`.

## Loops & Conditions in the render() Method

JSX and React impose some limitations on conditions and loops you can use
inside the `render()` method of a component.

This can be frustrating to newcomers. But often the solution is simply
to create a separate method that runs the loop or the condition and then
call that method from within `render()`.

One pattern you often see in React is the use of `.map()` to turn an
array into a set of elements.

For instance, it's common to keep a `this.state.history` with
information about what has happened in the past for a given component.
It's often formatted as an array of objects where
`this.state.history[0]` is the state of the component at the beginning
of the session and `this.state.history[this.state.history.length]` is
the most recent change.

Let's imagine you wanted to list out recent changes as a list. 

To do so, we'd use `.map()`:

```javascript
const history = this.state.history;

const changes = history.map((changeNum, changeDesc) => {
  return <li key={changeNum}>{changeDesc}</li>;
}
```

You can make `.map()` as complicated as you need it to be, creating
variables inside the arrow function you call in order to render the
elements, CSS classes, and content as needed.


## const vs let & Why React Prefers Immutability

I applaud you if you've made it this far into this guide. You're
committed to learning React fundamentals! 

Once you have this core understanding down, everything else you'll learn
about React is the fun part!

JavaScript introduced new keywords for declaring variables in ES6. A lot
has been written about the keywords `let` and `const` and why you would
use them instead of `var`. However, it's important to understand how
these new declarations affect React.

React officially prefers the use of `const` when declaring your
variables.

Of course, this means that you can't change the `const` later on in your
program. Instead, you'll need to make sure your declarations of constant
variables are well scoped inside functions and other scope blocks so
that values can be saved and returned.

That's not to say you won't use `let` or `var` in React! There are still
plenty of applications for mutable variables.

The reason React prefers immutability when possible is because so many 
other things are changing in React. It makes it easier to predict what 
your application will do when the underlying variables can't change.

In addition, immutability allows us do save past values of certain
variables. If we use a `const` inside a function and then add the value
of that `const` to `this.state.history` (and vice versa), then we have
the basis of state history, allowing us to undo/redo and revert to
certain points in time.

Consider the example we used above:

```javascript
const history = this.state.history;

const changes = history.map((changeNum, changeDesc) => {
  return <li key={changeNum}>{changeDesc}</li>;
}
```

Because we grab values from the state history, we know those values
won't need to change. When they do change, we'll save those changes as
new entries in `this.state.history`.

## Welcome to React!

Whew, that was a doozie of a guide! I hope it was useful.

Now that you have an understanding of the broader context of how React
fits together, developing your React web apps and understanding complex
projects in React gets easier.

Did I miss anything or make a mistake above? Please let me know in the
comments!

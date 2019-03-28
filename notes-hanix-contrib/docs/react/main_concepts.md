# Introducing JSX
## Embedding Expressions in JSX
You can put any valid JavaScript expression inside the curly braces in JSX

## JSX is an Expression Too
After compilation, JSX expressions become regular JavaScript function calls and evaluate to JavaScript objects.

This means that you can use JSX inside of if statements and for loops, assign it to variables, accept it as arguments, and return it from functions

## Specifying Attributes with JSX
You may use quotes to specify string literals as attributes or curly braces to embed a JavaScript expression in an attribute.

## JSX Prevents Injection Attacks
By default, React DOM escapes any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that’s not explicitly written in your application. Everything is converted to a string before being rendered.

# Rendering Elements
## Rendering an Element into the DOM
```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

## Updating the Rendered Element
React elements are immutable. Once you create an element, you can’t change its children or attributes,
the only way to update the UI is to create a new element, and pass it to `ReactDOM.render()`,
note that this method is implicitly called when using `setState` method.

# Components and Props
Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called **props**) and return React elements describing what should appear on the screen.

## Function and Class Components

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## Rendering a Component
When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object **props**.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

Note: Always start component names with a capital letter.

## Props are Read-Only
All React components must act like pure functions with respect to their props.

Of course, application UIs are dynamic and change over time. In the next section, we will introduce a new concept of **state**

# State and Lifecycle

Class components should always call the base constructor with props.

The `componentDidMount()` method runs after the component output has been rendered to the DOM.

If the component is ever removed from the DOM, React calls `componentWillUnmount()`.


You are free to add additional fields to the class manually if you need to store something that doesn’t participate in the data flow

## Using State Correctly

### Do Not Modify State Directly
The only place where you can assign this.state is the constructor.

### State Updates May Be Asynchronous
Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.


```jsx
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});

// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

## The Data Flows Down
Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.

Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree, this is commonly called a **top-down** or **unidirectional** *data flow*

# Handling Events
React events are named using camelCase, rather than lowercase.

With JSX you pass a function as the event handler, rather than a string.

You cannot return false to prevent default behavior in React. You must call preventDefault explicitly.

When you define a component using an ES6 class, a common pattern is for an event handler to be a method on the class

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

You have to be careful about the meaning of `this` in JSX callbacks. In JavaScript, class methods are not bound by default. If you forget to bind `this.handleClick` and pass it to onClick, `this` will be `undefined` when the function is actually called, if you refer to a method without () after it, such as `onClick={this.handleClick}`, you should **bind** that method.

If calling `bind` annoys you, there are two ways you can get around this. If you are using the experimental public class fields syntax, you can use class fields to correctly bind callbacks:

```jsx
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```
This syntax is enabled by default in Create React App.

If you aren’t using class fields syntax, you can use an arrow function in the callback:

```
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

The problem with this syntax is that a different callback is created each time the `LoggingButton` renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.

## Passing Arguments to Event Handlers
Inside a loop it is common to want to pass an extra parameter to an event handler. For example, if id is the row ID, either of the following would work:
```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
The above two lines are equivalent, and use arrow functions and `Function.prototype.bind` respectively.

In both cases, the e argument representing the React event will be passed as a second argument after the ID. With an arrow function, we have to pass it explicitly, but with bind any further arguments are automatically forwarded.

# Conditional Rendering

## Element Variables


```jsx
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;

    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

## Inline If with Logical && Operator

```jsx
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```
## Inline If-Else with Conditional Operator

```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

## Preventing Component from Rendering
To do this return `null` instead of its `render` output.

Returning `null` from a component’s `render` method does not affect the firing of the component’s lifecycle methods. For instance `componentDidUpdate` will still be called.

# Lists and Keys
## Rendering Multiple Components

You can build collections of elements and include them in JSX using curly braces `{}`.

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

## Keys
Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity

We don’t recommend using indexes for keys if the order of items may change. This can negatively impact performance and may cause issues with component state

Keys serve as a hint to React but they don’t get passed to your components. If you need the same value in your component, pass it explicitly as a prop with a different name:

```jsx
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

# Forms

## Controlled Components

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```



## Controlled Input Null Value

Specifying the value prop on a controlled component prevents the user from changing the input unless you desire so. If you’ve specified a value but the input is still editable, you may have accidentally set value to undefined or null.

The following code demonstrates this. (The input is locked at first but becomes editable after a short delay.)
```jsx
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);
```

## Handling Multiple Inputs

```jsx
// ES6 computed property name syntax to update the state key corresponding to the given input name:
this.setState({
  [name]: value
});

//It is equivalent to this ES5 code:
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

# Lifting State Up
Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let’s see how this works in action.

# Composition vs Inheritance

## Containment
Some components don’t know their children ahead of time. This is especially common for components like `Sidebar` or `Dialog` that represent generic **boxes**.

We recommend that such components use the special `children` prop to pass children elements directly into their output:

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

This lets other components pass arbitrary children to them by nesting the JSX:

```jsx
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

Anything inside the `<FancyBorder>` JSX tag gets passed into the `FancyBorder` component as a children prop. Since `FancyBorder` renders `{props.children}` inside a `<div>`, the passed elements appear in the final output.

While this is less common, sometimes you might need multiple **holes** in a component. In such cases you may come up with your own convention instead of using children:

```jsx
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

## So What About Inheritance?
At Facebook, we use React in thousands of components, and we haven’t found any use cases where we would recommend creating component inheritance hierarchies.

Props and composition give you all the flexibility you need to customize a component’s look and behavior in an explicit and safe way. Remember that components may accept arbitrary props, including primitive values, React elements, or functions.

If you want to reuse non-UI functionality between components, we suggest extracting it into a separate JavaScript module. The components may import it and use that function, object, or a class, without extending it.

# Thinking in React

## Step 1: Break The UI Into A Component Hierarchy

## Step 2: Build A Static Version in React (don’t use state at all)

## Step 3: Identify The Minimal (but complete) Representation Of UI State

In order to define state. Simply ask three questions about each piece of data:

1. Is it passed in from a parent via props? If so, it probably isn’t state.
2. Does it remain unchanged over time? If so, it probably isn’t state.
3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

## Step 4: Identify Where Your State Should Live

Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. **This is often the most challenging part for newcomers to understand**.

## Step 5: Add Inverse Data Flow
Since components should only update their own state, callbacks should be passed to the children components in order to allow them the change the state
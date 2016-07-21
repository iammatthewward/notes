## Key concepts
* virtual dom
* reconciliation
* state
* props

## Components
* React component
  - Classes
  - Functions
* React instance
* DOM node

## React ecosystem
* React
* React router
* Redux
* Relay

## React project with Express server and Babel transpiler
### Initialisation
* `npm init -y` to initialise your project.
* `npm install --save react react-dom express body-parser express-react-views babel` to install dependencies.

### Server set up
Below is an example server setup that will use JSX to render views from within the `views` directory.
TBC - make sure all is necessary
* Run the server with `node program.js` and visit `http://localhost:3000` in the browser.

```
var express = require('express');
var app = express();

app.set('port', 3000));
app.set('view engine', 'jsx');
app.set('views', __dirname + '/views');
app.engine('jsx', require('express-react-views').createEngine({ transformViews: false }));

require('babel/register')({
  ignore: false
});

app.use('/', function(req, res) {
  res.render('index', '');
});

app.listen(app.get('port'), function() {});

```

### Components
React is component based, meaning you can create many independent components, then combine them together to make a web app. Often components use the JSX JavaScript extension, which looks similar to XML and is transformed by React into native JavaScript. In the following example, React takes a JSX input and transforms it into arguments that are passed to `React.createElement`:

```
// Input (JSX):
var app = <Nav color="blue" />;
// Output (JS):
var app = React.createElement(Nav, {color: "blue"});
```

A React component is rendered by creating a variable that starts with an upper-case letter, such as:  

`class MyComponent extends React.Component {/*...*/};`.

A HTML tag is rendered by creating a variable that starts with a lower-case letter, such as:   

`let myElement = <MyComponent someProperty={true} />;`.

#### Namespaced Components

## Types of "model" data in React
React has two types of "model" data: props and state. The clear overlying distinction between the two is that props, as far as the component receiving them is concerned, are *immutable*, whereas state is used for *mutable* values. It is with this distinction in mind that the choice of which data type to use should be made when designing your component.

TL;DR:
* Props are *imutable*, and so a component cannot change it's props; it can only define the props for it's child components.
* State are *mutable* , and so a component can redefine it's state. It cannot, however, redefine the state of it's child components.

### Props
Passing values from a parent component to a child component is done through it's properties - or **props**. This is done by defining attributes in the parent component, then accessing them in the child component through `this.props.attributeName`.

When a component references a child component, it can pass properties to the child component which then uses them as necessary. In the following example props are passed down from the `TodoList` parent component to the `Todo` child component in the form of a named attribute, and a child value.

When the `Todo` child component references `this.props.title` it is accessing the value set to the named attribute `title` in the parent component. When it references `this.props.children` it is accessing the child values of the `Todo` component that are between the `<Todo` tags - in this case, it is returned the value 'Sandwich'. In both cases, the expressions used to access the values are wrapped in curly braces (`{}`).

```
// parent component
class TodoList extends React.Component {
  render() {
    return (
      <div className="todoList">
        <Todo title="Eat lunch">Sandwich</Todo>
      </div>
    );
  }
}

// child component
class Todo extends React.Component {
  render() {
    return (
      <tr>
        <td style={style.tableContent}>{this.props.title}</td>
        <td style={style.tableContent}>{this.props.children}</td>
      </tr>
    );
  }
}
```

In the example above static values are being used to create the child component from it's parent, which is not ideal. A better method is to have components created dynamically based on values stored in their (as in the example below). When creating components this way, React uses the `key` attribute to keep track of each component in the VirtualDOM, and React will output an error message to the console if done without using the `key` attribute.

```
// dynamically created Todo components
class TodoList extends React.Component {
  render() {
    var todo = this.props.data.map(function(obj) {
      return <Todo title={obj.title} key={obj.title}>{obj.detail}</Todo>
    });
    return (
      <div className="todoList">
        <table style={style.table}>
          <tbody>
            {todo}
          </tbody>
        </table>
      </div>
    );
  }
}
```

#### Proptypes
PropTypes are used to validate that all the necessary properties are passed to components, and are of the right type. This helps ensure components are being used correctly. This is done by specifying propTypes to verify that the data passed in is in the correct form, as well as specifying if a property is required by a component.

PropTypes are added simply by extending the component as follows:   
```
MyComponent.propTypes = {
    name:   React.PropTypes.string.isRequired,
    alt:    React.PropTypes.string
};
```
As per the example, setting a prop to required is done by adding `.isRequired` to the end of the PropType.

Below are examples of the different PropTypes available:   
```
// Basic types
React.PropTypes.string
React.PropTypes.number
React.PropTypes.object
React.PropTypes.func
React.PropTypes.bool
React.PropTypes.any

// React elements
React.PropTypes.element
React.PropTypes.node

// Enumerables
React.PropTypes.oneOf(['M','F'])
React.React.PropTypes.oneOfType([React.PropTypes.string, React.PropTypes.number ])

// Arrays and objects
React.PropTypes.array
React.PropTypes.arrayOf(React.PropTypes.number)
React.PropTypes.object
React.PropTypes.objectOf(React.PropTypes.number)
React.PropTypes.instanceOf(OtherComponent),

// Custom validation
customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error('Validation failed!');
    }
  ```

### State
The state of a component is set a default value when the component is mounted, and this state can have it's value changes, usually as a result of user events. State should be used to hold values that we wish to be changed over time, and is private to each component.

A component can hold multiple values in it's state, in the form of a JavaScript object: `this.state = {key1: value1, key2: value2};`. Any component that utilises state needs to have an initial state set and this can be done within the component's constructor:

```
class ComponentName extends React.Component {
  constructor(props) {
    super(props);
    this.state = {key: value};
  }
}
```

State is usually changed through user events, and this happens within the function called upon when an event happens using `setState`:

```
// Event that triggers function
<button onClick={this.handleClick}>Click me</button>

// Function that changes state
handleClick() {
  this.setState({key: new-value})
}
```

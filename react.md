---
title: React.js
layout: default
---

### Basic class

```js
React.createClass({
  render: function () {
    return (
      <div>Hello {this.props.name}</div>
    );
  }
});
```

### Using components

```js
var Component = React.createClass({ ... });

var compo = React.render(<Component />, mountnode);
```

### States

```js
this.setState({ editing: true });
this.state.editing === true

this.replaceState({ ... });
```

### Properties

```js
this.setProps({ fullscreen: true });
this.props.fullscreen === true

this.replaceProps({ ... });
```

### [API](http://facebook.github.io/react/docs/component-api.html)

```js
c.getDOMNode()
c.forceUpdate()
c.isMounted()
```

### [Methods](http://facebook.github.io/react/docs/component-specs.html)

```js
{
  render: function()
  getInitialState: function()
  getDefaultProps: function()
  mixins: []
  propTypes: {}       /* for validation */
  statics: { .. }     /* static methods */
  displayName: '..'   /* automatically filled in by jsx */
}
```

### [Lifecycle](http://facebook.github.io/react/docs/component-specs.html)

```js
componentWillMount()
componentDidMount()

// not on initial render
componentWillReceiveProps(props)
shouldComponentUpdate(props, state)
componentWillUpdate(props, state)
componentDidUpdate(prevProps, prevState)

componentWillUnmount()

// ...there is no DidUnmount or ReceiveState.
```

### Initial states

    React.createClass({
      getInitialState: function () {
        return {data: []};
      },

      render: function () {
        return (
          <CommentList data={this.state.data} />
        );
      }
    });

### Default properties

    React.createClass({
      getDefaultProps: function () {
        return {name: ''};
      }
    );

### Before rendering

    React.createClass({
      componentWillMount: function () {
        $.get(this.props.url, function (data) {
          this.setState(data);
        });
      },

      render: function () {
        return (
          <CommentList data={this.state.data} />
        );
      }
    });

### Actions

    React.createClass({
      handleSubmit: function (event) {
        name = this.refs['name'].getDOMNode().value;
        // see two-way binding below
      },
      
      render: function() {
        return (
          <form onSubmit={this.handleSubmit}>
            Name: <input ref="name">
          </form>
        );
      }
    })

### [Two-way binding](http://facebook.github.io/react/docs/two-way-binding-helpers.html)

```js
React.createClass({
  mixins: [React.addons.LinkedStateMixin],
  getInitialState: function() {
    return {value: 'Hello!'};
  },
  render: function() {
    return <input type="text" valueLink={this.linkState('value')} />;
  }
});
// LinkedStateMixin adds a method to your React component called
// linkState(). 
```

### Lists

    var TodoList = React.createClass({
      render: function() {
        var createItem = function(itemText) {
          return <li>{itemText}</li>;
        };
        return <ul>{this.props.items.map(createItem)}</ul>;
      }
    });

### [Property validation](http://facebook.github.io/react/docs/reusable-components.html#prop-validation)

```js
React.createClass({
  propTypes: {
    // required
    requiredFunc: React.PropTypes.func.isRequired,
    requiredAny: React.PropTypes.any.isRequired,

    // primitives, optional by default
    bool: React.PropTypes.bool,
    func: React.PropTypes.func,
    number: React.PropTypes.number,
    string: React.PropTypes.string,
  }
});

Also:

```js
  propTypes: {
    message: React.PropTypes.instanceOf(Message), // instanceof any Class
    element: React.PropTypes.element, // A React element
    enum: React.PropTypes.oneOf(['News', 'Photos']),
    node: React.PropTypes.node, // num, string, element, or array of these

    // any of below
    union: React.PropTypes.oneOfType([
      React.PropTypes.string,
      React.PropTypes.number
    ]),

    // arrays
    array: React.PropTypes.array,
    arrayOf: React.PropTypes.arrayOf(React.PropTypes.number),

    // objects
    object: React.PropTypes.object,
    objectOf: React.PropTypes.objectOf(React.PropTypes.number),

    // An object taking on a particular shape
    object2: React.PropTypes.shape({
      color: React.PropTypes.string,
      fontSize: React.PropTypes.number
    }),

    // custom validator
    customProp: function(props, propName, componentName) {
      if (!/matchme/.test(props[propName])) {
        return new Error('Validation failed!');
      }
    }
  }
```

### [Class set](http://facebook.github.io/react/docs/class-name-manipulation.html)

    render: function() {
      var cx = React.addons.classSet;
      var classes = cx({
        'message': true,
        'message-important': this.props.isImportant,
        'message-read': this.props.isRead
      });
      // same final string, but much cleaner
      return <div className={classes}>Great Scott!</div>;
    }

### [Propagating properties to children](http://facebook.github.io/react/docs/transferring-props.html)

    var VideoPlayer = React.createClass({
      render: function() {
        return <VideoEmbed {...this.props} controls='false' />;
      }
    });

    <VideoPlayer src="video.mp4" />

### Mixins

    var TickTock = React.createClass({
      mixins: [SetIntervalMixin]
    }

    SetIntervalMixin = {
      componentWillMount: function() { .. }
    }

----

### See also

* [Animations](http://facebook.github.io/react/docs/animation.html)

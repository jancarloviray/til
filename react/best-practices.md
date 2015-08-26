# Best Practice React

## jQuery vs React

*jQuery Style*

**Event Handler <---> Change DOM**

*React.js Style*

**Event Handler --> State --> render()**

- Event handler changes state. React does a diff in virtual dom and renders.

## Properties vs State

- Properties are immutable.
- State is mutable. **State is only reserved for interactiviy.** Therefore, *anything that is going to change, it goes into State*. State should store the most simplest values.

- Props in `getInitialState` is an anti-pattern.
- Avoid duplication of "source of truth" which is where the real data is. Whenever possible, compute values on the fly to ensure they don't get out of sync later on and cause maintenance trouble.
- *It is often easier and wiser to move the state higher in component hierarchy*
- You should never alter state directly. Call it through `setState`

- Most importantly, the state of a component should not depend on the props passed in (state as in state of the component - not of the app). *A huge "code smell" is when you start seeing the state depending on the props. For example, this is not good: `constructor(props){ this.state = { fullName: ${props.first} ${props.last}}}`. These guys should go in the `render`

- Leave calculations and conditionals to the render function.
- Note that everytime `state` is changed, `render` is called again.

### What Should Go in State?

- *State should contain data that a component's event handlers may change to trigger a UI update.* State must be very very small and JSON-serializable. When building a stateful component, think about the minimal possible representation of its state, and only store those properties in `this.state`.

### What Should Not Go in State?

- Computed data should not go in state
- React components must not go in state. They must be built in `render`
- Duplicated data from props. Try to use props as source of truth where possible. 

### What Shouldn't Go in State?

## Controlled Inputs

- If you specify a value, you are using a controlled input and you have to provide an `onChange` handler for any controlled input, otherwise it is set in stone. *If you're looking at loading an existing entity and editing it, you want controlled inputs and you want to set the values accordingly*
- A benefit of controlled inputs is that users can't edit them, so if you had some properties locked down (maybe for read only, security reasons), when the user attempts to save, even if they edited the HTML directly, React should pull the property's value from the vDOM representation.

## Misc

- use refs instead of target.name
- components can't be concerned with both presentation and data-fetching
- [separating component concerns](https://gist.github.com/chantastic/fc9e3853464dffdb1e3c)

## PropTypes

- Always use proptypes, especially considering if/when application grows.
- Every `propType` that is not `isRequired` should have a corresponding field in `getDefaultProps()`

```javascript
propTypes: {
	arrayProp: React.PropTypes.array,
	boolType: React.PropTypes.bool,
	funcProp: React.PropTypes.func,
	numProp: React.PropTypes.number,
	objProp: React.PropTypes.object,
	stringProp: React.PropTypes.string
}
```

## Avoid State

- Try to avoid state as much as possible. Worst place to call setState is in `componentDidMount` and in `componentDidUpdate` because most render calls will then render twice.
- Have the state flow down through your components purely as `props` - have no state actually be managed by components. Set state in stores to hold your application state and *have event-driven actions that modify stores*
- Components must primarily rely on `props`. *Instead of calling `setState`, components must communicate with central data store of some sort*

## Do More in Render()

- avoid logic in `componentWillReceiveProps` or `componentWillMount`. Instead, move it to `render()` 

## Component Organisation

```javascript
React.createClass({
	propTypes: {},
	mixins: [],

	getInitialState: function(){},
	getDefaultProps: function(){},

	componentWillMount: function(){},
	componentWillReceiveProps: function(){},
	componentWillUnmount: function(){},

	_parseData: function(){},
	_onSelect: function(){},

	render: function(){}
});
```

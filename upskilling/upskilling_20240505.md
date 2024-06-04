# React state, React philosophy, and lifecycle hooks

## React Philosophy

React's philosophy is very simple: ***data should only flow in one direction, which is from the parent to its children.***

This is to provide for better debugging: When there is only one owner of data, then the data can and will only be changed in one place.

There are two types of data in React: **Props** and **State**.

**Props** provide a way for parent components to passs data into child components, whereas **State** is a way for a component to maintain its own data.

Event listeners, passed as functions into inbuilt components, are no different to other props. They are just a prop which has a value equal to a function. The event object is directly populated with the DOM element's event, as the mapping from a **Virtual DOM -> Real DOM** is simpler, but is much harder to go in the reverse direction.

## Class Component Lifecycle Hooks

Lifecycle hooks are events which only live within class components, but not functional components.

In the old version of React (prior to 16.0.0), these lifecycle hooks could be split into three main categories: the **mounting stage**, the **updating stage**, and the **destroying stage**.

In the newer version of react (16.0.0 and above), there still are three stages, but some redundant hooks or hooks which encourage bad practices (against the React philosophy) have been removed.

### Old React hooks

The mounting stage consists of the following hooks:
- `constructor`: only runs once per component instance.
- `componentWillMount`: runs just before the component will be added to the DOm. normally speaking, this will only run once, but may run multiple times if React needs to take down the component internally.
- `render`: a function which returns a virtual DOM, which gets rendered as real DOM. This may run multiple times, but DO NOT USE setState HERE as that will cause an infinite loop.
- `componentDidMount`: runs once the component has been added to the DOm. This ONLY runs once.

The updating stage consists of the following hooks:
- `componentWillReceiveProps`: just before the props of a component are going to change.
- `shouldComponentChange`: a function which return value indicates to React whether or not to rerender the component.
- `componentWillUpdate`: runs as the component is about to rerender
- `componentDidUpdate`: runs just after the component finished rerendering.

The destroying stage consists of one hook:
- `componentWillUnmount`: runs just before the component will be taken out of the virtual DOM.

### New React hooks

The mounting stage consists of the following hooks:
- `constructor`
- `static getDerivedStateFromProps`: called right before any render, regardless of whether it's in the mounting or updating stage
- `render`
- `componentDidMount`

The updating stage consists of the following hooks:
- `static getDerivedStateFromProps`
- `shouldComponentUpdate`
- `render`
- `getSnapshotBeforeUpdate`: the real DOM has been constructed, but this fires just before the real DOM gets rendered onto the web page
- `componentDidUpdate`

The destroying stage consists of one hook:
- `componentWillUnmount`

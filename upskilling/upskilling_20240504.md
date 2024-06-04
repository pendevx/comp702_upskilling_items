# React 16

When I study, I like to take a small step back first and learn the history, or at least the previous major version(s) of the technology I'm about to use. Therefore, although React 18 is already released and React 19 is just around the corner, I still decided to begin with React 16.

The main point I studied were the following:
1. What is React?
2. What is ReactDOM?
3. What is JSX?

## What is React?

Put simply, React is a Javascript Libray, created to make frontend web development easier. 

React provides a set of APIs to make management of components of a web app more easier.

## What is ReactDOM?

The `ReactDOM` library is what "glues" React and the DOM together.

## What is JSX?

JSX is a language extension to JavaScript. It was created by FaceBook, and is the language used in React.

It allows embedding objects created via an HTML-like syntax into your Javascript code. However there is a catch - React Elements (elements created using JSX) are *not* HTML, they are simply just objects with a special definition, and is used internally in React to create something called the **Virtual DOM**.

React requires a *transpiler* to transpile the JSX code into valid React code, as understood by React itself. Commonly, transpilers such as **Babel** is sufficient to complete this task. However, there are also other transpilers out there, generally in the form of plugins to customize *build tools*.

In JSX, a piece of code such as `<h1>hello world <span>span</span></h1>` will be transpiled into: `React.createElement("h1", {}, "hello world", React.createElement("span", {}, "span"));`

### All JSX expressions can only have one root element

If in any case we need a piece of JSX which contains multiple root elements, we can wrap everything in a fragment (empty JSX containers) `<React.Fragment> </React.Fragment>`, or `<> </>`

Fragments will not be created in the real DOM, they are purely wrappers for JSX.

### All JSX expressions must have a closing tag (following the XML specifications)

For example, there are void elements in HTML (like `img` or `input`). In JSX, they must have a corresponding closing tag.

`<img src="">` Not allowed, should be: `<img src=""></img>` or `<img src="" />`

### Properties of React Element objects

The two most common properties are:
- `props`: properties passed to the component
- `props.children`: the logical children of the React element

Given the example:
```jsx
<Component a={1} b={2} c={3}>
    <Child />
    <div />
</Component>
```

The component `Component` will have the following properties:
- `props`: a javascript object `a=1, b=2, c=3`
- `props.children`: a javascript object `ReactElement $type=Child, ReactElement $type="div"`

#### The $type property

React elements use the `$type` property to differ between inbuilt components and user-defined components. For inbuilt components, the `$type` property is populated with a string, but for user-defined components, the `$type` property is populated with a class or a function. The way this field gets populated is depending on the letter casing of the first character.

An example is:
`<button>` will be transformed into a React Element with `$type = "button"`
`<MyComponent>` will be transformed into a React Element with `$type = class MyComponent` (the class may also be a function, depending on the definition of the component)
`<Button>` will be transformed into a React Element with `$type = class Button` (the class may also be a function, depending on the definition of the component)

**Therefore, the letter casing of the first character of a component's name does matter!**

### JSX can embed javascript expressions

```jsx
const div = <div>
    {a} * {b} = {a * b}
</div>
```
This is what it gets compiled into:

```js
const div = React.createElement("div", {}, `${a} * ${b} = ${a * b}`);
```

#### Embedded JS expressions which resolve to null, undefined, or false, will not be rendered

It is equivalent to doing `React.createElement(undefined, {})`, which will populate the React Element's `$type` property with `undefined`. Since the `$type` property is what React uses to differ between inbuilt components (populated with strings) and user-defined components (populated with a function or a Javascript class), then it will be unable to be transformed into HTML to be rendered onto the page by the `ReactDOM`.

### Comments in JSX must be JS comments, not HTML comments

`<!-- -->` is wrong, `{/* */}` is correct

### Regular JS objects cannot be children of a JSX expression

### JS arrays of primitive types or React elements may be children of a React expression - the primitives will be converted into text nodes by `ReactDOM`.

### Arrays of JSX which get rendered must contain a `key` property.

### JSX attribute values can be javascript expressions.

Example: `<div title={someVariable}></div>`

### JSX/HTML classes must be specified using `className` instead of `class`.

This is to avoid keyword conflicts, as the Javascript language contains the keyword `class`, so since it's already reserved, React must use a different term instead. So they picked `className`!

### For security purposes, JSX will automatically escape the result of your embedded javascript expressions

This is so no code obtained from an unknown origin will be executed, potentially damaging or causing a security risk to the viewer of the website (XSS).

#### `dangerouslySetInnerHTML` is a property you can add to JSX elements if you are really really really wanting to add html as a JS expression

```jsx
const content = "<h1>sadflkasdjf</h1>";
const div = <div dangerouslySetInnerHTML={{
    __html: content
}}>
</div>
```

### JSX objects, once created, are immutable

React uses `Object.freeze()` to make the React Element immutable. This is done internally

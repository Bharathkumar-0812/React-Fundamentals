# React Class Components & Lifecycle

This document explains **React Class Components** and their **lifecycle methods**, assuming prior knowledge of **functional components and hooks**. The goal is to map old concepts to modern mental models for clarity.

---

## 1. What is a Class Component

A **class component** is a JavaScript class that:

* Extends `React.Component`
* Must contain a `render()` method
* Manages state and lifecycle via class methods

### Basic Example

```jsx
class Hello extends React.Component {
  render() {
    return <h1>Hello World</h1>;
  }
}
```

Unlike functional components, class components persist as objects and React interacts with them throughout their lifecycle.

---

## 2. State in Class Components

State in class components is stored as an **object** and initialized inside the constructor.

### Example

```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  render() {
    return <p>{this.state.count}</p>;
  }
}
```

### Updating State

```js
this.setState({ count: this.state.count + 1 });
```

Notes:

* Never mutate `this.state` directly
* `setState` is asynchronous and batched

---

## 3. Lifecycle Phases Overview

Class components go through **three main phases**:

1. **Mounting** – Component is created and inserted into the DOM
2. **Updating** – Component re-renders due to state or props changes
3. **Unmounting** – Component is removed from the DOM

---

## 4. Mounting Phase

### 4.1 `constructor(props)`

* Initializes state
* Binds methods (if needed)

```js
constructor(props) {
  super(props);
  this.state = { data: null };
}
```

---

### 4.2 `render()`

* Mandatory method
* Must be pure
* Returns JSX

```js
render() {
  return <div>UI</div>;
}
```

No side effects should occur here.

---

### 4.3 `componentDidMount()`

* Runs once after initial render
* Used for:

  * API calls
  * Subscriptions
  * DOM manipulation

```js
componentDidMount() {
  this.fetchData();
}
```

---

## 5. Updating Phase

Triggered when:

* State changes via `setState`
* Props change

---

### 5.1 `render()`

Runs again on every update.

---

### 5.2 `componentDidUpdate(prevProps, prevState)`

* Runs after update
* Requires condition checks to avoid infinite loops

```js
componentDidUpdate(prevProps) {
  if (prevProps.id !== this.props.id) {
    this.fetchData();
  }
}
```

---

## 6. Unmounting Phase

### `componentWillUnmount()`

* Cleanup method
* Used to:

  * Clear timers
  * Remove event listeners
  * Cancel subscriptions

```js
componentWillUnmount() {
  clearInterval(this.timer);
}
```

---

## 7. Lifecycle vs Hooks Mapping

| Hooks (Functional)           | Class Component        |
| ---------------------------- | ---------------------- |
| `useState`                   | `this.state`           |
| `useEffect(() => {}, [])`    | `componentDidMount`    |
| `useEffect(() => {}, [dep])` | `componentDidUpdate`   |
| Cleanup function             | `componentWillUnmount` |

---

## 8. Key Takeaways

* Class components are common in legacy codebases
* Understanding lifecycle methods improves debugging skills
* New React development favors functional components and hooks
* Class components are still required for **Error Boundaries**

---

**Next Topics Suggested:**

* `this` keyword and method binding
* `setState` behavior and batching
* Controlled vs uncontrolled components
* Error Boundaries

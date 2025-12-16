# React Class Components – Props and State Handling

This document explains **props and state handling in React class components**, how they differ, how they interact, and common patterns used in real applications.

---

## 1. Props vs State (Core Difference)

| Props                  | State                       |
| ---------------------- | --------------------------- |
| Passed from parent     | Managed by component itself |
| Read-only              | Mutable via `setState`      |
| Used for configuration | Used for dynamic data       |

Short version:

* **Props** come from outside
* **State** lives inside

---

## 2. Using Props in Class Components

Props are accessed via `this.props`.

### Example

```jsx
class Greeting extends React.Component {
  render() {
    return <h1>Hello {this.props.name}</h1>;
  }
}
```

Props are:

* Immutable inside the component
* Controlled by the parent

Attempting to modify props is illegal React behavior.

---

## 3. Default Props

Used when a prop is not passed.

```js
class Button extends React.Component {
  static defaultProps = {
    type: 'button'
  };

  render() {
    return <button>{this.props.type}</button>;
  }
}
```

---

## 4. Props Destructuring

Cleaner and recommended.

```js
render() {
  const { name, age } = this.props;
  return <p>{name} - {age}</p>;
}
```

---

## 5. State Usage Recap

State is accessed via `this.state`.

```js
render() {
  const { count } = this.state;
  return <p>{count}</p>;
}
```

State updates trigger re-render. Props updates also trigger re-render.

---

## 6. When Props Change

Props change when the **parent re-renders** with new values.

React automatically:

* Calls `render()`
* Triggers update lifecycle methods

---

## 7. Deriving State from Props (Be Careful)

This is advanced and often misused.

### Anti-Pattern

```js
constructor(props) {
  super(props);
  this.state = { value: props.value };
}
```

Problem:

* State will not update when props change

---

## 8. Correct Pattern: Syncing Props to State

Only when absolutely necessary.

```js
static getDerivedStateFromProps(props, state) {
  if (props.value !== state.prevValue) {
    return {
      value: props.value,
      prevValue: props.value
    };
  }
  return null;
}
```

Notes:

* Runs before render
* Cannot access `this`
* Used rarely

---

## 9. Passing State as Props (Parent → Child)

```jsx
class Parent extends React.Component {
  state = { count: 0 };

  render() {
    return <Child count={this.state.count} />;
  }
}
```

Child:

```js
class Child extends React.Component {
  render() {
    return <p>{this.props.count}</p>;
  }
}
```

---

## 10. Lifting State Up

When multiple components need the same data:

* Move state to the closest common parent
* Pass it down via props

This avoids duplicated state and sync issues.

---

## 11. Props vs State Decision Guide

Use **props** when:

* Data comes from parent
* Component is reusable

Use **state** when:

* Data changes over time
* Component owns the behavior

---

## 12. Mental Mapping to Hooks

| Hooks              | Class Components |
| ------------------ | ---------------- |
| Function arguments | `this.props`     |
| `useState`         | `this.state`     |
| Lifting state      | Same pattern     |

---

## 13. Key Takeaways

* Props are immutable
* State is internal and mutable
* Avoid copying props into state
* Lift state when sharing data

---

**Next Recommended Topics:**

* `this` keyword and method binding
* Controlled vs uncontrolled components
* Event handling in class components
* Error boundaries

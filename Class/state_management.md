# React Class Components – State Management

This document focuses on **state management in React class components**, with special attention to **updating state based on previous state**. This is critical knowledge and a common source of bugs.

---

## 1. What is State in Class Components

State is an **internal data object** owned by a component. When state changes, React re-renders the component.

In class components, state:

* Lives in `this.state`
* Must be updated using `this.setState()`
* Is merged shallowly (not replaced entirely)

---

## 2. Initializing State

State is usually initialized inside the constructor.

```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
      loading: false
    };
  }

  render() {
    return <p>{this.state.count}</p>;
  }
}
```

Notes:

* `super(props)` is mandatory
* State must be an object

---

## 3. Updating State (Basic Form)

The simplest way to update state is by passing an object to `setState`.

```js
this.setState({ count: 1 });
```

Important behavior:

* Updates are **asynchronous**
* Multiple updates may be **batched**

This means you **cannot rely on `this.state` immediately after calling `setState`**.

---

## 4. The Core Problem: Previous State

This is where most beginners (and some seniors) mess up.

### ❌ Incorrect Example

```js
this.setState({ count: this.state.count + 1 });
this.setState({ count: this.state.count + 1 });
```

Expected result: `count + 2`

Actual result: `count + 1`

Why?

* React batches updates
* `this.state` does not update immediately

---

## 5. Correct Way: Functional `setState`

When new state depends on **previous state**, you must pass a function to `setState`.

### ✅ Correct Example

```js
this.setState((prevState) => ({
  count: prevState.count + 1
}));
```

This function:

* Receives the **latest** state
* Runs safely even with batching

---

## 6. Multiple Updates Using Previous State

```js
this.setState((prevState) => ({ count: prevState.count + 1 }));
this.setState((prevState) => ({ count: prevState.count + 1 }));
```

Final result:

```
count + 2
```

This works because each update receives the updated previous state.

---

## 7. Using Previous Props in State Updates

The functional form also receives `props`.

```js
this.setState((prevState, props) => ({
  total: prevState.total + props.step
}));
```

Use this when state depends on props.

---

## 8. Callback After State Update

Because `setState` is async, React provides a callback.

```js
this.setState(
  { loading: false },
  () => {
    console.log('State updated');
  }
);
```

This runs **after** the update is committed.

---

## 9. State Merging Behavior

`setState` performs a **shallow merge**.

```js
this.state = { user: { name: 'Alex', age: 20 } };

this.setState({ user: { age: 21 } });
```

Result:

```js
{ user: { age: 21 } }
```

Important:

* Nested objects are **not merged**
* You must spread manually

### Correct Update

```js
this.setState((prevState) => ({
  user: {
    ...prevState.user,
    age: 21
  }
}));
```

---

## 10. Mental Mapping to Hooks

| Hooks                  | Class Components                                |
| ---------------------- | ----------------------------------------------- |
| `setCount(c => c + 1)` | `setState(prev => ({ count: prev.count + 1 }))` |
| Async updates          | Async updates                                   |
| Batching               | Batching                                        |

---

## 11. Key Takeaways

* Never update state directly
* Use functional `setState` when relying on previous state
* Assume `setState` is asynchronous
* Shallow merge can break nested objects

---

**Next Logical Topics:**

* `this` keyword and method binding
* Controlled vs uncontrolled components
* Error boundaries
* Lifting state and state ownership

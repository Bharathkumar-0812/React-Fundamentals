# React Class Components – Conditional Rendering

This document explains **conditional rendering in React class components**. This is how components decide *what* to show and *when*, without turning your JSX into an unreadable mess.

---

## 1. What is Conditional Rendering

Conditional rendering means:

* Rendering different UI based on **state** or **props**
* Letting logic decide the output of `render()`

In class components, this logic lives entirely inside the `render()` method.

---

## 2. The Golden Rule

The `render()` method:

* Can contain JavaScript logic
* Must return **valid JSX** (or `null`)

React does not care *how* you decide. It only cares *what* you return.

---

## 3. Using `if` Statements (Clean and Boring)

Best for complex conditions.

```jsx
class Status extends React.Component {
  render() {
    const { isLoggedIn } = this.props;

    if (!isLoggedIn) {
      return <p>Please log in</p>;
    }

    return <p>Welcome back</p>;
  }
}
```

Pros:

* Very readable
* Scales well

Cons:

* Slightly more code

---

## 4. Using Ternary Operator

Best for simple conditions.

```jsx
class Status extends React.Component {
  render() {
    return (
      <p>
        {this.props.isLoggedIn ? 'Welcome back' : 'Please log in'}
      </p>
    );
  }
}
```

Use this when:

* The condition is short
* The UI difference is minimal

---

## 5. Logical AND (`&&`) Rendering

Used when you want to render something **only if a condition is true**.

```jsx
class Notification extends React.Component {
  render() {
    return (
      <>
        {this.props.hasMessage && <p>New Message</p>}
      </>
    );
  }
}
```

Warning:

* `0 && <Component />` renders `0`

---

## 6. Conditional Rendering with State

```jsx
class Loader extends React.Component {
  state = { loading: true };

  render() {
    if (this.state.loading) {
      return <p>Loading...</p>;
    }

    return <p>Data Loaded</p>;
  }
}
```

State-driven rendering is the most common pattern.

---

## 7. Rendering `null`

Returning `null` means rendering nothing.

```jsx
class Warning extends React.Component {
  render() {
    if (!this.props.show) {
      return null;
    }

    return <p>Warning!</p>;
  }
}
```

Useful for:

* Feature toggles
* Permissions
* Optional UI

---

## 8. Extracting Conditional Logic

Avoid cluttering JSX with logic.

```js
renderContent() {
  if (this.state.error) return <p>Error</p>;
  if (this.state.loading) return <p>Loading</p>;
  return <p>Success</p>;
}

render() {
  return <div>{this.renderContent()}</div>;
}
```

Cleaner and easier to test.

---

## 9. Common Anti-Patterns

Avoid:

* Nested ternaries
* Long logical chains in JSX
* Mixing business logic inside JSX

If JSX starts to look like algebra, you’ve gone too far.

---

## 10. Mental Mapping to Hooks

| Hooks                | Class Components       |
| -------------------- | ---------------------- |
| Conditional JSX      | Same pattern           |
| `if` inside function | `if` inside `render()` |
| `return null`        | `return null`          |

Conditional rendering does not change across paradigms.

---

## 11. Key Takeaways

* Conditional rendering controls UI flow
* Use `if` for complex logic
* Use ternary for simple cases
* Keep JSX readable

---

**Next Logical Topics:**

* Event handling in class components
* Refs and DOM access
* Context API (class-based)
* Error boundaries

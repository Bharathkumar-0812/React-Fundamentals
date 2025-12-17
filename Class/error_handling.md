# React Class Components – Error Handling & Performance Improvement

This document covers **error handling** and **performance optimization techniques** in **React class components**. This is the difference between apps that merely work and apps that survive real users.

---

## PART 1: ERROR HANDLING

---

## 1. Why Error Handling Matters

In React:

* A single uncaught error can crash the entire UI
* Users do not care *why* it broke

Error handling is about **failure containment**, not perfection.

---

## 2. What Are Error Boundaries

Error Boundaries are **special class components** that:

* Catch JavaScript errors in child components
* Prevent the whole app from crashing
* Display fallback UI

Important:

* Error boundaries **only exist in class components**

---

## 3. Creating an Error Boundary

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error(error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong</h2>;
    }

    return this.props.children;
  }
}
```

---

## 4. Using an Error Boundary

```jsx
<ErrorBoundary>
  <Dashboard />
</ErrorBoundary>
```

Best practice:

* Wrap **high-risk areas**, not the entire app blindly

---

## 5. What Error Boundaries Do NOT Catch

They do NOT catch errors in:

* Event handlers
* Async code (promises, `setTimeout`)
* Server-side rendering

Those require manual handling.

---

## PART 2: PERFORMANCE IMPROVEMENT

---

## 6. Why React Apps Become Slow

Common reasons:

* Unnecessary re-renders
* Heavy logic inside `render()`
* Large component trees

Performance issues are usually architectural, not magical.

---

## 7. `shouldComponentUpdate`

This lifecycle method controls re-rendering.

```js
shouldComponentUpdate(nextProps, nextState) {
  return nextProps.value !== this.props.value;
}
```

If it returns `false`, React skips rendering.

---

## 8. `PureComponent`

`PureComponent` implements shallow comparison automatically.

```js
class List extends React.PureComponent {
  render() {
    return <ul>{this.props.items}</ul>;
  }
}
```

Use when:

* Props and state are simple
* You avoid mutating objects

---

## 9. Avoiding Re-Renders (Best Practices)

* Keep state minimal
* Lift state only when necessary
* Avoid inline functions in render
* Avoid creating new objects repeatedly

Small habits, large gains.

---

## 10. Expensive Calculations

Move heavy logic **outside render**.

```js
getComputedValue() {
  return expensiveCalculation(this.props.data);
}
```

Or compute once and store in state when possible.

---

## 11. Key-Based Rendering

Incorrect keys cause re-render chaos.

```jsx
items.map(item => <Item key={item.id} />)
```

Never use array index as key unless you enjoy bugs.

---

## 12. Lazy Loading (Concept)

Split heavy components:

* Load only when needed
* Reduce initial load time

Classes support this via dynamic imports.

---

## 13. Performance vs Readability

Do not optimize blindly.

Rules:

* Measure first
* Optimize bottlenecks
* Prefer clarity over micro-optimizations

---

## 14. Mental Mapping to Hooks

| Hooks World       | Class Components        |
| ----------------- | ----------------------- |
| `React.memo`      | `PureComponent`         |
| Dependency arrays | `shouldComponentUpdate` |
| Error boundaries  | Same (class-only)       |

---

## 15. Key Takeaways

* Error boundaries prevent total app crashes
* Only class components can catch render errors
* Performance issues come from re-renders
* Optimize deliberately, not emotionally

---

**You have now covered the core of React class components.**

Next optional upgrades:

* Context API (class-based)
* Higher-order components (HOCs)
* Advanced performance profiling

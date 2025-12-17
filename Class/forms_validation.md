# React Class Components – Forms and Basic Validation

This document covers **handling forms and basic validation in React class components**. Forms are where props, state, and event handling finally collide, so precision matters.

---

## 1. Forms in React (Core Idea)

In React:

* Form inputs are usually **controlled components**
* Input value is driven by **state**
* User input updates state via events

This gives React full control over the UI.

---

## 2. Controlled Form Example

```jsx
class LoginForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      email: '',
      password: ''
    };
  }

  render() {
    return (
      <form>
        <input type="email" value={this.state.email} />
        <input type="password" value={this.state.password} />
      </form>
    );
  }
}
```

At this stage:

* Input cannot change yet
* React owns the value

---

## 3. Handling Input Changes

We update state using `onChange`.

```js
handleChange = (e) => {
  this.setState({
    [e.target.name]: e.target.value
  });
};
```

Updated form:

```jsx
<input
  type="email"
  name="email"
  value={this.state.email}
  onChange={this.handleChange}
/>
```

This pattern:

* Supports multiple inputs
* Keeps logic centralized

---

## 4. Submitting the Form

```js
handleSubmit = (e) => {
  e.preventDefault();
  console.log(this.state);
};
```

Attach it:

```jsx
<form onSubmit={this.handleSubmit}>
```

`preventDefault` stops the browser from reloading the page.

---

## 5. Basic Validation (Manual)

Validation usually runs on submit.

```js
handleSubmit = (e) => {
  e.preventDefault();

  const { email, password } = this.state;

  if (!email || !password) {
    this.setState({ error: 'All fields are required' });
    return;
  }

  this.setState({ error: null });
};
```

---

## 6. Showing Validation Errors

```jsx
{this.state.error && (
  <p style={{ color: 'red' }}>{this.state.error}</p>
)}
```

UI reacts directly to validation state.

---

## 7. Field-Level Validation

```js
handleChange = (e) => {
  const { name, value } = e.target;

  this.setState({ [name]: value }, () => {
    if (name === 'email' && !value.includes('@')) {
      this.setState({ emailError: 'Invalid email' });
    } else {
      this.setState({ emailError: null });
    }
  });
};
```

Used when immediate feedback is needed.

---

## 8. Controlled vs Uncontrolled Forms

### Controlled

* Value comes from state
* Predictable
* More code

### Uncontrolled

* Uses `ref`
* Less code
* Less control

Example (uncontrolled):

```js
this.inputRef = React.createRef();
```

---

## 9. Common Mistakes

* Forgetting `name` attribute
* Updating state without `setState`
* Over-validating on every keystroke
* Copying props into form state unnecessarily

---

## 10. Mental Mapping to Hooks

| Hooks                                  | Class Components          |
| -------------------------------------- | ------------------------- |
| `useState`                             | `this.state`              |
| `onChange={e => setX(e.target.value)}` | `handleChange + setState` |
| Validation state                       | Same pattern              |

---

## 11. Key Takeaways

* Forms are usually controlled
* State is the single source of truth
* Validation is just conditional state
* Keep validation logic simple

---

**Next Logical Topics:**

* Event handling in class components
* Controlled vs uncontrolled (deep dive)
* Refs in class components
* Error boundaries

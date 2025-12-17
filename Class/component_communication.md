# React Class Components – Component Communication

This document explains **component communication in React class components**, focusing on **Parent → Child** and **Child → Parent** data flow. This is the backbone of React architecture, not optional trivia.

---

## 1. Core Rule of React Communication

React follows **one-way data flow**:

* Data flows **down** via props
* Events flow **up** via callbacks

If this feels restrictive, that’s intentional. Predictability beats chaos.

---

## 2. Parent to Child Communication

### Passing Data via Props

The parent sends data to the child using props.

```jsx
class Parent extends React.Component {
  state = { message: 'Hello from Parent' };

  render() {
    return <Child message={this.state.message} />;
  }
}
```

Child component:

```jsx
class Child extends React.Component {
  render() {
    return <p>{this.props.message}</p>;
  }
}
```

Notes:

* Props are read-only in the child
* Child cannot change parent state directly

---

## 3. Passing Multiple Props

```jsx
<Child name="Alex" age={25} isAdmin={true} />
```

In child:

```js
const { name, age, isAdmin } = this.props;
```

Props are just function arguments in a fancier outfit.

---

## 4. Child to Parent Communication

Children communicate **upward** by calling a function passed from the parent.

---

### Step 1: Parent Defines a Callback

```jsx
class Parent extends React.Component {
  state = { count: 0 };

  increment = () => {
    this.setState((prev) => ({ count: prev.count + 1 }));
  };

  render() {
    return <Child onIncrement={this.increment} />;
  }
}
```

---

### Step 2: Child Calls the Callback

```jsx
class Child extends React.Component {
  handleClick = () => {
    this.props.onIncrement();
  };

  render() {
    return <button onClick={this.handleClick}>+</button>;
  }
}
```

What happened:

* Parent owns the state
* Child requests a change
* Parent decides what to do

This is React discipline in action.

---

## 5. Passing Data from Child to Parent

```jsx
class Parent extends React.Component {
  handleData = (value) => {
    console.log(value);
  };

  render() {
    return <Child sendData={this.handleData} />;
  }
}
```

Child:

```js
this.props.sendData('Hello Parent');
```

The child does not send state. It sends **intent**.

---

## 6. Lifting State Up

When two children need to share data:

* Move state to the closest common parent
* Pass data and callbacks down

```jsx
class Parent extends React.Component {
  state = { sharedValue: 0 };

  updateValue = (v) => this.setState({ sharedValue: v });

  render() {
    return (
      <>
        <ChildA value={this.state.sharedValue} />
        <ChildB onUpdate={this.updateValue} />
      </>
    );
  }
}
```

This avoids duplicated and unsynced state.

---

## 7. Props Drilling (The Annoyance)

Passing props through many layers:

```jsx
<A data={data}>
  <B data={data}>
    <C data={data} />
  </B>
</A>
```

Problems:

* Messy
* Hard to maintain

Solutions (conceptually):

* Context API
* State management libraries

---

## 8. Communication Anti-Patterns

Avoid:

* Mutating props
* Child updating parent state directly
* Copying props into state unnecessarily
* Two sources of truth

These always age badly.

---

## 9. Mental Mapping to Hooks

| Hooks           | Class Components |
| --------------- | ---------------- |
| Props arguments | `this.props`     |
| Callback props  | Same pattern     |
| Lifting state   | Same pattern     |

Communication rules do not change between paradigms.

---

## 10. Key Takeaways

* Parent → Child via props
* Child → Parent via callbacks
* State lives in one place
* Predictability beats cleverness

---

**Next Logical Topics:**

* Event handling in class components
* Refs and DOM access
* Context API (class-based usage)
* Error boundaries

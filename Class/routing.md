# React Class Components – Routing

This document covers **routing in React applications using class components**, typically done with **React Router**. Routing decides *which component shows up* based on the URL. Simple idea. Endless ways to mess it up.

---

## 1. What is Routing in React

Routing means:

* Mapping URLs to components
* Rendering different UI without full page reloads
* Letting the browser URL reflect app state

React itself does **not** handle routing. A library does.

---

## 2. React Router (The Usual Choice)

React Router provides:

* `<BrowserRouter>` – wraps the app
* `<Routes>` and `<Route>` – define paths
* Navigation helpers

Class components work perfectly fine with it, even if hooks get more attention.

---

## 3. Basic Router Setup

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

Routing is usually defined at the top level.

---

## 4. Routing with Class Components

Class components are rendered the same way.

```jsx
class Home extends React.Component {
  render() {
    return <h1>Home Page</h1>;
  }
}
```

No special handling needed just to render them.

---

## 5. Navigation Using `Link`

Never use `<a>` tags for internal navigation.

```jsx
import { Link } from 'react-router-dom';

class Navbar extends React.Component {
  render() {
    return (
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
    );
  }
}
```

`Link` prevents page reloads and keeps routing client-side.

---

## 6. Accessing Route Params (Class Components)

Hooks like `useParams` don’t work in classes. You use **props** instead.

```jsx
<Route path="/user/:id" element={<User />} />
```

```jsx
class User extends React.Component {
  render() {
    const { id } = this.props.params;
    return <p>User ID: {id}</p>;
  }
}
```

This requires a wrapper (shown next).

---

## 7. Creating a Wrapper for Router Props

```jsx
import { useParams, useNavigate } from 'react-router-dom';

function withRouter(Component) {
  return function (props) {
    const params = useParams();
    const navigate = useNavigate();
    return <Component {...props} params={params} navigate={navigate} />;
  };
}
```

Usage:

```js
export default withRouter(User);
```

Yes, it’s extra work. Classes asked for this.

---

## 8. Programmatic Navigation

```js
this.props.navigate('/login');
```

Used after:

* Form submit
* Auth checks
* Conditional redirects

---

## 9. Protected Routes (Basic Idea)

```jsx
<Route
  path="/dashboard"
  element={isAuth ? <Dashboard /> : <Navigate to="/login" />}
/>
```

Authorization logic lives outside the class.

---

## 10. Nested Routes (Concept)

```jsx
<Route path="/settings" element={<Settings />}>
  <Route path="profile" element={<Profile />} />
</Route>
```

Nested routes reflect UI hierarchy.

---

## 11. Common Routing Mistakes

Avoid:

* Using `<a>` for navigation
* Hardcoding URLs everywhere
* Mixing auth logic inside UI components
* Forgetting wrappers for class components

---

## 12. Mental Mapping to Hooks

| Hooks World     | Class Components      |
| --------------- | --------------------- |
| `useNavigate`   | `this.props.navigate` |
| `useParams`     | `this.props.params`   |
| Route rendering | Same                  |

Routing rules stay the same. Only access patterns differ.

---

## 13. Key Takeaways

* Routing is handled by React Router
* Class components need wrappers for router data
* Navigation should be declarative
* URLs are part of app state

---

**Next Logical Topics:**

* Context API (class-based)
* Error boundaries
* Performance optimization
* Higher-order components

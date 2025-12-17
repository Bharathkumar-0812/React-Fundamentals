# React Semantic UI – Core Components (Grid, Menu, Header, etc)

This document introduces **React Semantic UI** and its commonly used components such as **Grid, Menu, Header, Button, and Segment**. This is about building **clean, structured UI** without reinventing CSS every morning.

---

## 1. What is Semantic UI React

Semantic UI React is:

* A UI component library
* Based on **Semantic UI CSS**
* Focused on **human-readable class names**

Instead of styling everything manually, you compose UI using well-defined components.

---

## 2. Installation

```bash
npm install semantic-ui-react semantic-ui-css
```

Import CSS once at app entry:

```js
import 'semantic-ui-css/semantic.min.css';
```

Without this, everything looks broken and sad.

---

## 3. Core Layout Concept

Semantic UI emphasizes:

* Layout via **Grid**
* Structure via **Segments**
* Navigation via **Menu**

You assemble UI like Lego, not like a CSS endurance test.

---

## 4. Grid Component

The **Grid** system divides layout into **16 columns**.

### Basic Grid

```jsx
import { Grid } from 'semantic-ui-react';

class Layout extends React.Component {
  render() {
    return (
      <Grid>
        <Grid.Row>
          <Grid.Column width={8}>Left</Grid.Column>
          <Grid.Column width={8}>Right</Grid.Column>
        </Grid.Row>
      </Grid>
    );
  }
}
```

Key points:

* Total column width per row = 16
* Responsive by default

---

## 5. Responsive Grid

```jsx
<Grid.Column mobile={16} tablet={8} computer={4}>
  Content
</Grid.Column>
```

Breakpoints:

* `mobile`
* `tablet`
* `computer`

Responsive behavior without media-query gymnastics.

---

## 6. Menu Component

Menus handle navigation and actions.

```jsx
import { Menu } from 'semantic-ui-react';

class NavBar extends React.Component {
  state = { activeItem: 'home' };

  handleItemClick = (e, { name }) => {
    this.setState({ activeItem: name });
  };

  render() {
    const { activeItem } = this.state;

    return (
      <Menu pointing secondary>
        <Menu.Item
          name="home"
          active={activeItem === 'home'}
          onClick={this.handleItemClick}
        />
        <Menu.Item
          name="profile"
          active={activeItem === 'profile'}
          onClick={this.handleItemClick}
        />
      </Menu>
    );
  }
}
```

Menu state is usually local UI state.

---

## 7. Header Component

Used for titles and section headings.

```jsx
import { Header } from 'semantic-ui-react';

<Header as="h2">Dashboard</Header>
```

Variations:

* `as="h1"` to `h5`
* `subheader`
* `icon`

```jsx
<Header as="h2" icon>
  <Icon name="settings" />
  Settings
  <Header.Subheader>Manage preferences</Header.Subheader>
</Header>
```

---

## 8. Segment Component

Segments group content visually.

```jsx
import { Segment } from 'semantic-ui-react';

<Segment padded>
  Content goes here
</Segment>
```

Common props:

* `padded`
* `raised`
* `stacked`

---

## 9. Button Component

```jsx
import { Button } from 'semantic-ui-react';

<Button primary>Save</Button>
<Button secondary>Cancel</Button>
<Button loading>Loading</Button>
```

Button states communicate action clearly without custom CSS.

---

## 10. Icons

```jsx
import { Icon } from 'semantic-ui-react';

<Icon name="user" />
```

Icons integrate seamlessly with other components.

---

## 11. Form Components (Preview)

Semantic UI provides:

* `<Form>`
* `<Form.Input>`
* `<Form.Select>`

```jsx
<Form>
  <Form.Input label="Email" placeholder="Enter email" />
</Form>
```

These still follow controlled component rules.

---

## 12. Common Mistakes

Avoid:

* Mixing custom CSS aggressively with Semantic UI
* Over-nesting grids
* Ignoring responsiveness
* Styling instead of composing

Use components first. Style later.

---

## 13. When to Use Semantic UI

Good fit when:

* You want fast, clean UI
* Design consistency matters
* You don’t want to fight CSS daily

Not ideal when:

* You need highly custom design
* Bundle size is critical

---

## 14. Key Takeaways

* Semantic UI React accelerates UI development
* Grid controls layout
* Menu handles navigation state
* Components are composable and readable

---

**Next Logical Topics:**

* Forms and validation with Semantic UI
* Theming and customization
* Accessibility considerations
* Migrating to other UI libraries

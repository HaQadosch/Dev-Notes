# React

# Do's and Dont's

* never use the index as key or way to identify anything, it's always a bad idea. Rather use a proper id. The index of an element might change in a list, by push or unshift which changes the order of the elements in the list. Synchronisation between lists might not be the same between clients if you have a race condition. You _could_ always sort you list as well but that could end up being more work than just using proper id. 

# Pattern

The patterns are here to help resolve 3 common situations:
  1. Rendering Views
  2. Fetching Data
  3. Managing State

## Rendering Views

### Using HTML attributes in your props

Use `HTMLAttributes<HTMLInputElement>` and you can extend the types as well:
```typescript
import { HTMLAttributes } from "react"

type Theme = "light" | "dark"
interface IProps extends HTMLAttributes<HTMLInputElement> {
  // additional props if need
  theme: {
    variation: Theme
  }
}

// You might want to extract certain props and your custom props
// instead of just spreading all the props
// if you don't have additional props swap IProps for HTMLAttributes<HTMLInputElement>
const Input ({ theme, ...props }: IProps) => (
  <input
    {...props}
    className={`input input--${theme.variation}`}
  />
)

// Usage
<Input
  onChange={(e) => handle(e)}
  value={name} // hooks [name, setName]
  name="name"
  id="id"
  theme={{
    variation: "light"
  }}
/>
```

# Passing data around
## Prop drilling
## Context API
## Composition
https://youtu.be/3XaXKiXtNjw 
## Redux


### Passing a component as props

```javascript
import { ComponentType } from "react"
import { RouteProps } from "react-router-dom"

interface ICustomRoute extends RouteProps {
  // Allows you to pass in components and then render them
  component: ComponentType<any>
}

const CustomRoute = ({ component: Component, ...rest }: ICustomRoute) => (
  <Route
    {...rest}
    render={props => (<Component {...props} />)}
  />
)

```


## Fetching Data

```jsx
import React, { useEffect, useState } from 'react';

export function Posts() {
  const [posts, setPosts] = useState([]);

  function getData() {
    fetch('https://jsonplaceholder.typicode.com/posts').then(async (fetchedPosts) => {
      const postsAsJson = await fetchedPosts.json();
      setPosts(postsAsJson);
    });
  }

  useEffect(() => {
    getData();
    const pollForData = setInterval(() => getData(), 5000);
    return () => {
      clearTimeout(pollForData);
    };
  }, []);

  return (
    <div>
      <h4>Posts</h4>
      {posts.map(({ id, title, body }) => (
        <div key={id}>
          <h3>{title}</h3>
          <p>{body}</p>
        </div>
      ))}
    </div>
  );
}
```


## Managing State

We do this via context.

### context.provider

Barebone example:

First we have to create the React Context itself which gives you access to a `Provider` and a  `Consumer` component.
You can pass an initial value; it can be null too.
```js
// ThemeContext.js
import React from 'react'

export const ThemeContext = React.createContext()
```

Now the _component P_ would have to provide the context with the given Provider component. The value passed can be anything from fetched data,
component state ot props. The component P only displays component D and doesn't pass any props to it. 
```js
// ComponentP.js
import React from 'react'
import { ThemeContext } from './ThemeContext'

export const P = () => {
  <ThemeContext.Provider value={green}>
    <D />
  </ThemeContext.P>
}
```

What it does instead is to make the value **green** available to all its children and their children, ..., via a Consumer.

Component C is somewhere in the children tree of Component P. It derives its style by consuming the context.
```js
// ComponentC.js
import React from 'react'
import { ThemeContext } from './ThemeContext'

export const C = () => {
  <ThemeContext.Consumer>
    { value => (
      <p style={{ color: value }}> Hello World </p>
    )}
  </ThemeContext.Consumer>
}
```
---
### Context Hook: useContext

The context component is being created the same way:
```js
// ThemeContext.jsx
import React from 'react`

export const ThemeContext = createContext()
```

As is the Provider
```jsx
// ComponentP.jsx
import React from 'react'
import ThemeContext from './ThemeContext'

export const C = () => {
  <ThemeContext.Provider value="green">
    <D />
  </ThemeContext.Provider>
}
```

The Consumer is the component using the Hook to access the value passed by the Provider.
```jsx
// ComponentC.jsx
import React, { useContext } from 'react'
import ThemeContext from './ThemeContext'

export const C = () => {
  const color = useContext(ThemeContext)

  return (
    <p style={{ color }}>
      Yo.
    </p>
  )
}
```

---
Bigger expample using a Store.

```javascript
// State.tsx
import React, { createContext, useReducer } from "react";

let AppContext = createContext();

const initialState = {
  count: 0
}

let reducer = (state, action) => {
  switch(action.type) {
    case "SET_COUNT": {
      return { ...state, count: action.payload }
    }
  }
  return state;
};

function AppContextProvider (props) {
  const fullInitialState = {
    ...initialState,
  }

  let [state, dispatch] = useReducer(reducer, fullInitialState);
  let value = { state, dispatch };


  return (
    <AppContext.Provider value={value}>{props.children}</AppContext.Provider>
  );
}

let AppContextConsumer = AppContext.Consumer;

export { AppContext, AppContextProvider, AppContextConsumer };
```

### context.consumer

```javascript
import React, { useContext } from 'react';
import { IonButton } from '@ionic/react';
import { AppContext } from '../State';

export const MyComponent = () => {
  const { state, dispatch } = useContext(AppContext);

  return (
    <div>
      <IonButton onClick={() => dispatch({
        type: 'SET_COUNT',
        payload: state.count + 1
      })}>
        Add to Order
      </IonButton>
      <h2>You have {state.count} in your cart</h2>
    </div>
  )
}
```

### Render Props

Parent component, calls the Toggle component just to handle the states `on` and actions `toggle`
but decides what to render with `on` and `toggle`

```jsx
<Toggle onToggle={onToggle}>
  {({ on, toggle }) => (
    <div>
      {on ? 'The button is on' : 'The button is off'}
      <Switch on={on} onClick={toggle} />
      <hr />
      <button aria-label='custom-button' onClick={toggle}>
        {on ? 'on' : 'off'}
      </button>
    </div>
  )}
</Toggle>

// Handles the logic, then calls children, from the parent component.
const Toggle = ({ children, onToggle }) => {
  const [on, setOn] = useState(false)

  const handleClick = () => {
    const newOn = !on
    onToggle(newOn)
    setOn(newOn)
  }

  return children({ on, toggle: handleClick })
}
```

## Authentication


## Autosave

```javascript
const delay = (() => {
  let timer = 0;
  return (callback, ms) => {
    clearTimeout(timer);
    timer = setTimeout(callback, ms);
  };
})();

```

`delay` is called often, but each time it clears itself. It actually saves after 3s of non interruption.


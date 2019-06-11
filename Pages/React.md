# React

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

```javascript
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

## Authentication
## HOC

# Passing data around
## Prop drilling
## Redux
## Context API
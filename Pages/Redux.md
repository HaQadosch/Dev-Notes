# Redux

## Quick intro

``redux`` gives you a central place to put your states (data) for JS/TS apps. 
It's most often used with React (via `react-redux`).
This lets you access or change your state from **any** component in your tree.

* `redux` gives you a store, but it doesn't know anything about React.
* `react-redux` lets you connect pieces of the states to the React components.

![1](../assets/1.jpg "Store dictates the state of the app")
___
### Reducer
Your state lives in a central/global Redux store. 
That store is created with a function called a ```reducer```. 
A reducer takes in a `state` and an `action`, and returns the same or a **new** state.
At all times, the reducer should return a state:

* either the init state when called for the first time
* the current state if there is no change
* a new state otherwise
  
``` 
reducer: state, action => newState
```

![2](../assets/2.jpg "Creating the store via reducer and createStore")
___
### Provider
The store is given to your app using the `Provider` from react-redux. 
Use the provider to wrap the entire app, so that any component in your app can access the store.

![3](../assets/3.jpg "Store available via the context of the provider")
___
### Selector
To get data out of the store, use the `useSelector` hook from react-redux. `selector` is a fancy word for: _function that gets data out of the store_

![4](../assets/4.jpg "Accessing data from the store")
___
### Actions
Actions are plain JS objects. All actions should have a `type` key. They may also have additional keys (parameters), `payload` that are needed by the reducer to generate the new state.

![5](../assets/5.jpg "Actions have standard shape")
___
### Dispatch
Actions are not _called_, but are `dispatch`ed to the reducers. The action `type` is what tells the reducer what to do, i.e. return a new state or remain in the old one. `dispatch` is a built-in function available when you create the store.

![6](../assets/6.jpg "Writing in the store")
___
### Reducer, with a vengeance
To change data in the store (which probably change the state of your application), first write your `reducer`.
Reducers are often written with `switch/case` statements, but don't have to be. They just have to take in an `action` and a `state`, and return a new state.

![7](../assets/7.jpg "Shape of a reducer")
___
### Pure functions
It's important that reducers return a **new** state object (and not mutate the old one) so that your components will re-render when something changes.
Don't _set_ state values in reducers - only ever return a new state object with changed values.

![8](../assets/8.jpg "Don't mutate")
___
### Dispatch, with a vengeance
To dispatch an action, use the `useDispatch` hook from react-redux. 
Call useDispatch with an `action` object which will run through the reducers and will potentially change the state.

![9](../assets/9.jpg "Dispatching actions to generate a new state")
___
### Update
All the connected components (that call `useSelector`) will automatically get the new state. This is treated like props or state changing - `useSelector` will automatically detect changes and React will re-render the component.

## TL;DR

That's the basic, the core is always:

1.  Dispatch an action to the store
2.  Which may or may not change the state via reducers
3.  Get that state with a selector
4.  Changes will automatically re-render your app

# Immer and how to update the states from the store

[Immer](https://immerjs.github.io/immer/docs/introduction) is an easy way to handle immutability. 
It creates a copy/draft of the original object and you modify that draft the way you want without worrying about immutability.

https://daveceddia.com/react-redux-immutability-guide/
## Basic syntax
### Produce and draft
### Immer in a switch case
## Basic state manipulations
### Update an object
### Update an object in an object
### Update an object with a key
### Prepend an item to an array
### Add an item to an array
### Insert an item in the middle of an array
### Update an item in an array using an index
### Update an item in an array using map
### Update an object in an array
### Remove an item from an array with filter

# React and Context

a `Context` is a conduit/channel between the `Provider` and all the `Consumers`.

```tsx
// Source.tsx
export const Source = React.createContext()

// Outer.tsx
import { Source } from './Source'
import { Inner } from './Inner'

const Outer: React.FC<IOuterProps> = () => ( 
  <main>
    <Source.Provider value={usefulData}>
      <Inner />
    </Source.Provider>
  </main>
)

// Inner.tsx
import { Source } from './Source'

export const Inner: React.FC<IInnerProps> = () => (
  <Source.Consumer>
    {info => ( 
      <p>A useful piece of {info}</p>
    )}
  </Source.Consumer>
)
```


# Implementation with React

We will be using the [Redux Starter Kit](https://redux-starter-kit.js.org/) as it simplify the process and reduces the boilerplate in the simple cases.

## The Store

Let's make a simple on/off lightbulb. A button will toggle between the 2 states.

```tsx
import produce, { Draft } from 'immer';
import { configureStore, createSlice, EnhancedStore, AnyAction } from 'redux-starter-kit';

enum SwichState {
  Off,
  On,
}

interface ILight {
  readonly bulb: SwichState
}

const initialLightState: ILight = {bulb: SwichState.Off}


```

Making the store available across the whole application

```tsx
import { Provider } from 'react-redux'

const App: React.FC<IApp> = () => (
  <Provider store={store}>
    <MyFancyPony />
  </Provider>
)
```

Now that the store is availbale the components (MyFancyPony and its children) need to connect to the store.

# Testing the Store

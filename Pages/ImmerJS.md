# produce - simple form

```js
import produce from 'immer'

export const addGift = (state: IState, { id, description, image }: NewGift): IState => {
  return produce(state, draft => {
    draft.gifts.push({ id, description, image, reservedBy: undefined })
  })
}
```


# produce - curried form
```js
import produce, { Draft } from 'immer'


export const addGiftCur = produce((draft: Draft<IState>, { id, description, image }: NewGift) => {
  draft.gifts.push({ id, description, image, reservedBy: undefined })
})
```

# produce - as hook via useImmer

Works for both syncrhonous and asynchronous events.
```js
// lib.ts
export const getInitialState = (): IState => ({
  users: allUsers,
  currentUser: getCurrentUser() as IUser,
  gifts: defaultGifts as IGift[]
})
```

```js
// main.tsx
import React from 'react'
import { useImmer } from 'use-immer'

const App: React.FC = () => {
  const [{
    users,
    gifts,
    currentUser
  }, updateState] = useImmer<IState>(() => getInitialState())

  const handleAdd: React.MouseEventHandler<HTMLButtonElement> = () => {
    const description = prompt('Gift to Add')
    if (description) {
      updateState(draft => {
        draft.gifts.push({
          description,
          id: uuidv4(),
          image: `https://picsum.photos/200?q=${ Math.random() }`,
          reservedBy: undefined
        })
      })
    }
  }

  const handleBook: React.MouseEventHandler<HTMLButtonElement> = async () => {
    const isbn = prompt('Enter ISBN Number, like 0201558025')
    if (isbn) {
      const book: {
        identifiers: { isbn: string },
        title: string,
        cover: { medium: string }
      } = await getBookDetails(isbn)
      
      updateState(draft => {
        draft.gifts.push({
          id: book.identifiers.isbn,
          description: book.title,
          image: book.cover.medium,
          reservedBy: undefined
        })
      })
    }
  }

    return (
    <div className="App">
      <main>
        <aside>
          <button onClick={ handleAdd } >Add</button>
          <button onClick={ handleBook } >Add Book</button>
        </aside>
      </main>
    </div>
  );
}
```

# produce - wrapping a reducer
```js
// lib.ts
import produce, { Draft } from 'immer'

export const giftReducer = produce((draft: Draft<IState>, action) => {
  switch (action.type) {
    case "ADD_BOOK": {
      const { id, description, image } = action.payload
      if (!(id && description && image)) return
      draft.gifts.push({
        id,
        description,
        image,
        reservedBy: undefined
      })
    }
      break
    case "RESET":
      return getInitialState()
  }
})

export const getInitialState = (): IState => ({
  users: allUsers,
  currentUser: getCurrentUser() as IUser,
  gifts: defaultGifts as IGift[]
})
```

```js
// main.tsx
import React, { useCallback, useReducer } from 'react';
import logo from './logo.svg';
import './App.css';
import uuidv4 from 'uuid/v4'

import { getInitialState, IGift, getBookDetails, giftReducer, IUser } from './gifts'
import { Gift } from './Gift'

export const App: React.FC = () => {
  const [{ users, gifts, currentUser }, dispatch] = useReducer(giftReducer, getInitialState())

  const handleAdd: React.MouseEventHandler<HTMLButtonElement> = () => {
    const description = prompt('Gift to Add')
    if (description) {
      dispatch({
        type: "ADD_GIFT",
        payload: {
          description,
          id: uuidv4(),
          image: `https://picsum.photos/200?q=${ Math.random() }`,
        }
      })
    }
  }

  const handleReserve = useCallback((event: React.MouseEvent<HTMLButtonElement, MouseEvent>, id: IGift['id']) => {
    dispatch({
      type: "TOGGLE_RESERVATION",
      payload: { giftID: id }
    })
  }, [dispatch])

  const handleReset: React.MouseEventHandler<HTMLButtonElement> = () => {
    dispatch({
      type: "RESET"
    })
  }

  const handleBook: React.MouseEventHandler<HTMLButtonElement> = async () => {
    const isbn = prompt('Enter ISBN Number, like 0201558025')
    if (isbn) {
      const book: { identifiers: { isbn_10: string[] }, title: string, cover: { medium: string } } = await getBookDetails(isbn)
      dispatch({
        type: "ADD_BOOK",
        payload: {
          id: book.identifiers.isbn_10[0],
          description: book.title,
          image: book.cover.medium,
        }
      })
    }
  }

  return (
    <div className="App">
      <header className="App-header">
        <img src={ logo } className="App-logo" alt="logo" />
      </header>
      <main>
        { currentUser !== null ? (
          <>
            <h2>Hi, { currentUser.name } </h2>
            <aside>
              <button onClick={ handleAdd } >Add</button>
              <button onClick={ handleBook } >Add Book</button>
              <button onClick={ handleReset } >Reset</button>
            </aside>
            <ul>
              { (gifts as IGift[]).map((gift: IGift) => (
                <li key={ gift.id }><Gift key={ gift.description } onReserve={ handleReserve } gift={ gift as IGift } users={ users as IUser[] } currentUser={ currentUser } /></li>
              )) }
            </ul>
          </>
        ) : null }
      </main>
    </div>
  );
}
```

# Patches and InversPatches

```js
// lib.ts
import produce, { Draft, produceWithPatches } from 'immer'

const giftsRecipe = (draft: Draft<IState>, action: { type: string, payload?: any }) => {
  switch (action.type) {
    case "ADD_BOOK": {
      // Doing Stuff
    }
      break
    case "TOGGLE_RESERVATION": {
      // Doing Stuff
    }
      break
    case "ADD_GIFT": {
      // Doing Stuff
    }
      break
    case "RESET":
      // Stuff
  }
}

// curState, action => [newState, patches, inversePatches]
export const patchGeneratingGiftReducer = produceWithPatches(giftsRecipe)

```

# Compress patches

```js
import { produceWithPatches, applyPatches, Draft, Patch } from 'immer'

let history: Patch[] = []

function whenActionsHappen (patches) {
  history.push(...JSON.parse(patches))
}

function compressHistory (currentPatches: Patch[]) {
  const [, patches] = produceWithPatches(initialState, (draft: Draft<{ gifts: IGifts }>): { gifts: IGifts } => {
    return applyPatches(draft, currentPatches)
  })
  console.log(`compressed from ${ currentPatches.length } to ${ patches.length } patches.`)
  return patches
}

// We compress regularly.
setInterval(() => {
  history = compressHistory(history)
}, 5000)
```
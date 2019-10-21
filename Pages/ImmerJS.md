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
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


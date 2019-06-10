# Typescript

# Only allow specific keys on an object

``` typescript
type TListenerTypes = 'onload' | 'progress' | 'error';
type TListeners = Record<TListenerTypes, React.MouseEventHandler>;

// or with the signature of the event handler:

type TListeners = Record<TListenerTypes, ((e: Event) => void)>;
```



# DON'T
## Don't use any
If you happen not to know the exact type, you can use `unknown` type – which is similar to `any`, but it doesn't allow code to escape the TypeScript type system.
`unknown` is the type-safe counterpart of `any`.

## Don't use Function and Object types.
Specify function signatures explicitly, e.g. for event handlers:
``` typescript
(e: Event) => void
```

# React 

## useState
It's better to specify the type of your values
``` typescript
const [value, setValue] = useState<string>('init')
```

It's better to use `React.EventHandler<T>` type and its specializations like `MouseEventHandler`, `PointerEventHandler` and so on.
``` typescript
const handleChange: React.ChangeEventHandler<HTMLInputElement> = ({ target: { value: inputValue } }): void => {
  setValue(inputValue)
}
```

## Forms

``` typescript
const handleSubmit: React.FormEvent<HTMLFormEvent> = (): void => {

}

```

# links
https://basarat.gitbooks.io/typescript/content/docs/jsx/react.html

# Typescript

# Only allow specific keys on an object

``` typescript
type TListenerTypes = 'onload' | 'progress' | 'error';
type TListeners = Record<TListenerTypes, React.MouseEventHandler>;

// or with the signature of the event handler:

type TListeners = Record<TListenerTypes, ((e: Event) => void)>;
```



# DON'T

## Don't treat it as a linter.
  1. type your JS like any JS file
  2. declare some type on top
  3. repeat 2. until Typescript is happy

That's how you work with a linter. You end up with some generic definitions, too broad to be useful as you are missing on some implementation details and assistance from the TS engine.

You should try and use the closest definition possible, and write them as you write your JS code.

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
  * https://basarat.gitbooks.io/typescript/content/docs/jsx/react.html
  * https://thoughtbot.com/blog/javascript-type-systems-are-not-linters 
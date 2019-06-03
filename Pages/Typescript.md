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

It's better to use `React.EventHandler<T>` type and its specializations like `MouseEventHandler`, `PointerEventHandler` and so on.


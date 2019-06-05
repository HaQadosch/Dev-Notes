# CSS

## format

The order of the element should be like the below:
  1. Box (position, display, width, margin, etc.)
  2. Text
  3. Background
  4. Border
  5. Other (Alphabetically)

# Minimal setting
This setting is the minimal way to look good in all screens
``` css
main {
  max-width: 70ch;
  padding: 2ch;
  margin: auto;
}
```


# Centering Elt in the page

``` css
constainer {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
```

If the container is `main`
``` css
  margin: auto;

```
is all you need as `main` is a block element.

Within a grid cell
``` css
constainer {
  align-items: center;
  justify-content: center;
}
```


## Display: block

## Display: flex

## Display: grid

# Bleed an elt over another

# International
Begin/End instead of Left/Right


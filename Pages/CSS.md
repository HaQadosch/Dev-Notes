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

### Implicit vs Explicit
### Rows **and** Columns

# Bleed an elt over another

# International
Begin/End instead of Left/Right

# The future of CSS

* CSS grid
* Flexbox
* Alignment
* Writing modes
* Multiculomn
* Viewport Unit
* Transform
* Object Fit
* Clip-path
* Masking
* Shape-outside
* Initial-letter

# Graphic Design Principals

1. Overlap
2. The Viewport
   1. Play with above/below the fold
   2. Put with rythm, discovering the content one scroll at a time
3. Storyboards
   1. Plan a website, viewport shot by viewport shot
4. Responsive based on the 4 edges of the screen
   1. Grid rows and columns
   2. Alignment
   3. Viewport Units
5. Framing
   1. What comes in the frame, what comes out
   2. Webic language
6. White space
7. Verticality
   1. Graphic Design from vertical writing system
8. Flexibility
   1. min-content
   2. max-content
   3. fr unit
   4. minmax()
9. Questions are:
   1.  What happens when parts of the content/interface is missing
   2.  shorter/longer than ideal
10. Creativity
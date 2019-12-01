# CSS

## Reset

A modern reset by Andy Bell. [Original Page](https://hankchizljaw.com/wrote/a-modern-css-reset/)

```css
/* Box sizing rules */
*,
*::before,
*::after {
  box-sizing: border-box;
}

/* Remove default padding */
ul[class],
ol[class] {
  padding: 0;
}

/* Remove default margin */
body,
h1,
h2,
h3,
h4,
p,
ul[class],
ol[class],
li,
figure,
figcaption,
blockquote,
dl,
dd {
  margin: 0;
}

/* Set core body defaults */
body {
  min-height: 100vh;
  scroll-behavior: smooth;
  text-rendering: optimizeSpeed;
  line-height: 1.5;
}

/* Remove list styles on ul, ol elements with a class attribute */
ul[class],
ol[class] {
  list-style: none;
}

/* A elements that don't have a class get default styles */
a:not([class]) {
  text-decoration-skip-ink: auto;
}

/* Make images easier to work with */
img {
  max-width: 100%;
  display: block;
}

/* Natural flow and rhythm in articles by default */
article > * + * {
  margin-top: 1em;
}

/* Inherit fonts for inputs and buttons */
input,
button,
textarea,
select {
  font: inherit;
}

/* Remove all animations and transitions for people that prefer not to see them */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

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
## Centering an Image

```css
img {
  max-width: 100%;
  display: block;
  height: auto;
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
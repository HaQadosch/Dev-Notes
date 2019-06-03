# Tools and Boilerplate

# Setting up a project with React

> This is a series of recommendations. They’re more what you’d call guidelines than actual rules.

## Editor: VS-Code.

* A JavaScript editor firts and foremost.
* Lighter than an IDE.
* Plugins are numerous.
* It can do a lot: [vscodecandothat](https://vscodecandothat.com/)

## Toolkit/Framework: Create-react-app

* Created and maintained by the Facebook React team
* Default config is sensible, but easy to customise.
* Comes with a nice set of tools.

## Type checking: typescript

* Prevents bug (some, not all).
* Comes by default with CRA (create-react-app).
* More mature than Flow. TypL still in pre-alpha stage.
* Extensive @types libraries but by no means exhautive.

## Test: Jest

* Built-in with create-react-app.
* Built and maintain by the Facebook React team.

## Linter and formatter: Prettier

* Set up to format the code before comitting.
* The linter in your local machine is up to you as long as the code in git is prettified.

``` 
npm i create-react-app
npx create-react-app my-little-pony-app --typescript
```

## Design system: Storybook

  * Most used in modern framework communities
  * Tend to integrate all the features of the other design tools
  * Integrate well with jest for snapshot testing
  * Plugin for a11y

## Folder Structure:

```
 my-little-pony-app
 ↳ src
  ↳ components
    ↳ App.tsx
    ↳ App.css // The css files stay close to the component their attached to.
  ↳ __tests__
    ↳ App.test.tsx // The tests follow the convention by Jest.
  ↳ storybook
    ↳ App.story.tsx // Same for the stories.

```

## Pre-commit hook:

* Git hooks handled via husky
* Prettier triggered via pretty-quick

```
cd my-little-pony-app
npm i prettier pretty-quick husky -D
```

To prettify only the stagged files, add to the package.json of your cra project

``` JS
  "husky": {
    "hooks": {
      "pre-commit": "pretty-quick --staged"
    }
  },
```

##Prettier Config

* The style should be the same throughout the repo.
* Add the snippet below to the package.json file of your cra project

``` JS
  "prettier": {
    "printWidth": 120,
    "singleQuote": true,
    "jsxSingleQuote": true,
    "trailingComma": "all"
  },
```

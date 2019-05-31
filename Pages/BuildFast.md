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

## Folder Structure:

All the tests in the src/__test__ folder

All the components in the src/Components folder

## Pre-commit hook:

* Git hooks handled via husky
* Prettier triggered via pretty-quick

```
cd my-little-pony-app
npm i prettier pretty-quick husky -D
```

To prettify only the stagged files, add to the package.json of your cra project

```
  "husky": {
    "hooks": {
      "pre-commit": "pretty-quick --staged"
    }
  },
```

##Prettier Config

* The style should be the same throughout the repo.
* Add the snippet below to the package.json file of your cra project

```
  "prettier": {
    "printWidth": 120,
    "singleQuote": true,
    "jsxSingleQuote": true,
    "trailingComma": "all"
  },
```

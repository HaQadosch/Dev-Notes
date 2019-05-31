# Move fast with confidence

> Move fast and *don't* break things

# Use fast tools

Use easy framework/library/toolset that are ideal to iterate quickly.
  * Node
  * React (Svelte?)
  * GraphQL
  * Service Workers
  * Serveless deployment
  
Increasing the development iteration speed help grow the confidence of the dev team.
To do that, avoid specific knowledge (stay close to the standards) and manual process.

  1. Increase level of confidence and speed
  2. Automate more of the PR process
  3. Catch errors early, before they hit production

# Increase Level of confidence

## Never skip the basics
  * Test driven development
  * Linter
  * Autoformat
  * Static type checking

Everytime you fire a process that takes few minutes, you start doing something else.
If the process breaks, you need to dive into it again, and stop what you were doing in the meantime.
And when you fire the process again, it's pretty much back to square one with the second task you left hanging.
Switching contexts costs a lot of non productive time.

## Do as much as possible locally
  * Configure post-checkout hooks to install the node_module if it is missing
  * Same for post-merge and post-rebase hooks

``` JS
hooks: {
  'post-checkout': `if [[ $HUSKY_GIT_PARAMS =~ l$ ]]; then npm install; fi `; // Only call install if you co a branch.
  'post-merge': 'npm install';
  'post-rebase': 'npm t && npm lint-staged'
}
```

## Staging builds takes too long

### Break down your build and detect the culprits

``` 
node --inspect-brk node_modules/.bin/webpack --config configs/webpack.web.js
```
It helps detect slow tools. 

Maybe try rollup ?

### CSS modules
Remove css modules. CSS-in-JS allow for faster build time (need to be tested) .

### React Component Benchmarks

https://github.com/paularmstrong/react-component-benchmark

Looks deprecated for the past 2 years.

## Automate pull request reviews

### Run all the conformance checks
  * Have Jest, storybook, typescript and linter running while you type your code.
  * Install pre-commit hook to force the validations before sending to more time consuming pipeline.

### Have performance budgeting

Set up a hard limit on key metrics, like bundle size.
Only alert and warn when the metrics get reached. No need to overload with data to say all is good.
https://buildtracker.dev/

### Alert on errors

  * Have as close as possible 100% type coverage.
  * Sentry or equivalent. Pops message into slack.
  * React.strict
  * Error.boundaries

``` JS
class ErrorBoundary extends React.Component {
  componentDidCactch (error, info) {
    logErrorToMyReportService(error, extradata, info)
  } 
}
```


# The manual process you cannot remove
  * Manual review of PRs
  * Commit to staging
  * Merge to master
  * Push to prod


# Storybook

# Categorising Storybook Stories
Once you've added a few stories, you start wishing for a way to categorise these into relevant groupings.
``` javascript
// uncategorised
storiesOf("Button", module).add(...)

// categorised under "Form"
storiesOf("Form|Selectbox", module).add(...)
```

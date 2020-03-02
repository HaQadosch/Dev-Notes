

https://design-system.service.gov.uk/get-started/

# HTML 

  * Use semantic tags as much as possible:
    * it helps with accessibility
    * makes the css clearer
    * auto-documenting
    * browser features can be applied to specific tags

## Headings

* only one `<h1>` in the page.
* the headings must reflect the structure of the page
  * put a `<h3>` only inside the region of a `<h2>`
  * section does not create a separate region with an isolate heading structure
    * see https://html.spec.whatwg.org/multipage/sections.html#sectioning-root
    * https://css-tricks.com/how-to-section-your-html/
* the headings should not be used to style texts, use css for that



# Form

## feild

## Inputs

* input over select
* be specific with the type of input, it helps with a11y and validation
  * type number should only be used for incremental values. Gov.uk has [a lot to say](https://technology.blog.gov.uk/2020/02/24/why-the-gov-uk-design-system-team-changed-the-input-type-for-numbers/) about this.
  * make sure backend is doing validation as well!
* use <label> to explain what value is expected
* Sanitise your input? 
  * Do [contextual escaping](https://benhoyt.com/writings/dont-sanitize-do-escape/) instead.
  * Use industry recognised [DOMPurify](https://github.com/cure53/DOMPurify)
* link the label to the input
  * `for` attribute linked to the `id` of the input
  * or insert the input inside the `<label>`
  * or use aria-labelledby 
    ```html
    <h2 id="resetHeading">Reset Password</h2>
    <span id="resetUsername">Username</span>  
    <input type="text" name="username" aria-labelledby="resetHeading resetUsername">  
    ```

* If you don’t need to use a label because it is visually clear what the cta is, use a aria-label to help a11y
  * example for a search 
    ```html
      <input type="text" name="searchTerm" aria-label="Search">
      <button type="submit">Search</button>
    ```

# Button

## Click Handlers

* don’t insert a onClick event in a non interactive elt
  * a button is a button
  * a link is a link
  * a div is neither

# Links

* always provide an href
  * and always with a value 
    ```html
      <a href="#" onClick={this.upload}>Upload</a>  
    ```

# Live region

* notifications, error messages, status updates, pop ups, modals, … [what google says](https://developers.google.com/web/fundamentals/accessibility/semantics-aria/hiding-and-updating-content#aria-live)
  * Live region should be used to announce important information 
    ```html
      <div aria-live="polite" role="status">
        Your message has been sent
      </div>

      <div aria-live="assertive">
        Unable to place order as you're offline
      </div>   
    ```

# img

# HTML for email

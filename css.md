# CSS/SASS

## Table of Contents

1. [General](#general)
1. [Code Formatting](#code-formatting)
1. [Components](#components)
  1. [Modifiers](#modifiers)
  1. [Descedants](#descedants)
  1. [States](#states)
1. [Variables](#variables)
  1. [Naming Variables](#naming-variables)
1. [Webpack and Local Scopes](#webpack-and-local-scopes)

## General
  - We use a strict subset of [SASS](http://sass-lang.com/):
    - variables
    - mixins
  - The following naming conventions are inspired by [BEM](https://en.bem.info/method/definitions/) and [SUIT CSS](https://github.com/suitcss/suit/blob/master/doc/naming-conventions.md)

## Code Formatting
  - Indent with 4 spaces.
  - Add new line at end of files.

## Components
  Format:
  ```
  .c-componentName[--modifierName|__descendantName] {}
  ```

### Modifiers
  A modifier is a property fo a component that alters its look or behavior. The class should be included in the HTML *in addition* to the base component class.

  Example:
  ```css
  .c-button {}
  .c-button--primary {}
  ```
  
  ```html
  <button class="c-button c-button--primary">...</button>
  ```

### Descedants
  A component descendant is a class that is attached to a descendent node of a component. It's responsible for applying presentation directly to the descendant on behalf of a particular component.

  Example:
  ```css
  .c-profileCard {}
  .c-profileCard__title {}
  .c-profileCard__avatarImage {}
  ```

  ```html
  <div class="c-profileCard">
      <h3 class="c-profileCard__title">...</h3>
      <img class="c-profileCard__avatarImage" src="image.jpg" alt="Example" />
  </div>
  ```

### States
  Use `is-stateName` for state-based modifications of components. The state name must be Camel case. **Never style these classes directly; they should always be used as an adjoining class.**

  JS can add/remove these classes. This means that the same state names can be used in multiple contexts, but every component must define its own styles for the state (as they are scoped to the component).

  Format:
  ```
  .[is|has]-stateName {}
  ```

  Example:
  ```css
  .is-open {}
  .has-error {}
  ```

## Variables
  
### Naming Variables
  - Name your SASS variables modularly (generic to specific, left to right)

    ```
    // bad
    $border-color;
    $dark-border-color;
    $light-border-color;

    // good
    $color-border;
    $color-border-dark;
    $color-border-light;
    ```

  - Prefix your variables with the generic word they all have in common (e.g. color)
  > Prefixes make code hinting/completion easier.


    ```
    // bad
    $link;
    $text;
    $light-text;
    
    // good
    $color-link;
    $color-text;
    $color-text-light;
    ```

## Webpack and Local Scopes
  By default all CSS selectors exist within the same global scope. Every selector has the potential to have unintended side effects by targeting unwanted elements. And this can be very evil. But we can change that using Webpack's [css-loader#Local Scope](https://github.com/webpack/css-loader#local-scope)

  Read more: [The End of Global CSS](https://medium.com/seek-ui-engineering/the-end-of-global-css-90d2a4a06284)

### Naming with local scopes
  Camel case is recommended for local selectors and  they are easier to use in the importing JavaScript module (`styles.title` instead of `styles['.c-profileCard__title']`). Therefore, we are modifying slightly the above naming convention, if we use Webpack with local scopes.

  - **[Component root](#components)**
  > Use always `root`

    ```css
    .root {}
    ```

  - **[Modifiers](#modifiers)**
  > Use the prefix `m`

    Format: 
    ```
    .mModifierName {}
    ```

    Example:
    ```css
    .mPrimary {}
    .mLarge {}
    ```
    - **[Descedants](#descedants)**
    
    Format: 
    ```
    .descedantName {}
    ```

    Example:
    ```css
    .title {}
    .avatarImage {}
    ```

  - **[States](#states)**
    Format: 
    ```
    .[is|has]StateName {}
    ```

    Example:
    ```css
    .isOpen {}
    .hasError {}
    ```

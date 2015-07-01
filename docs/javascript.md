# JavaScript
This style guide is a list of *dos* and *don'ts* for JavaScript programs and is heavily inpired by the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) and the [Node.js Style Guide](https://github.com/felixge/node-style-guide)

## Table of Contents

1. [General](#general)
1. [Code Formatting](#code-formatting)
1. [Variable Declarations](#variable-declarations)
1. [Objects](#objects)
1. [Strings](#strings)
1. [Arrays](#arrays)
1. [Modules](#modules)
1. [Naming things](#naming-things)
1. [Documenting code](#documenting-code)
  1. [JSDoc](#jsdoc)

## General
  - **ES6**: Embrace ECMAScript 6 (also known as ECMAScript 2015). It's the next JavaScript standard. Some features:
    - Arrow functions
    - Classes
    - Template Strings
    - Modules
    - Generators
    - Promises
    - ...
  - **ES5**: Use transpilers like [Babel](https://babeljs.io/) to convert the ES6 code into pure ES5, so it can be understandable by all browsers.

## Code Formatting
  - Indent with 4 spaces.
  - Always use semicolons.
  - Add new line at end of files.

## Variable Declarations
  - Avoid using `var`.
  > `var` is to be considered legacy.

  - Use `const` for all of your declarations.
  > This ensures that you can't reassign your references (mutation), which can lead to bugs and difficult to comprehend code.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - If you must mutate references, use `let` instead of `var`.

  > `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    const name = "example";

    // good
    const name = 'example';
    ```

## Arrays

  - Arrays must be created using the Array literal.

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    const fruits = [];

    // bad
    fruits[fruits.length] = 'banana';

    // good
    fruits.push('apple');
    ```

## Modules

  - Always use ES6 modules (`import`, `export`) over a non-standard module system.
  
    ```javascript
    // bad
    const FancyModule = require('./FancyModule');
    module.exports = FancyModule.doSomething;
    
    // good
    import FancyModule from './FancyModule';
    export default FancyModule.doSomething;
    
    // better
    import { doSomething } from './FandyModule';
    export default doSomething;
    ```
  - Have a consistent module order:
    1. core / third-party modules
    2. own modules
  
    ```javascript
    // bad
    import MyModule from './MyModule';
    import express from 'express';
    import path from 'path';
    import { doSomething } from './FancyModule';
    
    //good
    import path from 'path';
    import express from 'express';
                                        // <-- add blank line
    import MyModule from './MyModule';
    import { doSomething } from './FancyModule';
    ```

## Naming things

  - **Quantity**: A variable/constant that stores a quantity always ends with `Count`.

    ```javascript
    let cpuCount = os.cpus().length;
    ```
  - **Descriptive conditions**: Any non-trivial conditions should be assigned to a descriptively named variable or function.
  
    ```javascript
    // bad
    if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
      // ...
    }
    
    // good
    var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

    if (isValidPassword) {
      // ...
    }
    ```

## Documenting code

  - Always use `//` unless it's a JSDoc declaration.

  - Insert always a space after `//`.

    ```javascript
    // bad
    //This is a comment

    // good
    // This is a comment
    ```

  - Don't use trailing `.` unless the comment contains multiple sentences.

  - Don't write down a developer name or other personal notes.

  - `TODO`, `FIXME`, `HACK` or other markers are not allowed. Open instead an issue, if you cannot fix it immediately.
  > Such markers tend to rot over time and nobody feels responsible to deal with it.

### JSDoc

  *work in progress*

  Typically we don't generate a [JSDoc](http://usejsdoc.org/) documentation when we're writing code just for internal use. But we want to make the life for JavaScript developers easier. Therefore, we use a subset of JSDoc to provide structured information about some code parts. Some IDEs will even use JSDoc to make code suggestions.

  - Primitive types start with a lowercase letter: number, string, boolean, null, undefinded

  - Object types start with a capital letter: Function, Object, Array, RegExp, MyCustomObject

  - `@param` and `@return` can be used without a description.

  - Use '@see' for further information.

  - Document errors that a function might throw with `@throw`

#### Function comments

  - A description must be provided, which starts with a sentence written in the third person singular.

    ```javascript
    /**
     * Generates a config file
     * @see http://example.org/additional-infos
     * @param {Object} options
     * @param {number} options.port Server port
     * @param {Function} cb Callback
     * @return {Object}
     * @throws Will throw an error if options is null.
     */
    function makeConfig(options, cb) {
        // ...
    }
    ```

# JavaScript

## Table of Contents

1. [General](#general)
1. [Code Formatting](#code-formatting)
1. [Variable Declarations](#variable-declarations)
1. [Objects](#objects)
1. [Strings](#strings)
1. [Arrays](#arrays)
1. [Naming things](#naming-things)

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

## Naming things

  - **Quantity**: A variable/constant that stores a quantity always ends with `Count`.

    ```javascript
    let cpuCount = os.cpus().length;
    ```
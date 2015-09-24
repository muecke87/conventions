# JavaScript
This style guide is a list of *dos* and *don'ts* for JavaScript programs and is heavily inpired by the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) and the [Node.js Style Guide](https://github.com/felixge/node-style-guide)

## Table of Contents

1. [General](#general)
1. [Code Formatting](#code-formatting)
1. [Variable Declarations](#variable-declarations)
1. [Objects](#objects)
1. [Strings](#strings)
1. [Arrays](#arrays)
1. [Functions](#functions)
  1. [Callback](#callback)
1. [Modules](#modules)
1. [Comparison Operators & Equality](#comparison-operators--equality)
1. [Logging & Error handling](#logging-and-error-handling)
1. [Naming things](#naming-things)
1. [Documenting code](#documenting-code)
  1. [JSDoc](#jsdoc)
1. [Resources](#resources)

## General
  - **ES6**: Embrace [ECMAScript 6](http://es6-features.org/) (also known as ECMAScript 2015). It's the next JavaScript standard. Some features:
    - Arrow functions
    - Classes
    - Template Strings
    - Modules
    - Promises
    - ...
  - **ES5**: Use transpilers like [Babel](https://babeljs.io/) to convert the ES6 code into pure ES5, so it can be understandable by all browsers.

## Code Formatting
  - Indent with 4 spaces.
  - Always use semicolons.
  - Add new line at end of files.
  - Clean up trailing whitespaces at end-of-line.

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

  - Add a space after the object key ´:´:

    ```javascript
    // bad
    const item = {
      title:'Example'
    }

    // good
    const item = {
      title: 'Example'
    }
    ```

  - Use object method shorthand.

    ```javascript
    // bad
    const atom = {
        value: 1,

        addValue: function (value) {
            return atom.value + value;
        }
    };

    // good
    const atom = {
        value: 1,

        addValue(value) {
            return atom.value + value;
        }
    };
    ```

  - Use property value shorthand.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
        lukeSkywalker,
    };
    ```

  - Group your shorthand properties at the beginning of your object declaration.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
        episodeOne: 1,
        twoJedisWalkIntoACantina: 2,
        lukeSkywalker,
        episodeThree: 3,
        mayTheFourth: 4,
        anakinSkywalker
    };

    // good
    const obj = {
        lukeSkywalker,
        anakinSkywalker,
        episodeOne: 1,
        twoJedisWalkIntoACantina: 2,
        episodeThree: 3,
        mayTheFourth: 4
    };
    ```

  - Use [object destructuring](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) when accessing and using multiple properties of an object.

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```


## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    const name = "example";

    // good
    const name = 'example';
    ```

  - When programmatically building up strings, use [template strings](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/template_strings) instead of concatenation.

    ```javascript
    // bad
    function sayHi(name) {
        return 'How are you, ' + name + '?';
    }

    // good
    function sayHi(name) {
        return `How are you, ${name}?`;
    }
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

## Functions

  - Use function declarations instead of function expressions.
  > Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. More about [function declarations vs. function expressions](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)

    ```javascript
    // bad
    const doSomething = function() {
    };

    // good
    function doSomething() {
    }
    ```

  - Use default parameter syntax rather than mutating function arguments.

    ```javascript
    // bad
    function handleThings(opts) {
        // No! We shouldn't mutate function arguments.
        // Double bad: if opts is falsy it'll be set to an object which may
        // be what you want but it can introduce subtle bugs.
        opts = opts || {};
        // ...
    }

    // good
    function handleThings(opts = {}) {
        // ...
    }
    ```

  - Always prefer arrow functions.

  ``` javascript
  // bad
  function doSomething(foo, bar, function(bla, cb) {
      // ...
  })

  // good
  function doSomething(foo, bar, (bla, cb) => {
      // ...
  })
  ```

  - Omit return statements as long as readability is not sacrificed.
  > Whole function body has to fit onto that one line (max. ~100 characters per line), except object literals or JSX expressions.

    ``` javascript
    // bad
    function doSomething(options, (foo) => {
        return {
            value1: foo.value1,
            value2: foo.value2
        }
    })

    // good
    function doSomething(options, (foo) => ({
        value1: foo.value1,
        value2: bar.value2
    }))
    ```

  > In these two examples, omitting the return statement doesn't make sense:

    ``` javascript
    // bad
    axios({
        // ...
        // In this case it's not easy to read:
    }).then((response) => response.data).catch((response) => {
        // ...
    });

    // good
    axios({
        // ...
        // Much better:
    }).then((response) => {
        return response.data;
    }).catch((response) => {
        // ...
    });
    ```

    ``` javascript
    // bad (nothing left to say)
    Passport.use(new LocalStrategy(strategyOptions, (req, username, password, callback) => validators['passport-local'].validateUserLogin(req, username, password, callback));

    // good
    Passport.use(new LocalStrategy(strategyOptions, (req, username, password, callback) => {
        return validators['passport-local'].validateUserLogin(req, username, password, callback);
    }));
    ```
### Callback

  - Name callback handlers always `callback` (not `cb`, `done`, `next`, etc.):
    ```javascript
    export default function fetchInitialData(context, routerState, callback) {
      ...
      concurrent(dataFetchers, callback);
    }
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

  - For imports which you have to go at maximum two directory levels higher for, use relative paths:

    ```javascript
    // modules/reader/components/SearchResultList/SearchResultList.js

    //bad
    import GuidelineStore from 'modules/reader/stores/GuidelineStore';
    import MainSearch from 'modules/reader/components/MainSearch';
    import SearchResultItem from 'modules/reader/components/SearchResultList/SearchResultItem';

    //good
    import GuidelineStore from '../../stores/GuidelineStore';
    import MainSearch from '../MainSearch';
    import SearchResultItem from './SearchResultItem';
    ```

  - For imports which you have to go at minimum three directory levels higher for (libraries), use absolute paths:

    ```javascript
    // modules/reader/pages/GuidelinePage.js

    //bad
    import Accordion from '../../../elements/Accordion';
    import fetchData from '../../../utils/higher-order/fetchData';

    //good
    import Accordion from 'elements/Accordion';
    import fetchData from 'utils/higher-order/fetchData';
    ```

  - Sort modules after their directory path:

    ```javascript
    import connectToStores from 'fluxible/addons/connectToStores';
    import FluxibleComponent from 'fluxible/addons/FluxibleComponent';
    import _ from 'lodash';
    import React from 'react';
    import Router from 'react-router';
    import BrowserHistory from 'react-router/lib/BrowserHistory';

    import Accordion from 'elements/Accordion';
    import Loader from 'elements/Loader';
    import PageHeader from 'elements/PageHeader';
    import fetchData from 'utils/higher-order/fetchData';
    import loadGuideline from '../actions/loadGuideline';
    import GuidelineStore from '../stores/GuidelineStore';
    import SearchResultItem from './SearchResultItem';
    ```

  - Add an index.js file to an exported module, which's parent folder has the same name as the module itself (prevents repetitions in import statements):

    ```javascript
    // app/elements/TextField/index.js
    import TextField from './TextField';
    export default TextField;
    ```

## Comparison Operators & Equality

  - Use `===` and `!==` over `==` and `!=`.

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
        // ...
    }

    // good
    if (name) {
        // ...
    }

    // bad
    if (collection.length > 0) {
        // ...
    }

    // good
    if (collection.length) {
        // ...
    }
    ```

  - Further reading: [Truth, Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/)

## Logging & Error handling

  - Use Utility app\utils\logger\

  Available log-functions are 'debug', 'warn' and 'logError'

  Dont use the exported logger-object directly! It's only exported so it can be used for unit tests.

  - Use it with Promises

  ```javascript
  import { logError } from 'utils/logger';
  ...

  return axios({
      data: method === 'get' ? undefined : data  
      ...
  }).then((response) => {
      return response.data;
  }).catch((response) => {
      logError(`callApi error ${response} ${method} ${url} ${data}`);
      return Promise.reject();
  });
  ```

  - If Promises are not available, then use it with CallBacks

  ```javascript
  Router.run(app.getComponent(), location, (error, initialState) => {
         fetchData(context, initialState, (err) => {
             if (err) {
                 logError(`fetchData error ${err}`);
             }
  ```

## Naming things

  - Use camelCase, never underscore.

  - Use PascalCase when naming constructors or classes.

  - Avoid using numbered variables (e.g. i1, i2, i3).

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

  Typically we don't generate a [JSDoc](http://usejsdoc.org/) documentation when we're writing code just for internal use. But we want to make the life for JavaScript developers easier. Therefore, we use a subset of JSDoc to provide structured information about some code parts. Some IDEs will even use JSDoc to make code suggestions.

#### When to write JSDoc

Everywhere, where an object/function is going further than just to be a simple implementation of an in the application widely used library/framework (e.g. React component, Flux store), has to be described with JSDoc.

#### Tags

Tag | Template | Description
------|--------------|----------------
@example (required) | `@example` | The @example tag allows you to provide a snippet of code that illustrates the usage of a constructor, a function (or method) or a variable.<br /><br />The @example tag is not intended to be used to generate "inline" examples, if you want this, you need to do it via HTML markup embedded within a @description block, using the <code> tag, for example.
@param (required) | `@param {Type} varname Description` | Used with method, function and constructor calls to document the arguments of a function.<br />Type names must be enclosed in curly braces. If the type is omitted, the compiler will not type-check the parameter.
@return (required) | `@return {Type} Description` | Used with method and function calls to document the return type. When writing descriptions for boolean parameters, prefer "Whether the component is visible" to "True if the component is visible, false otherwise". If there is no return value, use `@return {void}`.<br />Type names must be enclosed in curly braces. If the type is omitted, the compiler will not type-check the return value.
@see (optional) | `@see Link` | Reference a lookup to another class function or method.
@throws (required) | `@throws {exceptionType} exceptionDescription` | The @throws tag allows you to document the exception a function might throw.

#### Types

  - Primitive types start with a lowercase letter: `number`, `string`, `boolean`, `null`, `undefinded`

  - Object types start with a capital letter: `Function`, `Object`, `Array`, `RegExp`, `MyCustomObject`

#### Usage

##### Function comments

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


## Resources

**ES6**

  - [Learn ES2015](https://babeljs.io/docs/learn-es2015/)

**Functional programming**

  - [Don’t Be Scared Of Functional Programming](http://www.smashingmagazine.com/2014/07/02/dont-be-scared-of-functional-programming/)

**Debugging**

  - [memwatch: Keep an eye on your memory usage, and discover and isolate leaks](https://www.npmjs.com/package/memwatch)

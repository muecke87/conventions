# React + Flux

## Table of Contents

1. [Before you begin](#before-you-begin)
1. [General](#general)
1. [Components](#components)
  1. [Testing components](#testing-components)
1. [Actions](#actions)
  1. [Testing actions](#testing-actions)
1. [Stores](#stores)
  1. [Testing stores](#testing-stores)
1. [Resources](#resources)

## Before you begin

  - Learn the basics:
    - [Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html)
    - Flux is an application architecture (e.g. like MVC), not a framework or a library.
    - [Getting To Know Flux](https://scotch.io/tutorials/getting-to-know-flux-the-react-js-architecture)
  
## General

  - We use [Fluxible](http://fluxible.io/) to build our [isomorphic JavaScript](http://www.sitepoint.com/isomorphic-javascript-applications/) applications.

## Components

  - Always validate all your [prop types](https://facebook.github.io/react/docs/reusable-components.html#prop-validation).
  > They are a convenient way to document what props your component expects.

  ```javascript
  static propTypes = {
      type: React.PropTypes.oneOf(['text', 'password']).isRequired,
      label: React.PropTypes.string,
      placeholder: React.PropTypes.string,
      onChange: React.PropTypes.func
  }
  ```
  
  - Use the prop type `React.PropTypes.node` for `this.props.children`.

  - [Avoid state](https://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#what-components-should-have-state).

### Testing components

  - Use [shallow rendering](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) to instantiate a component. It's the [recommended way](https://discuss.reactjs.org/t/whats-the-prefered-way-to-test-react-js-components/26) to test a component.

  ```javascript
  import createShallowComponent from 'utils/test/createShallowComponent';
  
  // ...
  
  const textField = createShallowComponent(TextField, { label: 'My label' });
  const label = textField.props.children[0];
  const input = textField.props.children[1];
  expect(label.props.children).to.equal('My label');
  expect(label.type).to.equal('label');
  expect(input.type).to.equal('input');
  ```

  - [console.json](https://www.npmjs.com/package/console.json) might be useful to output the generated tree in a pretty format. Don't forget to remove this import before comitting your code.
  
  ```javascript
  import 'console.json';
  
  // ...
  
  console.json(textField);
  ```

  - Resources:
    - http://simonsmith.io/unit-testing-react-components-without-a-dom/
    - https://facebook.github.io/react/docs/test-utils.html
    - https://github.com/robertknight/react-testing/blob/master/tests/TweetList_test.js#L73
    - https://github.com/Granze/react-starterify/blob/master/test/components/mycomponent-test.js

## Actions

### Testing actions

  - Use Fluxibles `createMockActionContext`.
  
  ```javascript
  import createMockActionContext from 'fluxible/utils/createMockActionContext';
  ```
  
  - Always test:
    - `dispatchCalls.length`
    - `dispatchCalls[n].name`
    - `dispatchCalls[n].payload`
    - `executeActionCalls.length`
    - `executeActionCalls[n].action`
    - `executeActionCalls[n].payload`
  
  - Resources:
    - http://fluxible.io/api/actions.html#testing
    - https://github.com/yahoo/fluxible.io/tree/master/tests/unit/actions

## Stores

### Testing stores

  - Just instantiate your store. No special utilities required.

  ```javascript
  // ...
  
  describe('ExampleStore', () => {
      let store;
  
      const mockData = {
          title: 'An example'
      }
  
      beforeEach(() => {
          store = new ExampleStore();
      });
      
      it('should do something', () => {
          store.handleDocumentLoaded({ doc: mockData });
          expect(store.doc).to.be.an('object');
          expect(store.doc).to.equal(mockData);
          // ...
      })
  })
  ```

  - Resources:
    - https://github.com/yahoo/fluxible.io/tree/master/tests/unit/stores

## Resources
  
  - [react-primer-draft](https://github.com/mikechau/react-primer-draft)
  - [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)

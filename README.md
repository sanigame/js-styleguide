# Scale360 React/JSX Style Guide

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Class vs stateless](#class-vs-stateless)
  1. [Naming](#naming)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)

## Basic Rules

  - Only include one React component per file.
  - Always use JSX syntax.

## Class vs stateless

  - Class component

    ```jsx
    // good
    import React, { Component } from 'react'

    class Listing extends Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>
      }
    }

    export default Listing
    ```

    And if you don't have state or refs, prefer arrow functions over classes:

    ```jsx
    // bad
    import React, { Component } from 'react'
    class Listing extends Component {
      render() {
        return <div>{this.props.hello}</div>
      }
    }

    // good
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    )
    ```

## Naming

  - **Directory**: Use camelCase only.
  - **Extensions**: Use `.js` extension.
  - **Filename**: Use PascalCase for React component and camelCase for other js file.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances.
  - **Scene Naming** Add ```Scene``` text to last of file name.
  - **Container Naming** Add ```Container``` text to last of file name.
  - **Action naming** Add ```Action``` text to last of file name.
  - **Reducer naming** Add ```Reducer``` text to last of file name.

    ```jsx
    // bad
    import productCard from './ProductCard'

    // good
    import ProductCard from './ProductCard'

    // bad
    const ProductItem = <ProductCard />

    // good
    const productItem = <ProductCard />
    ```

  - **Component Naming**: Use the filename as the component name. For example, `Footer.js` should have a reference name of `Footer`. However, for root components of a directory, use `index.js` for export component file.

    ```jsx
    // bad
    import Footer from './footer/Footer'

    // bad
    import Footer from './footer/index'

    // good
    import Footer from './footer'
    ```
  - **Container Component**: Use for React component is connect redux. Add `Component` in filename and class name. Ex. `ProductListContainer.js`

    ```jsx

    // good
    import React, { Component } from 'react'
    class ProductListContainer extends Component {
      render() {
        ...
      }
    }
    ```

  - **Props Naming**: Avoid using DOM component prop names for different purposes.

    > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx
    // bad
    <MyComponent style="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

## Alignment

  - Follow these alignment styles for JSX syntax. ```freestyle```

    ```jsx
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz" />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz" >
      <Quux />
    </Foo>
    ```

## Quotes

  - Always use single quotes (`'`) for all other JS.

    ```jsx
    <Foo bar='bar' />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Always include a single space in your self-closing tag.

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

  - Do not pad JSX curly braces with spaces.

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## Props

  - Always use camelCase for prop names.

    ```jsx
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678} />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678} />
    ```

  - Omit the value of the prop when it is explicitly `true`.

    ```jsx
    // bad
    <Foo hidden={true} />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`.

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props.

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions).

    ```jsx
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - Do not use `accessKey` on elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - Avoid using an array index as `key` prop, prefer a unique ID. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // bad
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // good
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

  - Always define explicit defaultProps for all non-required props.

  > Why? propTypes are a form of documentation, and providing defaultProps means the reader of your code doesn’t have to assume as much. In addition, it can mean that your code can omit certain type checks.

  ```jsx
  // bad
  const SFC = ({ foo, bar, children }) => {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // good
  const SFC = ({ foo, bar, children }) => {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

## Refs

  - Always use ref callbacks.(https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // bad
    <Foo
      ref="myRef" />

    // good
    <Foo
      ref={(ref) => { this.myRef = ref; }} />
    ```
## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line.

    ```jsx
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```
## Tags

  - Always self-close tags that have no children.

    ```jsx
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a line.

    ```jsx
    // good
    <Foo
      bar="bar"
      baz="baz" />
    ```

## Ordering

  - Ordering for `class extends React.Component`:

  1. optional `static` methods
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - How to define `propTypes`, `defaultProps`, `contextTypes`, etc...

    ```jsx
    import React, { Component } from 'react';
    import PropTypes from 'prop-types';

    class Link extends Component {
      static propTypes = {
        id: PropTypes.number.isRequired,
        url: PropTypes.string.isRequired,
        text: PropTypes.string,
      }

      static defaultProps = {
        text: 'Hello World',
      }

      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    export default Link;
    ```

  

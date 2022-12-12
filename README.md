<img src="https://i.imgur.com/Yysn5IA.png">

# Intro to React Hooks
---

## Learning Objectives

| Students Will Be Able To: |
| --- |
| Explain the use case (what & why) of hooks |
| Use the `useState` hook to use/set state in function components |
| Use the `useEffect` hook to perform "side effects" in function components |

## Road Map

- Intro
- Set Up
- Review the Starter Code
- Why use React Hooks?
- What are Hooks?
- Using Hooks
- Rules of Using Hooks
- Essential Questions
- Custom Hooks

## Intro

[Hooks](https://reactjs.org/docs/hooks-intro.html) are an exciting addition to React versions 16.8+.

This lesson will cover the use case of hooks and demonstrate how to use the two most common React hooks.

## Set Up

First we'll open the remote branch containing code from the previous lesson. Then we can create a new branch to practice using hooks by refactoring one of the Class-based components. 

- Navigate to your `react-product-app` repo.
- Get the remote branch *component-lifecycle* with `$ git checkout component-lifecycle`
- Switch to a new local branch *hooks-code-along* with `git checkout -b hooks-code-along`
- Make sure all packages installed: `$ npm i`
- Open the project in VS Code: `$ code .`
- Open a terminal in VS Code (`ctrl + backtick`)
- Start the React Dev Server: `$ npm start`

## Why use React Hooks?

Fun fact: Hooks don't add any additional functionality to React applications.

So, if hooks don't bring any new features to React apps, one might wonder why they've been added to the library.

The short answer is that hooks enable functional components to use the capabilities of class components.

Capabilities such as the ability to:

- Store and update state.
- Run code in a way similar to using lifecycle methods from Component Class.

> IMPORTANT:  Hooks are backward-compatible and can be used side-by-side with classes and existing code so you can start using them gradually when you feel the urge to do so.

### So, what's wrong with class components?

There's nothing "wrong" with class components. However, consider that:

- Most developers find class components more complex to write and grasp than function components.
- With function components, there's no need to be concerned about the proper binding of `this` in event handlers, etc.
- Equivalent function components are more concise than their class counterparts.
- Functions aren't just easier on human developers, they're also easier for tooling and testing.

### Additional benefits of hooks

Just a few of the additional benefits hooks include:

- Hooks allow complex components to be split into smaller functions.
- The functionality in hooks are reusable across multiple components.
- Hooks allow related, stateful behavior to be kept together instead of being spread across multiple class methods such as `componentDidMount`, `componentDidUpdate` & `componentWillUnmount`.
- Custom hooks can be written and shared across projects and with the community.

## What are Hooks?

Specifically, a hook:

- Is a JavaScript function.
- Can only be used within **function** components.
- Allows function components to "remember" state and stateful behavior between renders.

### Built-in Hooks and their use cases:

| Hook | Use Case |
|---|---|
|[`useState()`](https://reactjs.org/docs/hooks-reference.html#usestate)|Used to implement class components' `this.state` and `setState()`.|
|[`useEffect()`](https://reactjs.org/docs/hooks-reference.html#useeffect)|Used to implement "side effects", e.g., fetching data, using timers, subscriptions, etc.<br>`useEffect()` implements the functionality of `componentDidMount`, `componentDidUpdate` & `componentWillUnmount` with a single hook!|
|[`useRef()`](https://reactjs.org/docs/hooks-reference.html#useref)|In addition to how refs are used to access DOM elements in class components, `useRef()` can be used more generally to "remember" any non-state data that needs to be persisted between renders similar to how we use instance properties in class components. |
|[`useReducer()`](https://reactjs.org/docs/hooks-reference.html#usereducer)|An alternative to `useState()` for when the state is more complex.  It uses a reducer function and "actions" to update state - similar to how Redux does (but not as comprehensive).|
| Other built-in hooks|[`useContext()`](https://reactjs.org/docs/hooks-reference.html#usecontext)<br>[`useMemo()`](https://reactjs.org/docs/hooks-reference.html#usememo)<br>[and other less common hooks here...](https://reactjs.org/docs/hooks-reference.html#useimperativehandle)|

## Using Hooks

We are going to refactor our existing product app from Class-based components into functional components using hooks for state management and react's component lifecycle events.

### 💪 Practice Refactoring ProductList.js

Jump into ProductList.js and comment out all the code inside the class. Change the class declaration into an arrow function expression. Set the parameter name to props.

In: `src/pages/ProductList.js`

```js
const ProductList = (props) => {
    // state = {
    //     products: [],
    //     maxDisplay: this.props.maxDisplay
    // }

    // componentDidMount() {
    //     this.fetchData()
    // }

    ...

    //      {productCards}
    //                     </Card.Group>
    //             </Segment>
    //             <Button onClick={this.loadMore} content="Load More" color='black' fluid/>
    //         </>
    //     )
    // }
}

export default ProductList
```
### Adding State Using `useState()`

We use the `useState()` hook to insert a key into state and get a function to add a value to that key.

`useState()` takes an optional inital value when invoked.

`useState()` returns an array with two elements, the **value** and a **setter function** used to update the value.

We know that we need to store all of the products from our API call into state to render our list. Let's get to it. 

1. Update the import from react to include the useState hook. (You can remove Component.) 
1. In the function add a useState call with an empty array as the default value setting the result to variables, products and setProducts.
1. Uncomment the **return** section of the **render** method. Remove any instances of this.state. 
1. Uncomment the productCards section. Remove any instances of this.state. 
1. Uncomment the setting of loadMore function but don't uncomment the body of the function yet. 


In: `src/pages/ProductList.js`

```js
// Update the import to include the useState hook; can rm Component
import { useState } from "react";
import { Link } from "react-router-dom";
import { Button, Card, Dimmer, Loader, Segment } from "semantic-ui-react";
import ProductCard from "../components/ProductCard";
import ProductModel from "../models/product";

const ProductList = (props) => {
    // state = {
    //     products: [],
    //     maxDisplay: this.props.maxDisplay
    // }
    const [products, setProducts] = useState([]) 
    // useState always returns an array of two elements
    // First element "games" is the value of the state returned
    // Second element "setGames" is a setter function for updating the state
    
    // componentDidMount() {
    //     this.fetchData()
    // }

    const loadMore = () => {
    //     console.log('button')
    //     let maxDisplay = this.state.maxDisplay + 3
    //     this.fetchData(maxDisplay)
    }

    // fetchData = (max=3) => {
        ...
    // }

    let productCards = products.map((product) => (
        <Link to={`/products/${product.id}`} key={product.id}>
            <ProductCard product={product} key={product.id}/>
        </Link>
    ))

    return (
        <>
            <h2>List of Products</h2>
            <h3>Found: {products[0]? products.length:  '...Searching'}</h3>
            <Segment style={{minHeight: "100px"}}>
                    <Dimmer active={!products.length}>
                        <Loader size='tiny' indeterminate>Getting Products</Loader>
                    </Dimmer>
                    <Card.Group centered stackable doubling itemsPerRow={3}>
                        {productCards}
                    </Card.Group>
            </Segment>
            <Button onClick={loadMore} content="Load More" color='black' fluid/>
        </>
    )
}

export default ProductList
```

> Key Point: The names of the variables that hold the state value and setter function are up to us. However, it's convention to name the setter function by prepending `set` to the name of the state variable as we did with `products` & `setProducts`.

#### Mini Lab
Your turn! Update loadMore to change the value of maxDisplay in state when the button is clicked.

1. Add new variables, maxDisplay and setMaxDisplay, by destrucuturing the returned values from a useState() call with a default value from props.maxDisplay.
1. In loadMore uncomment the console.log and add a new line to use setMaxDisplay to increase the state value of maXDisplay by 3.

*Open the react dev tools and select the ProductList component from the available components to see how the button updates the maxDisplay state!*

<details>
    <summary>Solution</summary>
    In: `src/pages/ProductList.js`

    ```js
        const loadMore = () => {
            console.log('button')
            setMaxDisplay( maxDisplay + 3 )
            // let display = maxDisplay + 3
            // this.fetchData(maxDisplay)
        }
    ```
</details>

### Performing Side Effects Using `useEffect()`

Side effects include performing tasks such as:

- Fetching data
- Using timers
- Manually updating the DOM
- Managing subscriptions

> Key Point: When performing a task in a class component that requires overriding `componentDidMount`, `componentDidUpdate`, and/or `componentWillUnmount`, an equivalent function component will require a `useEffect()` hook.  Amazingly though, a single `useEffect()` hook can replace those three lifecycle methods combined!

#### Adding the `useEffect()` Hook

Like `useState()` and other hooks, because they are functions, we just invoke them from the top-level of the function component:

1. Update the import from react to include the useEffect hook. 
1. In the ProductList function add a useEffect after the commented out componentDidMount(). Give it a callback that logs it was called. 

In: `src/pages/ProductList.js`

```js
// Update the import to include the useState AND useEffect hook; can rm Component
import { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import { Button, Card, Dimmer, Loader, Segment } from "semantic-ui-react";
import ProductCard from "../components/ProductCard";
import ProductModel from "../models/product";

const ProductList = (props) => {
    // state = {

   ...  

    // componentDidMount() {
    //     this.fetchData()
    // }

    // takes a callback function; REQUIRED first argument
    useEffect(() => {
        console.log('useEffect was called')
    })

    ...

}

export default ProductList
```


**By default, the effect's callback function is going be invoked after every render of the component.**

> Note: In class components, we would have to implement the `componentDidUpdate()` lifecycle method to run a side effect every render after the initial render.

You can test the above statement by clicking the button and updating state. 
As you can see the log will run with every update.

#### Preventing Side Effects from Running

In many cases, you will want to optimize the component so that side effects only run if:

- Certain data changes (typically a prop or state variable).
- After the initial render, but not after subsequent renders (as with `componentDidMount` in class components). 

The `useEffect()` hook provides for these scenarios by accepting an array as a second argument.

The array is designed to hold a list of dependencies, that is, a list of variables and/or object properties that causes the side effect to run only if at least one of the dependencies change their value.

Providing an empty array (`[]`), will result in the side effect only running after the **initial** render.  Let's check this out:

```js
  // Add the [] as a 2nd argument
  useEffect(() => {
    console.log('useEffect was called');
  }, []);
```

Clear the console and refresh. The "useEffect was called" message will be logged.  However, unlike without the `[]` arg, clicking the button will no longer run the side effect!

#### Time to fetch data from our database!

1. Uncomment fetchData function.
1. Change this.setState() to setProducts() and setMaxDisplay()
1. In loadMore remove setMaxDisplay(). Uncomment fetchData call updating argument to `maxDisplay + 3`.   


In: `src/pages/ProductList.js`

```js

    ...

    const loadMore = () => {
        console.log('button')
        // move setMaxDisplay to fetchData
        fetchData(maxDisplay + 3)
    }

    const fetchData = (max=3) => {
        ProductModel.all(max)
            .then((res) => {
                console.log(res)
                setProducts(res.data)
                setMaxDisplay(max)
            })
    }

    ...

```

Click that button and watch the product data fill the page! 

#### Leverage useEffect to fetch on component mount

To have our component load the products on mount we must call our fetchData function inside useEffect. 

In: `src/pages/ProductList.js`

```js
import { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import { Button, Card, Dimmer, Loader, Segment } from "semantic-ui-react";
import ProductCard from "../components/ProductCard";
import ProductModel from "../models/product";

const ProductList = (props) => {
    const [products, setProducts] = useState([]) 
    const [maxDisplay, setMaxDisplay] = useState(props.maxDisplay)
   
    // takes a callback function; REQUIRED first argument
    // no second arg = called every render
    // empty [] second arg = called once (componentDidMount)
    // array with vars = called when one of those values changes
    useEffect(() => {
        fetchData()
    }, [])

    const loadMore = () => {
        console.log('button')
        // move setMaxDisplay to fetchData
        fetchData(maxDisplay + 3)
    }

    const fetchData = (max=3) => {
        ProductModel.all(max)
            .then((res) => {
                console.log(res)
                setProducts(res.data)
                setMaxDisplay(max)
            })
    }

    let productCards = products.map((product) => (
        <Link to={`/products/${product.id}`} key={product.id}>
            <ProductCard product={product} key={product.id}/>
        </Link>
    ))

    return (
        <>
            <h2>List of Products</h2>
            <h3>Found: {products[0]? products.length:  '...Searching'}</h3>
            <Segment style={{minHeight: "100px"}}>
                    <Dimmer active={!products.length}>
                        <Loader size='tiny' indeterminate>Getting Products</Loader>
                    </Dimmer>
                    <Card.Group centered stackable doubling itemsPerRow={3}>
                        {productCards}
                    </Card.Group>
            </Segment>
            <Button onClick={loadMore} content="Load More" color='black' fluid/>
        </>
    )
}

export default ProductList
```

Now our page will fill with data on load! 

#### Cleaning Up Side Effects

Certain side effects need to "clean up" after themselves and others don't.

For example, a side effect that fetches data, has nothing to clean up - it fetches data, updates state with it, and that's it.

However, in the case of say, creating a timer, or using subscriptions, you'll want to clear that timer or unsubscribe when the component that created it no longer exists.

Cleaning up is a good development practice because it prevents memory leaks.

In a class component, we use the `componentWillUnmount()` lifecycle method to clean up side effects.

However, a `useEffect()` hook can also run a cleanup process by returning a "cleanup" function from the hook's function:

```js
  useEffect(() => {
      console.log('useEffect was called')
      fetchData()
      // Return a "cleanup" function
      return () => {
          console.log("runs on unmount")
      }
  },[])
```

Using that empty array as a second argument results in the cleanup function running once - when the component unmounts.

Here's a summary of when side effects and their cleanup functions run according to what is passed as a second argument:

|Second Arg| When **Effect** and **Cleanup** Functions Run|
|---|---|
|none|**Effect fn:** Runs after every render.<br>**Cleanup fn:** Runs just before then next render to cleanup the previous render's effects.|
|`[]`|**Effect fn:** Runs once after initial render (mounting).<br>**Cleanup fn:** Runs once before unmounting.|
|`[depVar, obj.depProp, ...]`|**Effect fn:** Runs once after initial render, then only when either `depVar`, `obj.depProp`, etc. changes.<br>**Cleanup fn:** Runs before next render if previous render ran the effect and also when unmounting.|

#### Summary

Both the functional version and Class version of ProductList are functionally equivalent.

Comparing the code may tempt you to write only function components in future projects 😊

## Rules of Using Hooks

Using hooks requires that we always follow these two rules:

- Only call hooks at the top-level of a function component. Calling hooks inside loops, conditions, or nested functions is not allowed.

- Only call hooks from within:
	- React function components (they don't work with class components)
	- Or, your own custom Hooks

## ❓ Essential Questions

1. **True or False: React hooks allow function components to use React features previously possible only in class components?**

2. **The ________ hook allows function components to manage state.**

3. **Give one example of when to use the `useEffect()` hook.**

## Additional Practice! 

Time to refactor ProductDetail.js to use hooks! 

1. Update ProductDetail.js into a functional component.
1. Use the hooks useState and useEffect to get the same functionality. 
1. Remember we had to hard-code the productId. The new react-router-dom only passes params via the hook, `useParams()`. See if you can use that new hook to get the actual productId identied in the routes config. 


<details>
  <summary>Solution</summary>

In: `src/pages/ProductDetail.js`

```js
import { useEffect, useState } from "react"
import { useParams } from "react-router-dom"
import ProductCard from "../components/ProductCard"
import ProductModel from "../models/product"

const ProductDetail = () => {
    // destructure the params obj and rename; assign the value for 
    // the key id to the var productId
    const {id:productId} = useParams()
    const [product, setProduct] = useState({})
    // state = {
    //     productId: 3,
    //     product: {}
    // }
    
    useEffect(() => {
        ProductModel.show(productId)
            .then((res) => {
                console.log(res)
                setProduct(res.data)
            })
    }, [productId])
    // componentDidMount() {
    // }

    // render() {
    return (
        <>
            <h2>Individual Product</h2>
            {(product.title)? (
                <ProductCard product={product} detail/>
                ) : (
                <div>loading...</div>
                )}

        </>
        )
    // }

}

export default ProductDetail
```
</details>


## Custom Hooks


## Creating our own useProducts hook.


### Using our custom hook.

### But wait! What about the GameShow?


## References

[React Docs - Introducing Hooks](https://reactjs.org/docs/hooks-intro.html)

[React Docs - Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)

[React Docs - Building Your Own Hooks](https://reactjs.org/docs/hooks-custom.html)

[Observer Design Pattern](https://en.wikipedia.org/wiki/Observer_pattern)

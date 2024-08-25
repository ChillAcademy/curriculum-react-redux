Redux and API: Intro
Redux and async data
As you already know, when working with remote data that is fetched from a server, we are dealing with async operations. But, as redux documentation explains:

"By itself, a Redux store doesn't know anything about async logic. It only knows how to synchronously dispatch actions, update the state by calling the root reducer function, and notify the UI that something has changed."

To be able to work with async data, you should learn about middleware and side effects.

In Redux Toolkit, middleware refers to a layer of code that sits between the dispatch action and the reducer. It allows you to modify or enhance the behavior of your store.

Middlewares are chainable, which means that you can have several middlewares that will be called one after the other (we will see in the next section below).

One of the foundations of Redux is that you should avoid side effects in reducers, but middlewares are different because they are designed to work with side effects in a safe way. Redux middlewares can do anything when they see a dispatched action: modify the action, delay the action, make an async call, etc.

Using the thunk middleware
const fetchTodosMiddleware = (storeAPI) => (next) => (action) => {
  if (action.type === "todos/fetchTodos") {
    // Make an API call to fetch todos from the server
    fetch("todos").then((todos) => {
      // Dispatch an action with the todos we received
      storeAPI.dispatch({ type: "todos/todosLoaded", payload: todos })
    })
  }
  
But, what is a "thunk"? In general programming, a thunk is a programming term that means a piece of code that does some delayed (asynchronous) work.

In Redux Toolkit, it is a higher-order function that allows for asynchronous actions and state updates in your Redux store. It's a way to handle side-effects such as API calls or other asynchronous actions in a Redux store, allowing you to delay or conditionally dispatch actions.

A middleware is just a function. You can create your middleware or use an existing one.
It has access to the store, storeAPI in the example.
It has access to the action.
You can do something based on that: if (action.type === 'todos/fetchTodos').
It has a next function. Since middlewares are chainable, once you have finished with the current one, you should call next(), to execute the next middleware in the chain, and you can pass arguments (in this case, we are passing again the action).
Thunks vs. middlewares
In Redux Toolkit, both thunks and middleware are used to handle side-effects and perform asynchronous operations in your Redux store, but they differ in terms of their purpose and how they are used.

A thunk refers to a higher-order function that returns a function that dispatches an action. A middleware refers to a layer of code that sits between the dispatched action and the reduction of the state.

How to work with side effects in Redux
import { createStore, applyMiddleware } from "redux"
import thunk from "redux-thunk"
import rootReducer from "./reducers"

const GET_CURRENT_USER = "GET_CURRENT_USER"
const GET_CURRENT_USER_SUCCESS = "GET_CURRENT_USER_SUCCESS"
const GET_CURRENT_USER_FAILURE = "GET_CURRENT_USER_FAILURE"

const getUser = () => {
  return (dispatch) => {
    dispatch({ type: GET_CURRENT_USER })
    return fetch("your-api-endpoint/users").then(
      (user) => dispatch({ type: GET_CURRENT_USER_SUCCESS, user }),
      (err) => dispatch({ type: GET_CURRENT_USER_FAILURE, err })
    )
  }
}
React Redux Thunk Middleware in Redux Toolkit

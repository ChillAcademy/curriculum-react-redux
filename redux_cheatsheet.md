Redux cheat sheet
Here is a handy list of terms you need to understand Redux core concepts.

Note: We are using Redux Toolkit as it's intended to be the standard way to write Redux logic.

Core concepts
Store
It is an object that holds the current application state.
It is created by using the Redux Toolkit configureStore function:
  import { configureStore } from '@reduxjs/toolkit'

  import rootReducer from './reducers'

  const store = configureStore({ reducer: rootReducer })
  
It is accessed using the following functions:
useSelector: returns current state.
useDispatch: triggers an action on the store in order to update the state.
See documentation
Slice
It is a part of the store, that holds feature-specific state (i.e. authentication, product cart, counter)
import { createSlice } from '@reduxjs/toolkit'

const initialState = { value: 0 }

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment(state) {
      state.value++
    },
    decrement(state) {
      state.value--
    },
    incrementByAmount(state, action) {
      state.value += action.payload
    },
  },
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions
export default counterSlice.reducer
It contains both state, as well as state modifiers (called reducers)
See documentation
Provider
A special component that connects the Redux store with a React application:
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
See documentation
useSelector
A special function that accepts the Redux store state (or part of the state) as an argument, and returns data that is based on that state:
import { useSelector } from 'react-redux'

function Counter() {
    const count = useSelector(state => state.value);
    
    return <div>The count is {count}!<div>
}
See documentation
useDispatch
A special function that accepts a reducer function. It is used to update the Redux store:
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'

function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
See documentation
Learn more
For further learning, check out the following from the official documentation:

Full Counter example
Redux Glossary

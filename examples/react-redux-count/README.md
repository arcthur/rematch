# Example using rematch with react and react-redux

#### To run
From the rematch directory...
```
yarn install
yarn run build
cd examples/react-redux-count/
yarn install
yarn start
```
Then go to http://localhost:3000

#### The example
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import { connect, Provider } from 'react-redux'
import { init, model, dispatch, select, getStore } from 'rematch-x'

// No need to specify a 'view' in init.
init()

// Create the model
model({
  name: 'count',
  state: 0,
  reducers: {
    increment: state => state + 1
  },
  selectors: {
    doubled: state => state * 2
  }
})

// Make a presentational component.
// It knows nothing about redux or rematch.
const App = ({ value, valueDoubled, handleClick }) => (
  <div>
    <div>The count is {value}</div>
    <div>The count doubled is {valueDoubled}</div>
    <button onClick={handleClick}>Increment</button>
  </div>
)

// Use react-redux's connect
const AppContainer = connect(state => ({
  value: state.count,
  valueDoubled : select.count.doubled(state),
  handleClick: () => dispatch.count.increment()
}))(App)

// Use react-redux's <Provider /> and pass it the store.
ReactDOM.render(
  <Provider store={getStore()}>
    <AppContainer />
  </Provider>,
  document.getElementById('root')
)

```


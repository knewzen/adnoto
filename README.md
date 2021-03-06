# adnoto
Ultra-light library to implement observer state management. i.e. lightweight replacement for when [you might not need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367) but still want to have an observer pattern implementation.


## Install

Either `yarn add adnoto` or `npm install adnoto --save`.

## Usage

```javascript
  // Add the library
  const adnoto = require('adnoto')

  // Define some unique constants
  const ADD_USER = Symbol('ADD_USER')
  const REMOVE_USER = Symbol('REMOVE_USER')

  // Define a reducer, the name of the function is the name of the state
  function user (state = Object.create(null), action = {}) {
    switch (action.type) {
        // When no type is set (i.e. initialization) return the default state
      default: return state

      case ADD_USER: return Object.assign(Object.create(null), action.data)
      case REMOVE_USER: return Object.create(null)
    }
  }

  // Create actions
  const actionAddUser = data => adnoto.dispatch({ type: ADD_USER, data })
  const actionRemoveUser = () => adnoto.dispatch({ type: REMOVE_USER })

  // Add the initial reducer(s)
  adnoto.initialReducers(user /*, ...more reducers */)

  // Subscribe a listener function that will be notified on state change
  adnoto.subscribe(state => {
    console.log('New state', state)
  })

  // Trigger an action
  actionAddUser({ username: 'someuser', password: 'much secret' })
  actionRemoveUser()
```

### subscribe(function)

Adds a listener function to receive state changes.

### initializeReducers({ function, ...function })

Initializes new reducers into the state. Requires a plain object with reducers.

### dispatch(object)

Dispatches an action request to change the state. It's recommended to use an object with a `type` property that defines the action. 

### sideEffect(function)

Adds a listener function that receives the dispatch action instead of the state results. Very handy if you want to do, well, side effects (e.g. async operations etcetera).

### select(function)

Provides functionality to select keys from the current state. The argument function is the current state object, and the return value of that function will be parsed back. 

### reset()

Removes _all_ reducers, state and listeners.

## The reducer function

In order to function correctly, reducer functions need a specific layout. Each reducer with the **same key**, will overwrite the previous reducer, there is no error handling here, as it's assumed this is the wished course of action.

Secondly, reducers need to return a new state object, in all cases when triggered. If the state is the same as the previous state, then that's fine, as long as they return a value.

Thirdly the arguments to the reducer function are as follows: `state`, `action`. Where `state` is the current state and `action` is the argument parsed into the `dispatch` function.

## Contributors 

- Joseph Callaars <joseph@callaa.rs>


## License

MIT




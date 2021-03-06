# Redux Promise Middleware

[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-green.svg)](https://conventionalcommits.org)

## Install

> npm install --save @tracktl/redux-promise-middleware

or

> yarn add @tracktl/redux-promise-middleware
## Usage

Create promise implementations:

```js
const API = Symbol('API');

const implementations = new Map([
  [
    API,
    action => {
      if (process.env.NODE_ENV !== 'production') {
        if (typeof action.endpoint !== 'string') {
          throw new Error('Expected endpoint to be a string.');
        }
      }

      return fetch(action.endpoint);
    },
  ]
]);
```

Add it as a middleware:

```js
import { createStore, applyMiddleware } from 'redux';
import createPromiseMiddleware from '@tracktl/redux-promise-middleware';

const store = createStore(
  reducer,
  applyMiddleware(
    createPromiseMiddleware(implementations)
  )
)

```

Dispatch an action

```js
store.dispatch({
  promise: API,
  endpoint: '/movies'
  types: ['movies/get:fetching', 'movies/get:success', 'movies/get:fail'],
  // optional metas
  metas: [null, null, null],
});
```



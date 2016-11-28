# Store

En las secciones anteriores definimos las [acciones](acciones.md), que representan los "cambios que ocurrieron", y los [reducers](reducers.md), que actualizan el estado de acuerdo a dichas acciones.

El **Store** es el objeto que une las acciones y los reducers. Las responsabilidades del **Store** son:

* Contener el estado de la aplicación;
* Permite el acceso al estado por medio de [`getState()`](../api/Store.md#getState);
* Permite la actualización del estado por medio de [`dispatch(action)`](../api/Store.md#dispatch);
* Registra `listeners` por medio de [`subscribe(listener)`](../api/Store.md#subscribe);
* Administra la cancelación de registros de `listeners` por medio de la función que regresa [`subscribe(listener)`](../api/Store.md#subscribe).

Es importante tener en cuenta que una aplicación Redux tendrá un único **Store**, que contendrá el estado de **toda** la aplicación. Cuando se necesite dividir la lógica que maneja los datos, se debe usar [composición de reducers](Reducers.md#splitting-reducers) en vez de múltiples **Stores**.

It's easy to create a store if you have a reducer. In the [previous section](Reducers.md), we used [`combineReducers()`](../api/combineReducers.md) to combine several reducers into one. We will now import it, and pass it to [`createStore()`](../api/createStore.md).

```js
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```

You may optionally specify the initial state as the second argument to [`createStore()`](../api/createStore.md). This is useful for hydrating the state of the client to match the state of a Redux application running on the server.

```js
let store = createStore(todoApp, window.STATE_FROM_SERVER)
```

## Dispatching Actions

Now that we have created a store, let's verify our program works! Even without any UI, we can already test the update logic.

```js
import { addTodo, toggleTodo, setVisibilityFilter, VisibilityFilters } from './actions'

// Log the initial state
console.log(store.getState())

// Every time the state changes, log it
// Note that subscribe() returns a function for unregistering the listener
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)

// Dispatch some actions
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// Stop listening to state updates
unsubscribe()
```

You can see how this causes the state held by the store to change:

<img src='http://i.imgur.com/zMMtoMz.png' width='70%'>

We specified the behavior of our app before we even started writing the UI. We won't do this in this tutorial, but at this point you can write tests for your reducers and action creators. You won't need to mock anything because they are just [pure](../introduction/ThreePrinciples.md#changes-are-made-with-pure-functions) functions. Call them, and make assertions on what they return.

## Source Code

#### `index.js`

```js
import { createStore } from 'redux'
import todoApp from './reducers'

let store = createStore(todoApp)
```

## Next Steps

Before creating a UI for our todo app, we will take a detour to see [how the data flows in a Redux application](DataFlow.md).

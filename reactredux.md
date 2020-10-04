# React + Redux
### What is Redux?
__Redux__ is a state management framework that you can use to simplify the management of your application's state.

In a React Redux app, you create a single Redux store that manages the state of your entire app.
Your React __components subscribe to only the pieces of data in the store that are relevant to their role__.
Then, you __dispatch actions__ directly from React components, which then trigger store updates.

In Redux, there is a single state object that's responsible for the entire state of your application. This means if you had a React app with ten components, and each component had its own local state, the entire state of your app would be defined by a single state object housed in the Redux store. This is the first important principle to understand when learning Redux: the Redux store is the single source of truth when it comes to application state.

This also means that any time any piece of your app wants to update state, it must do so through the Redux store.

### Why use Redux?
Although React components can manage their own state locally, when you have a complex app,
it's generally better to keep the app state in a single location with Redux.
There are exceptions when individual components may have local state specific only to them. 

### Redux terms
* `store` object: holds and manages application state
* `store.createStore()`: create the Redux store; Takes a reducer function as a required argument
* __reducer function__:  takes state as an argument and returns state

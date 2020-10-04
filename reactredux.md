# React + Redux
### What is Redux?
__Redux__ is a state management framework that you can use to simplify the management of your application's state.

In a React Redux app, you create a single Redux store that manages the state of your entire app.
Your React __components subscribe to only the pieces of data in the store that are relevant to their role__.
Then, you __dispatch actions__ directly from React components, which then trigger store updates.

### Why use Redux?
Although React components can manage their own state locally, when you have a complex app,
it's generally better to keep the app state in a single location with Redux.
There are exceptions when individual components may have local state specific only to them. 

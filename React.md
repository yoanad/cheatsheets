# React Cheatsheet

## Hooks

### `useEffect`
* *componentDidMount*
- run once when component is mounted
```js
useEffect(() => {
  // Your code here
}, []);
```

```js
useEffect(() => {
  // Without second parameter, useEffect will be called at every render
}); 
```

* *componentWillUnmount* - used for cleanup (like removing event listeners, cancel the timer etc). Say you are adding a event listener in componentDidMount and removing it in componentWillUnmount as below.

```js
componentDidMount() {
  window.addEventListener('mousemove', () => {})
}

componentWillUnmount() {
  window.removeEventListener('mousemove', () => {})
}
```

Hook equivalent of above code will be as follows:

```js
useEffect(() => {
  window.addEventListener('mousemove', () => {});

  // returned function will be called on component unmount 
  return () => {
    window.removeEventListener('mousemove', () => {})
  }
}, [])
```
credit: https://stackoverflow.com/questions/53464595/how-to-use-componentwillmount-in-react-hooks 

# React Cheatsheet

## React Fragment


## React Class

```js
class DisplayMessages extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      input: '',
      messages: []
    }
  }
  render() {
    return <div />
  }
};
```

## React + API call
1. User visits webpage
2. The component is mounted to the DOM (webpage)
3. The componentDidMount or useEffect method is called sending off the HTTP request
4. The webpage gives an indication it is loading data
5. The data is received from the external API and added to state (side effect)
6. The component renders with the data that was fetched.

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


### `useCallback`
* the hook returns the same function instance between renderings
```js
function MyComponent() {
  // handleClick is the same function object
  const handleClick = useCallback(() => {
    console.log('Clicked!');
  }, []);

  // ...
}
```
#### Use case
E.g. when we have a component which is wrapped with React.memo() (e.g. a list of items that we don't want to re-render all the time, because it's expensive)
```js
import React from 'react';
import useSearch from './fetch-items';

function MyBigList({ term, handleClick }) {
  const items = useSearch(term);

  const itemToElement = item => <div onClick={handleClick}>{item}</div>;

  return <div>{items.map(itemToElement)}</div>;
}

export default React.memo(MyBigList);
```

```js
export default function MyParent({ term }) {
  const handleClick = useCallback(item => {
    console.log('You clicked ', item);
  }, [term]);

  return (
    <MyBigList
      term={term}
      handleClick={handleClick}
    />
  );
}
```
* handleClick callback is memoizied by useCallback(). As long as term variable stays the same, useCallback() returns the same function instance.

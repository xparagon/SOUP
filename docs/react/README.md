# React Notes

- [ReactJS](https://reactjs.org/)
- [hooks-reference](https://reactjs.org/docs/hooks-reference.html)
- [Create React App](https://create-react-app.dev/)

- [axios](https://axios-http.com/)

## Components
``` ts
import React from 'react';

interface ComponentProps {
    name: string,
    count: number,
}
function ComponentName({name, count}: ComponentProps) {

    return (
        <main />
    );
}
export default ComponentName;
```

You may now use this as a \<ComponentName name={nameVar} count={+0} /\><br />
Look under [Event bubbling](#event-bubbling) for how to add events to the Props.

*More about [components and props](https://reactjs.org/docs/components-and-props.html)*


## Hooks
``` ts
// states
const [count, setCount] = useState(0);
// State variables can hold objects and arrays

// effect
useEffect(() => {
    doUpdate();
    return () => {
        doCleanup();
    }
}, [dependentOn]);

// generate a unique ID
const id = useId();

```
The built-in Hooks:

useState
useEffect
useContext<br/>
useReducer
useCallback
useMemo
useRef
useImperativeHandle
useLayoutEffect
useDebugValue
useDeferredValue
useTransition
useId<br />
useSyncExternalStore
useInsertionEffect

*See more hooks under ['Hooks libs'](#hooks-libs)*


### Callback
Read this about [callbacks-refs](https://tkdodo.eu/blog/avoiding-use-effect-with-callback-refs)
``` ts
function MeasureExample() {
  const [height, setHeight] = React.useState(0)

  const measuredRef = React.useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height)
    }
  }, [])

  return (
    <>
      <h1 ref={measuredRef}>Hello, world</h1>
      <h2>The above header is {Math.round(height)}px tall</h2>
    </>
  )
}
```

### Memoized
``` ts
// callback
const memoizedCallback = useCallback(
    () => {
        doSomething(a, b);
    },
    [a, b],
);

// memo
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

```
See 'Custom Hooks' below for an example of useCallback.<br />
Only recompute the memoized value when one of the dependencies has changed.<br/>
This optimization helps to avoid expensive calculations on every render.

### Custom
A custom Hook is a JavaScript function whose name starts with ”use” and that may call other Hooks.<br />
(This is the first hook from https://usehooks.com/)
``` ts
import { useCallback, useState } from 'react';
// Usage
function App() {
    // Call the hook which returns, current value and the toggler function
    const [isTextChanged, setIsTextChanged] = useToggle();
    
    return (
        <button onClick={setIsTextChanged}>{isTextChanged ? 'Toggled' : 'Click to Toggle'}</button>
    );
}
// Hook
// Parameter is the boolean, with default "false" value
const useToggle = (initialState = false) => {
    // Initialize the state
    const [state, setState] = useState(initialState);
    
    // Define and memorize toggler function in case we pass down the component,
    // This function change the boolean value to it's opposite value
    const toggle = useCallback(() => setState(state => !state), []);
    
    return [state, toggle]
}
```

### Hooks libs
- https://usehooks.com/
- https://github.com/rehooks/awesome-react-hooks 
- https://react-hooks.org/

## Event bubbling

``` ts
// props for component    
export interface Props {
    onSelect(id: number): void;
}

function BubblingComponentName({onSelect}: Props) { ... }

// function inside component calling onSelect
const handleChange = (event: React.ChangeEvent<{ value: string }>) => {
  onSelect(+(event.target.value)); 
};

// trigger the change
<input 
  type='number'
  onChange={((e: React.ChangeEvent<{ value: string }>) => handleChange(e))}     
/>

``` 
See more about events here:
- [events](https://reactjs.org/docs/events.html)
- [handling-events](https://reactjs.org/docs/handling-events.html)

## Input validation
Given this HTML
``` html
<div>
  <label>Input: </label>
  <input type="number" min={0} max={100} value={num} onChange={handleNumInput} />
  <label> Output: </label>&rarr;&nbsp;{num}
</div
```
Then this is some code to handle the input:

``` ts
  const [num, setNum] = useState(0);
  /**
   * NOTE:
   * When used by <input type="number" min={0} max={100} ... />
   * You can not enter values outside of the range nor use the up/down icons
   * to add a number outside of the range.
   * BUT you could PASTE an invalid value.
   * The validation in this handle code disables that misbehavior.
   */
  function handleNumInput(event: React.ChangeEvent<{ value: string }>) {
    const value = (event.target.value);
    if (value) {
      // validate
      const v = Number.parseInt(value);
      if (v<0 || v>100) {
        console.error('value outside of range :', value);
      } else {
        setNum(v);
      }
    }
  }
```


## Handle api calls
A button's _handle_ call could be:
``` ts
  const [prodNo, setProdNo] = useState(0); // 0 is no prodNo given
  const [prodRating, setProdRating] = useState(-1); // -1 is error or no rating

  async function handleProdRating() {
    setLoading(true);
    const result = await fetchProdRating(prodNo);
    setLoading(false);
    setProdRating(result);
  }
```
When the api to call the endpoint returns a Promise
``` ts
  function fetchProdRating(prodNo: number): Promise<number> {
    return new Promise(resolve => {
      fetch('https://dummyjson.com/products/'+prodNo)
        .then(res => {
          if (res.ok) {
            return res.json();
          } else {
            throw new Error(res.statusText);
          }
        })
        .then(data => {
          console.log( data.rating);
          resolve(data.rating);
        })
        .catch(error => {
          console.error(error);
          resolve(-1);
        });
    });
  }
```
Note that the return should include information about an error. 
In this case it is just indicated with a -1.

Also, note that fetch has two 'then' step, so you have pass on or throw and catch an error!


# ToDo

I plan to document this:

- [mui](https://mui.com/), [eds](https://eds.equinor.com/)
- [styled-components](https://www.npmjs.com/package/styled-components)
- slots like in Vue, see [composition](https://reactjs.org/docs/composition-vs-inheritance.html) - using children: ReactNode;
- async/await, Promise
- [storybook](https://storybook.js.org/), [styleguideist](https://react-styleguidist.js.org/)
- web workers
- webpack, vite
- micro frontend

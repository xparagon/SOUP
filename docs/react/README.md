# React Notes

- [ReactJS](https://reactjs.org/)
- [hooks-reference](https://reactjs.org/docs/hooks-reference.html)
- [Create React App](https://create-react-app.dev/)

- [axios](https://axios-http.com/)

## Component template
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

# Plans to document

- slots like in Vue
- async/await
- service worker

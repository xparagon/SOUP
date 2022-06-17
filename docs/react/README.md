# React Notes

- https://reactjs.org/
- https://reactjs.org/docs/hooks-reference.html

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

You may now use this as a \<ComponentName name={nameVar} count={+0} /\>


## Hooks most used
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
## Mmemoized callback.
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
// Only recompute the memoized value when one of the dependencies has changed. 
// This optimization helps to avoid expensive calculations on every render.

```

## Custom Hooks
A custom Hook is a JavaScript function whose name starts with ”use” and that may call other Hooks.
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

**Nice hooks:**
- https://usehooks.com/
- https://github.com/rehooks/awesome-react-hooks 
- https://react-hooks.org/

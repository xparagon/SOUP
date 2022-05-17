# React Notes

https://reactjs.org/

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
```

## Hooks most used
``` ts
// states
const [count, setCount] = useState(0);

// effect
useEffect(() => {
    doUpdate();
    return () => {
        doCleanup();
    }
}, [dependentOn]);

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


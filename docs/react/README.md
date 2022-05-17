# React Developer Notes

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

## Hooks
``` ts
// state
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

const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

```


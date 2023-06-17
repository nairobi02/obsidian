# ![[react.png]] useMemo

`useMemo` is a React Hook that lets you cache the result of a calculation between re-renders.

```js
import { useState, useMemo } from "react";
import "./styles.css";

// A function that simulates a time-consuming operation
function funcDouble(num) {
  for (let i = 0; i < 999_999_999; i++) {}
  return num * 2;
}

export default function App() {
  const [number, setNumber] = useState(0);
  const [dark, setDark] = useState(false);

  // Using useMemo to memoize the result of the funcDouble function
  const doubleNumber = useMemo(() => {
    return funcDouble(number);
  }, [number]);

  const theme = {
    backgroundColor: dark ? "#333" : "#FFF",
    color: dark ? "#FFF" : "#333"
  };

  return (
    <div className="App" style={theme}>
      <input value={number} onChange={(e) => setNumber(e.target.value)} />
      <button onClick={(e) => setDark((prev) => !prev)}>Toggle theme</button>
      <div>{doubleNumber}</div>
    </div>
  );
}

```

### Why is `useMemo` necessary?

- The `useMemo` hook is used here to optimize performance by memoizing the result of the `funcDouble` function.
- Since `funcDouble` is a time-consuming operation, it's not ideal to call it on every render when `number` changes. Memoization allows us to store the calculated result and only recalculate it when the dependency (`number`) changes.
- By using `useMemo`, the expensive calculation is performed only when necessary, avoiding unnecessary calculations and improving the responsiveness of the application.

### How does it work?

- The `useMemo` hook takes two arguments: a function representing the calculation to be cached and an array of dependencies.
- The function is executed initially and whenever the dependencies in the array change.
- If the dependencies haven't changed, `useMemo` returns the previously cached value, avoiding recalculation and improving performance.
- In this case, the `useMemo` hook ensures that `funcDouble(number)` is recalculated only when the `number` state changes.

### Caveats and considerations:

- Memoization is most effective for expensive calculations or operations that don't depend on frequently changing data. Be cautious when applying memoization, as it can add complexity to the code and may not always provide a significant performance improvement.
- Make sure to include all relevant dependencies in the dependency array to trigger recalculation when necessary. Omitting dependencies can lead to stale data or incorrect behavior.
- Only use memoization when there is a measurable performance improvement. In some cases, the overhead of memoization can outweigh the benefits, especially for simple or fast computations.


## Referential equality

If we add this code in the above scenario 
```js
  const theme = {
    backgroundColor: dark ? "#333" : "#FFF",
    color: dark ? "#FFF" : "#333"
  };
  useEffect(()=>{console.log("Hlelo")},[theme])
```

every time we change our number state, the component will be re rendered. this causes the theme variable to be re recreated. and every time the theme object is re-created, the useEffect will run because the theme variable is essentially changing. since objects are compared by reference [[reference vs value]]

So that means, in our original code, the theme object is being re-created on every re-render, and the old theme object is being garbage collected. To avoid this, we can memoize the theme object, so that if none of its dependencies (dark) changes, then it won't be re-created. 

```js
import { useState, useMemo, useEffect } from "react";
import "./styles.css";

function funcDouble(num) {
  for (let i = 0; i < 999_999_999; i++) {}
  return num * 2;
}
export default function App() {
  const [number, setNumber] = useState(0);
  const [dark, setDark] = useState(false);
  console.log(number);
  const doubleNumber = useMemo(() => {
    return funcDouble(number);
  }, [number]);
  const theme = useMemo(
    () => ({
      backgroundColor: dark ? "#333" : "#FFF",
      color: dark ? "#FFF" : "#333"
    }),
    [dark]
  );
  useEffect(() => {
    console.log("Hlelo");
  }, [theme]);

  return (
    <div className="App" style={theme}>
      <input value={number} onChange={(e) => setNumber(e.target.value)} />
      <button onClick={(e) => setDark((prev) => !prev)}>set theme </button>
      <div>{doubleNumber}</div>
    </div>
  );
}

```

so here, if i press the button, then dark state changes, and because of that the component is re-rendered. but `const doubleNumber` 's value does not change, ie it uses its cached value, because only dark state changed ( not a dependency for doubleNumber useMemo). and so the entire code  inside the memo ie, execution of funcDouble does not take place hence reducing time taken. 
but since dark changed, and dark is a dependency of theme useMemo, the theme variable is re assigned (even though here it's value is same)

The major thing here is, when I press button to change the theme, because the number does not change, the cached value doubleNumber is used, hence skipping the execution of funcDouble.


#### Summary

The `useMemo` hook in React serves two main purposes. Firstly, it is useful for optimizing the performance of slow functions. By wrapping a slow function with `useMemo`, it ensures that the function is not recomputed on every component render. Instead, it only recalculates when the actual value is needed and the inputs to the function have changed.

The second use case for `useMemo` is to maintain referential equality of objects or arrays. When you want to ensure that the reference of an object or array remains the same unless its internal contents have changed, you can use `useMemo`. This prevents unnecessary updates and allows you to update the reference only when the object's contents have actually changed.

By using `useMemo`, you can optimize the performance of your React components by avoiding unnecessary computations and updates. If you want to dive deeper into React, consider checking out the comprehensive React course linked in the description for a thorough understanding of React concepts.
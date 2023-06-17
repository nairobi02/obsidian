
# ![[react.png]] useCallback

very similar to [[useMemo]]

```js
import { useCallback, useState } from "react";
import List from "./List";
import "./styles.css";

export default function App() {
  const [number, setNumber] = useState(1);
  const [dark, setDark] = useState(false);
  const getItems = useCallback(() => {
    return [number, number + 1, number + 2];
  },[number]);
  const theme = {
    backgroundColor: dark ? "#333" : "#FFF",
    color: dark ? "#FFF" : "#333"
  };
  const fool = () => {
    return "fool";
  };
  return (
    <div style={theme} className="App">
      <input
        type="number"
        value={number}
        onChange={(e) => setNumber(parseInt(e.target.value))}
      />
      <button onClick={() => setDark((prevDark) => !prevDark)}>
        Toggle theme
      </button>
      <List getItems={getItems} />
  
    </div>
  );
}

```

```js app.js
import { useState, useEffect } from "react";
const List = ({ getItems }) => {
  const [items, setItems] = useState([]);
  useEffect(() => {
    setItems(getItems());
    console.log("Updating Items");
  }, [getItems]);
  return items.map((item) => <div key={item}>{item}</div>);
};
export default List;

```

only difference between useCallback and useMemo is
in useCallback the funtion itself is being cached and assigned to `getItems` ,
in useMemo, for the same code, the getItems would be assigned the value  `[number, number + 1, number + 2]`


mostly only 2 reasons to use useCallback over useMemo
1. referential equality (useMemo can be used here)
2. if the function itself is slow to create (rare)


for referential equality, it would be better to use useCallback if you were to pass parameters to the function, since here we are caching the function and not the value it returns 

```js
  const getItems = useCallback((additional) => {
    return [number+additional, number + 1 + additional, number + 2 + additional];
  },[number]);
  
```


```js
  useEffect(() => {
    setItems(getItems(5));
    console.log("Updating Items");
  }, [getItems]);
```
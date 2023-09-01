
# ![[react.png]] useContext_Snippet


`src/Context/counter-context`
```js
import React, { useState, useCallback } from "react";

const CounterContext = React.createContext({
  data: 0,
  handleClick: () => {}
});

const CounterContextProvider = (props) => {
  const [data, setData] = useState(0);
  const handleClick = useCallback(() => {
    console.log("--------------");
    setData((prev) => ++prev);
  }, []);
  return (
    <CounterContext.Provider value={{ data, handleClick }}>
      {props.children}
    </CounterContext.Provider>
  );
};
export { CounterContext, CounterContextProvider };
```

`src/App.js`
```js
import AppContent from "./AppContent";
import { CounterContextProvider } from "./Context/counter-context";
import "./styles.css";

export default function App() {
  return (
    <CounterContextProvider>
      <AppContent />
    </CounterContextProvider>
  );
}
```

`src/AppContent.js`
```js
import { CounterContext } from "./Context/counter-context";
import Child1 from "./Child1";
import Child2 from "./Child2";
import { useContext } from "react";
export default function AppContent() {
  const { data, handleClick } = useContext(CounterContext);
  console.log("App content re-rendered");
  return (
    <div className="App">
      {data}
      <Child1 />
      <Child2 />
    </div>
  );
}
```

src/Child1.js
```js
import { CounterContext } from "./Context/counter-context";
import { useContext } from "react";
export default function Child1() {
  const { data, handleClick } = useContext(CounterContext);
  console.log("child1 re-rendered");
  return (
    <div>
      <button onClick={() => handleClick()}>{data} Child1</button>
    </div>
  );
}
```
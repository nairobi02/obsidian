
# ![[react.png]] useContext_Snippet


`src/Context/counter-context`
```js
import AppContent from "./AppContent";
import { useContext } from "react";
import { CounterContextProvider } from "./Context/counter-context";

export default function App() {
  return (
    <CounterContextProvider>
      <AppContent />
    </CounterContextProvider>
  );
}

```

`src/App.js`
```js
import AppContent from "./AppContent";
import { useContext } from "react";
import { CounterContextProvider } from "./Context/counter-context";

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
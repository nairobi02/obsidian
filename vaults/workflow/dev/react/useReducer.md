
# ![[react.png]] [useReducer – React](https://react.dev/reference/react/useReducer)

```js
import { useReducer } from "react";
const emailReducer = (state, action) => {
  if (action.type === "USER_INPUT") {
    return { value: action.val, isValid: action.val.includes("@") };
  }
  return { value: "", isValid: false };
};

export default function App() {
  const [emailState, dispathEmail] = useReducer(emailReducer, {
    value: "",
    isValid: false
  });

  const handleChange = (val) => {
    dispathEmail({ type: "USER_INPUT", val: val });
  };

  return (
    <div className="App">
      <input
        className={"input " + (emailState.isValid ? " green" : " red")}
        type="text"
        value={emailState.value}
        onChange={(e) => handleChange(e.target.value)}
      />
      {emailState.value}
      {" " + emailState.isValid}
    </div>
  );
}
```


```js
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

###  [Parameters](https://react.dev/reference/react/useReducer#parameters "Link for Parameters")

- `reducer`: The reducer function that specifies how the state gets updated. It must be pure, should take the state and action as arguments, and should return the next state. State and action can be of any types.
- `initialArg`: The value from which the initial state is calculated. It can be a value of any type. How the initial state is calculated from it depends on the next `init` argument.
- *optional* `init`: The initializer function that should return the initial state. If it’s not specified, the initial state is set to `initialArg`. Otherwise, the initial state is set to the result of calling `init(initialArg)`.

#### [Returns](https://react.dev/reference/react/useReducer#returns "Link for Returns")

`useReducer` returns an array with exactly two values:

1. The current state. During the first render, it’s set to `init(initialArg)` or `initialArg` (if there’s no `init`).
2. The [`dispatch` function](https://react.dev/reference/react/useReducer#dispatch) that lets you update the state to a different value and trigger a re-render.

#### `useReducer` is preferred over `useState` in the following scenarios:

1. Complex state logic: When the state transitions involve complex logic or depend on multiple variables, `useReducer` provides a more organized and centralized approach. It allows you to handle state updates using a reducer function, making it easier to manage and reason about the state changes. 
2. Large state objects: If your state object has multiple properties and updating them individually with `useState` becomes cumbersome, `useReducer` can be a better choice. It enables you to update the state based on action types, allowing for more granular control over state updates.
3. Sharing state and actions: When you have multiple components that need access to the same state and actions, `useReducer` facilitates state management at a higher level. The state and dispatch function can be passed down through component context or prop drilling without the need to lift the state up in the component hierarchy.
4. Performance optimizations: In some cases, `useReducer` can offer performance optimizations over `useState`. Since the state updates are based on the reducer function, you have more control over how the state is updated. You can choose to update the state only when necessary or use memoization techniques to optimize re-renders.

Let's break down each part:

- `state`: This variable holds the current state value.
    
- `dispatch`: This function is used to dispatch actions to the reducer function. When an action is dispatched, the reducer function is called with the current state and the action, and it returns the new state. The `dispatch` function triggers the state update.
    
- `reducer`: This is a function that receives the current state and an action as arguments, and returns a new state based on the action type. It follows a specific pattern:

```js
const reducer = (state, action) => {
  switch (action.type) {
    case 'ACTION_TYPE_1':
      // return the updated state based on action type 1
    case 'ACTION_TYPE_2':
      // return the updated state based on action type 2
    // ...
    default:
      return state;
  }
};
```
- The `reducer` function uses a `switch` statement to handle different action types. It evaluates the `action.type` and performs the necessary state modifications, returning a new state object.
    
- `initialState`: This parameter represents the initial value of the state.

To update the state, you call the `dispatch` function and pass an action object as an argument. The action object typically has a `type` property, which is a string that describes the action being performed, and it can include additional data as needed. The reducer function then uses this `type` value to determine how to update the state.

Here's an example usage of `useReducer`:
```js
const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  const increment = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const decrement = () => {
    dispatch({ type: 'DECREMENT' });
  };

  return (
    <div>
      <button onClick={decrement}>-</button>
      <span>{state.count}</span>
      <button onClick={increment}>+</button>
    </div>
  );
};

```

#### [Caveats](https://react.dev/reference/react/useReducer#setstate-caveats "Link for Caveats")

- `useReducer` is a Hook, so you can only call it **at the top level of your component** or your own Hooks. You can’t call it inside loops or conditions. If you need that, extract a new component and move the state into it.
    
- In Strict Mode, React will **call your reducer and initializer twice** in order to [help you find accidental impurities.](https://react.dev/reference/react/useReducer#my-reducer-or-initializer-function-runs-twice) This is development-only behavior and does not affect production. If your reducer and initializer are pure (as they should be), this should not affect your logic. The result from one of the calls is ignored.
    
- The `dispatch` function **only updates the state variable for the _next_ render**. If you read the state variable after calling the `dispatch` function, [you will still get the old value](https://react.dev/reference/react/useReducer#ive-dispatched-an-action-but-logging-gives-me-the-old-state-value) that was on the screen before your call.
    
- If the new value you provide is identical to the current `state`, as determined by an [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison, React will **skip re-rendering the component and its children.** This is an optimization. React may still need to call your component before ignoring the result, but it shouldn’t affect your code.
    
- React [batches state updates.](https://react.dev/learn/queueing-a-series-of-state-updates) It updates the screen **after all the event handlers have run** and have called their `set` functions. This prevents multiple re-renders during a single event. In the rare case that you need to force React to update the screen earlier, for example to access the DOM, you can use [`flushSync`.](https://react.dev/reference/react-dom/flushSync)


### with debouncing 

```js
import { useEffect, useReducer } from "react";
import "./index.css";

const emailReducer = (state, action) => {
  if (action.type === "USER_INPUT") {
    return { ...state ,value: action.val };
  }
  if (action.type === "USER_INPUT_VALIDITY") {
    return { ...state, isValid: state.value.includes("@") };
  }
  return { value: "", isValid: false };
};

export default function App() {
  const [emailState, dispathEmail] = useReducer(emailReducer, {
    value: "",
    isValid: false
  });
  useEffect(() => {
    const identifier = setTimeout(() => {
      dispathEmail({ type: "USER_INPUT_VALIDITY" });
    }, 500);
    return () => {
      clearTimeout(identifier);
    };
  }, [emailState.value]);

  const handleChange = (val) => {
    dispathEmail({ type: "USER_INPUT", val: val });
  };

  return (
    <div className="App">
      <input
        className={"input " + (emailState.isValid ? " green" : " red")}
        type="text"
        value={emailState.value}
        onChange={(e) => handleChange(e.target.value)}
      />
      {emailState.value}
      {" " + emailState.isValid}
    </div>
  );
}
```



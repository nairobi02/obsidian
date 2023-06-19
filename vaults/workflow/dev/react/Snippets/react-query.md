# react-query


```bash
npm i @tanstack/react-query
```


root component
```jsx
const queryClient= new QueryClient()

root.render(
  <StrictMode>
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider >
  </StrictMode>
);

```





src/util/query
```jsx
import { useQuery } from "@tanstack/react-query";
export const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));
export const useGet = () => {
  const obj = useQuery({
    queryFn: async () => {
      const response = await fetch(
        "https://jsonplaceholder.typicode.com/todos/1"
      );
      const data = await response.json();
      await wait(5000);
      return data;
    },
    onSuccess: () => {
      //
    },
    onError: () => {
      //
    }
  });
  return obj;
};

//useQuery for GET request,
//useMutation for POST, PUT, DELETE, PATCH

```


```jsx
import {useGet} from './utils/query'
export default function App() {
const {data,isLoading} = useGet() 


  return <div className="App">
    {isLoading?"laoding":
    <div>
  {JSON.stringify(data)}
    </div>}
  </div>;
}
```
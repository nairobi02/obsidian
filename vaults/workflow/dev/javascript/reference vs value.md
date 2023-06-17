# Call by reference v value


An example of it explained for react's useEffect dependency list, 

so its sort of like , for a primitive `variable`, in the useEffect dependency list, react replaces the variable with the value itself, like if `variable= 1` and dependency list is` [variable]`, then react basically makes the dependency list `[1]`, and on the next re render when a new `variable` is created, it compares 
```variable===1``` and since it is true, it doesn't execute the useEffect, but for a non primitive since it will be `obj={} `the dependency list would be` [#hexaddress1]` and on the next re render, the newly created `obj ={}` might still be an empty object, but its address would be `[#hexaddress2] `and  `#hexaddress1!==#hexaddress2` and hence the effect would be considered stale and run.
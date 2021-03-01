# Typesafe-actions

{% embed url="https://github.com/piotrwitek/typesafe-actions\#using-createreducer-api-with-type-free-syntax" %}



## Reducers

can handle multiple actions

```text
  // handle multiple actions using array
  .handleAction([add, increment], (state, action) =>
    state + (action.type === 'ADD' ? action.payload : 1)
  );
```




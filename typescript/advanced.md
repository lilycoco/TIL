# Advanced

## Type Guard

{% embed url="https://basarat.gitbook.io/typescript/type-system/typeguard" %}

{% embed url="https://www.typescriptlang.org/docs/handbook/advanced-types.html" %}

### in

```typescript
interface A {
  x: number;
}
interface B {
  y: string;
}

function doStuff(q: A | B) {
  if ('x' in q) {
    // q: A
  }
  else {
    // q: B
  }
}
```

## Utility Types

{% embed url="https://www.typescriptlang.org/docs/handbook/utility-types.html" %}

### Pick&lt;Type, Keys&gt; <a id="picktype-keys"></a>

Constructs a type by picking the set of properties `Keys` from `Type`.

```javascript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};

```

### Omit&lt;Type, Keys&gt; <a id="omittype-keys"></a>

Constructs a type by picking all properties from `Type` and then removing `Keys`.

```javascript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};

```


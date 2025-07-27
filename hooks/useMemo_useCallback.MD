### **Code**

```jsx
import React, { useState, useMemo } from 'react';

const App = () => {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  function updateCount1() {
    setCount1(prevCount => prevCount + 1);
  }

  function updateCount2() {
    setCount2(prevCount => prevCount + 1);
  }

  const even = useMemo(() => {
    let sum = 0;
    for (let i = 0; i < 1_000_000_000_000; i++) {
      sum += Math.sqrt(i) % 3;
    }
    return count1 % 2 === 0;
  }, [count1]);

  return (
    <>
      <button onClick={updateCount1}>Counter - {count1}</button>
      <span>{even ? 'Even' : 'False'}</span>
      <br />
      <br />
      <button onClick={updateCount2}>Counter - {count2}</button>
    </>
  );
};

export default App;
```

---

## **useMemo vs useCallback — Performance Optimization in React**

---

### `useMemo`

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

* Caches the **result** of the function.
* Runs only when dependencies change.
* Best used for **heavy computations** that do not need to rerun on every render.

#### Example from code:

```js
const even = useMemo(() => {
  let sum = 0;
  for (let i = 0; i < 1_000_000_000_000; i++) {
    sum += Math.sqrt(i) % 3;
  }
  return count1 % 2 === 0;
}, [count1]);
```

**Effect**: This expensive operation runs **only** when `count1` changes. Updates to `count2` do **not** trigger it.

---

### `useCallback`

```js
const memoizedFunction = useCallback(() => {
  // logic
}, [dependencies]);
```

* Caches the **function** reference.
* Useful when passing callbacks to memoized components (`React.memo`) to avoid unnecessary re-renders.

#### Example use case:

```js
const handleClick = useCallback(() => {
  setCount1(prev => prev + 1);
}, []);
```

---

## Variations and Common Patterns

### 1. **Memoizing Derived Data**

```js
const filteredData = useMemo(() => {
  return data.filter(item => item.active);
}, [data]);
```

Used when you need to recompute filtered/mapped/transformed data only when the source data changes.

---

### 2. **Stable Function for Child Props**

```js
const handleChange = useCallback(() => {
  console.log('changed');
}, [someDependency]);

// Passed to a memoized child
<MemoizedInput onChange={handleChange} />
```

Prevents unnecessary renders in children when reference equality matters.

---

### 3. **Avoid Inline Functions in JSX**

```js
const increment = useCallback(() => setCount1(c => c + 1), []);
```

Instead of:

```js
<button onClick={() => setCount1(c => c + 1)}>Click</button>
```

This helps reduce function re-creation and unnecessary updates to effects or child components.

---

## Summary

| Hook          | What it Memoizes       | When to Use                                     |
| ------------- | ---------------------- | ----------------------------------------------- |
| `useMemo`     | **Return value**       | Heavy calculations or derived data              |
| `useCallback` | **Function reference** | Passing stable callbacks to memoized components |

* Avoid using these hooks **by default**; they are optimizations for specific cases.
* Always keep dependency arrays accurate to avoid stale values.


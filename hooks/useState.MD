### 1. **State Initialization**

* **Direct Initialization:**

  ```js
  const [count, setCount] = useState(0);
  ```

  The state is set to `0` directly every time the component renders.

* **Lazy Initialization with Function:**

  ```js
  const [count, setCount] = useState(() => {
    console.log('function rendered');
    return 4;
  });
  ```

  The function is only called once during the initial render, making it useful for expensive computations.

* **Calling Function Outside Component:**

  ```js
  const [count, setCount] = useState(countInitial());
  ```

  If `countInitial()` is defined **outside** the component and **called**, it will run on **every re-render** — not recommended unless intended.

---

### 2. **State Update Pitfalls**

Incorrect:

```js
setCount(count - 1);
```

* This uses the current render's `count`, which may be stale if multiple updates happen rapidly.

Correct:

```js
setCount(prevCount => prevCount - 1);
```

* Uses the previous state value, ensuring correct behavior in all scenarios.

---

### 3. **Object State Management**

Using state as an object:

```js
const [state, setState] = useState({ count: 0, theme: 'blue' });
```

To update only part of the object:

```js
setState(prevState => ({
  ...prevState,
  count: prevState.count + 1,
}));
```

This ensures the `theme` is preserved while updating `count`.

---

## 💡 Key Takeaways

* Use lazy initialization with functions for performance-heavy initial state.
* Avoid calling initialization functions during render unless memoized.
* Use functional updates (`prevState => ...`) when the next state depends on the previous state.
* When using object state, always spread previous state to retain untouched values.

---

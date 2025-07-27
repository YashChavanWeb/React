### **Code**

```jsx
import React, { useRef, useState, useEffect } from 'react';

function App() {
  /*
    The main difference between ref and state is that ref does not 
    cause your component to re-render.
  */

  const inputRef = useRef(null);

  // We'll use state only to display the timer count
  const [displayCount, setDisplayCount] = useState(0);

  // Use ref to store the actual count without triggering re-renders
  const countRef = useRef(0);

  const handleFocus = () => {
    // Access the input DOM element and focus it
    inputRef.current.focus();
  };

  useEffect(() => {
    const intervalId = setInterval(() => {
      // Increment count stored in ref
      countRef.current += 1;

      // Update state to display the current count every second
      setDisplayCount(countRef.current);
    }, 1000);

    // Cleanup on unmount
    return () => clearInterval(intervalId);
  }, []);

  return (
    <>
      <div>
        <input ref={inputRef} type="text" placeholder="Click button to focus me" />
        <button onClick={handleFocus}>Focus Input</button>
      </div>

      <div style={{ marginTop: 20 }}>
        Timer count (using useRef): {displayCount}
      </div>
    </>
  );
}

export default App;
```

---

## 🔍 **Detailed Explanation**

### 1. **Using `useRef` for DOM Access**

```js
const inputRef = useRef(null);
```

* `useRef` is used to create a mutable reference to the input DOM element.
* This allows focusing the input programmatically without re-rendering the component.

---

### 2. **Using `useRef` to Store Mutable Values**

```js
const countRef = useRef(0);
```

* `countRef` holds the current count value but **does not cause re-renders** when updated.
* This is useful for keeping track of values that change frequently but don't need to trigger UI updates directly.

---

### 3. **State for Displaying the Count**

```js
const [displayCount, setDisplayCount] = useState(0);
```

* State is used **only** for the visible count displayed to the user.
* Updating this state triggers a re-render and shows the updated timer count.

---

### 4. **Interval Logic and Cleanup**

```js
useEffect(() => {
  const intervalId = setInterval(() => {
    countRef.current += 1;
    setDisplayCount(countRef.current);
  }, 1000);

  return () => clearInterval(intervalId);
}, []);
```

* An interval updates `countRef.current` every second.
* `setDisplayCount` is called to update the UI.
* Cleanup clears the interval when the component unmounts to avoid memory leaks.

---

### 5. **Focus Button Handler**

```js
const handleFocus = () => {
  inputRef.current.focus();
};
```

* Clicking the button calls `handleFocus`, which focuses the input field using the `inputRef`.

---

## ✅ **Key Takeaways**

* `useRef` stores mutable values or DOM nodes without causing component re-renders.
* Use `useRef` to keep track of data changing outside of the React rendering lifecycle.
* State (`useState`) is necessary to trigger UI updates.
* Combining `useRef` and `useState` can optimize performance by avoiding unnecessary re-renders.
* Always clean up side effects (like intervals) in `useEffect` cleanup functions.



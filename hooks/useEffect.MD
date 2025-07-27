### **Code**

```jsx
import React, { useState, useEffect } from 'react';

const App = () => {
  const [resourceType, setResourceType] = useState('posts');

  // useEffect runs a side effect when resourceType changes
  useEffect(() => {
    console.log('Resource Type Changed');

    // Cleanup function runs before the effect runs again or when the component unmounts
    return () => {
      console.log('This is a cleanup');
    };
  }, [resourceType]);

  console.log('Render');

  return (
    <>
      <button onClick={() => setResourceType('posts')}>Posts</button>
      <button onClick={() => setResourceType('users')}>Users</button>
      <button onClick={() => setResourceType('comments')}>Comments</button>
      <h1>{resourceType}</h1>
    </>
  );
};

export default App;
```

---

## 🔍 **Detailed Explanation**

### 1. **State Initialization**

```js
const [resourceType, setResourceType] = useState('posts');
```

* Initializes `resourceType` to `'posts'`.
* Updates when any of the buttons are clicked.

---

### 2. **Component Lifecycle and Logging**

```js
console.log('Render');
```

* Logs on **every render**, including initial and subsequent re-renders.
* Useful for tracking when the component updates.

---

### 3. **Using `useEffect` for Side Effects**

```js
useEffect(() => {
  console.log('Resource Type Changed');

  return () => {
    console.log('This is a cleanup');
  };
}, [resourceType]);
```

* **Purpose:** Handles side effects that should occur when `resourceType` changes.
* The effect runs:

  * On initial mount.
  * Every time `resourceType` changes.
* **Cleanup Function:**

  * Runs **before the effect is re-executed** (i.e., when `resourceType` changes again).
  * Also runs **once when the component unmounts**.
  * Typically used to remove event listeners, cancel subscriptions, etc.

---

### 4. **Event Handling and UI**

```jsx
<button onClick={() => setResourceType('posts')}>Posts</button>
<button onClick={() => setResourceType('users')}>Users</button>
<button onClick={() => setResourceType('comments')}>Comments</button>
```

* Updates the `resourceType` state based on the button clicked.
* Triggers a re-render.
* Triggers `useEffect` if the new type is different from the current.

```jsx
<h1>{resourceType}</h1>
```

* Displays the current resource type.

---

## ✅ Key Takeaways

* `useEffect` with a dependency array reacts only to specified state changes.
* Cleanup functions prevent memory leaks and ensure correct side-effect lifecycle.
* `console.log('Render')` helps differentiate between rendering and effect execution.
* Keeping the dependency array empty (`[]`) would run the effect **only once** on mount.


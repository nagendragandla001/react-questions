# React Interview Questions and Answers (Q1–Q60)

## React Developer Basics (Q1–Q20)

### 1. Parent state update causes unnecessary child re-render
**Question:** If a parent component updates its state, all child components re-render even when the child does not depend on that state. How can you prevent unnecessary re-renders?

**Explanation:** React re-renders all child components when a parent re-renders by default, even if the children do not depend on the updated state.

**Solution:** Use `React.memo`:
```jsx
const Child = React.memo(() => {
  console.log("Child rendered");
  return <div>Child</div>;
});
```

### 2. React.memo sometimes doesn't prevent re-render
**Problem:** Props are objects or functions created inline.
**Solution:** Use `useMemo` and `useCallback`:
```jsx
const data = useMemo(() => ({ name: "React" }), []);
const handleClick = useCallback(() => {}, []);
<Child data={data} onClick={handleClick} />
```

### 3. useEffect cleanup
**Explanation:** Cleanup prevents memory leaks from timers or subscriptions.
```jsx
useEffect(() => {
  const intervalId = setInterval(() => {}, 1000);
  return () => clearInterval(intervalId);
}, []);
```

### 4. useMemo vs useCallback
| Hook | Purpose |
|---|---|
| useMemo | Memoize value |
| useCallback | Memoize function |

### 5. Infinite re-render issue
**Solution:** Update state inside `useEffect`:
```jsx
useEffect(() => {
  setCount(prev => prev + 1);
}, []);
```

### 6. Avoid using index as key
```jsx
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

### 7. Direct state mutation
```jsx
// Wrong
state.count = 5;
// Correct
setCount(5);
```

### 8. Functional state update
```jsx
setCount(prev => prev + 1);
```

### 9. React rendering process
- State/props change → Virtual DOM → Diffing → Reconciliation → DOM update

### 10. Avoid creating functions inside render
**Solution:** useCallback
```jsx
const handleClick = useCallback(() => {}, []);
```

### 11. Stale closure problem
**Solution:** Functional updates:
```jsx
setCount(prev => prev + 1);
```

### 12. Controlled vs Uncontrolled components
```jsx
<input value={value} onChange={e => setValue(e.target.value)} /> // Controlled
<input ref={inputRef} /> // Uncontrolled
```

### 13. useRef uses
- DOM access, mutable value across renders

### 14. Dependency array missing vs empty
- Missing → runs every render
- Empty → runs once after mount

### 15. Causes of re-render
- State/props change, Context change, Parent re-render, forceUpdate

### 16. Prop drilling solutions
- Context API, Redux/Zustand/React Query

### 17. Side effects inside render?
- Never, use `useEffect`

### 18. Reconciliation
- Compare old vs new virtual DOM, minimal DOM updates

### 19. Large list optimization
- Memoization, virtualization (`react-window`)

### 20. useEffect double run in React 18 StrictMode
- Development only, intentional for detecting side effects

## Intermediate React (Q21–Q40)

### 21. setState is asynchronous
- React batches updates for performance
- Use functional updates when relying on previous state
```jsx
setCount(prev => prev + 1);
```

### 22. React batching
- Multiple state updates batched into a single render to improve performance

### 23. Avoid heavy calculations in render → useMemo
```jsx
const memoizedValue = useMemo(() => expensiveCalculation(a, b), [a, b]);
```

### 24. React.memo may hurt performance
- Only use for expensive child components

### 25. Arrow functions in props cause re-render → useCallback
```jsx
const handleClick = useCallback(() => {}, []);
<Child onClick={handleClick} />
```

### 26. Update state with same value → no re-render
- React skips re-render if state is unchanged

### 27. Immutable state updates
- Always create new objects/arrays to trigger reconciliation

### 28. Stable keys for list
- Keys must be stable to prevent full DOM re-renders

### 29. useEffect vs useLayoutEffect
- useEffect → after paint (non-blocking)
- useLayoutEffect → before paint (blocking, useful for measurements)

### 30. Hooks must be called unconditionally
- Conditional hooks break call order and cause errors

### 31. Missing dependency in useEffect → bug
- Add all state or props referenced inside useEffect to dependency array

### 32. Context API performance issues → memoize value
```jsx
const value = useMemo(() => ({data}), [data]);
<Context.Provider value={value}>
```

### 33. useState vs useRef
- useState triggers re-render
- useRef stores mutable value without re-render

### 34. Error boundaries
- Wrap components to prevent full app crash
```jsx
class ErrorBoundary extends React.Component {
  componentDidCatch(error) { console.log(error); }
  render() { return this.props.children; }
}
```

### 35. useReducer vs useState
- useReducer for complex state with multiple transitions

### 36. SSR hydration issues
- Mismatch occurs if server-rendered HTML differs from client render
- Avoid browser-only APIs during SSR

### 37. Suspense and lazy loading
```jsx
const Dashboard = React.lazy(() => import('./Dashboard'));
<Suspense fallback={<Loader />}><Dashboard /></Suspense>
```

### 38. Server state vs client state
- Server state: async, cached, shared (React Query, SWR)
- Client state: local UI state (useState/Redux)

### 39. Scalable frontend architecture
- Feature-based folders
- Component separation
- Shared UI libraries
- API layer abstraction
- State management strategy

### 40. React StrictMode benefits
- Detects unsafe lifecycle methods
- Warns about deprecated APIs
- Double-invokes functions in development

## Advanced React & Senior-Level (Q41–Q60)

### 41. Performance optimizations
- Component splitting, memoization
- Virtualization
- Code splitting, caching
- CDN optimization

### 42. Global API error handling
- Centralized API client, interceptors
- Error boundaries, fallback UI

### 43. Frontend performance metrics
- FCP, LCP, TTI, Bundle size, API latency
- Tools: Lighthouse, WebPageTest

### 44. Custom hook best practices
- Reusable, prefix with 'use'
- No side effects in body, use useEffect

### 45. React Fiber explanation
- Incremental rendering engine
- Pausable, interruptible, prioritized rendering

### 46. React reconciliation key heuristics
- Different types → full subtree replacement
- Same type → update attributes
- Keys → track list elements efficiently

### 47. Context with frequent updates
- Not suitable for rapidly changing state
- Use dedicated state libraries

### 48. Large table rendering optimization
- Pagination
- Virtualization (`react-window`)
- Memoized row and cell components

### 49. Handling forms efficiently
- Controlled components for simple forms
- Formik / React Hook Form for complex forms
- Memoize inputs to avoid re-renders

### 50. Common pitfalls with useEffect
- Forgetting cleanup for subscriptions
- Missing dependencies → stale closures
- Overfetching data

### 51. Advanced memoization strategies
- useMemo for derived data
- useCallback for stable functions
- React.memo for component memoization

### 52. React Portal usage
- Render outside parent hierarchy
- Useful for modals, tooltips, overlays
```jsx
ReactDOM.createPortal(<Modal />, document.body);
```

### 53. React Profiler usage
- Identify slow/unnecessary re-renders
- Analyze render timings

### 54. Concurrent mode (React 18)
- Interruptible rendering
- Improves responsiveness
- Works with Suspense, transitions

### 55. Transitions
- Mark updates as low priority
```jsx
import { startTransition } from 'react';
startTransition(() => setState(newValue));
```

### 56. Handling asynchronous state updates
- Use functional updates if new state depends on previous state
- Avoid race conditions

### 57. Server-side rendering data fetching
- Fetch data before render
- Use hydration carefully
- Avoid browser-only APIs during SSR

### 58. Avoiding unnecessary renders with selectors
- Use Redux Toolkit / Zustand selectors to limit re-renders

### 59. Testing React components
- Unit: Jest + React Testing Library
- Integration: RTL + mock APIs
- End-to-end: Cypress / Playwright

### 60. Frontend accessibility best practices
- ARIA roles
- Keyboard navigation
- Semantic HTML, focus management



# React Diffing Algorithm

## Introduction
The **diffing algorithm** in React is a core part of the **reconciliation process**. It determines how the **virtual DOM** is compared with the previous version to efficiently update the real DOM.

## Key Concepts

1. **Virtual DOM**
   - React maintains a lightweight copy of the real DOM called the virtual DOM.
   - When state or props change, a new virtual DOM tree is created.

2. **Diffing**
   - React compares the new virtual DOM with the previous one.
   - Only the differences (diffs) are applied to the real DOM, reducing expensive DOM operations.

3. **Heuristics**
   React uses certain heuristics to make diffing **O(n)** instead of **O(n^3)**:
   - **Elements of different types** → Replace entire subtree.
   - **Elements of the same type** → Compare attributes and children.
   - **Lists with keys** → Keys help track which items changed, added, or removed.

## Steps in Diffing Algorithm

1. **Element Type Check**
   - If the element type changes (e.g., `<div>` → `<span>`), React replaces the old subtree entirely.

2. **Attributes Comparison**
   - For elements with the same type, React compares the attributes and updates only the changed ones.

3. **Child Reconciliation**
   - **Without keys:** Children are compared in order. Reordering may lead to unnecessary DOM operations.
   - **With keys:** Helps React identify and match elements correctly to minimize changes.

## Best Practices

- **Always provide unique keys** for list items.
- Avoid using **index as key** when list order can change.
- Keep component trees **shallow** to optimize diffing.
- Split large components into smaller ones to localize updates.

## Example
```jsx
const List = ({ items }) => (
  <ul>
    {items.map(item => (
      <li key={item.id}>{item.name}</li>
    ))}
  </ul>
);
```
- Using `item.id` as a key ensures React efficiently updates only the changed items.

## Conclusion
The **diffing algorithm** is crucial for React's performance. By leveraging virtual DOM, keys, and optimized comparison heuristics, React minimizes direct DOM manipulations and ensures smooth UI updates.




# Redux Toolkit Interview Questions and Answers

## 1. What is Redux Toolkit?
**Answer:**
Redux Toolkit (RTK) is the **official, recommended approach for writing Redux logic**. It simplifies Redux by providing:
- `configureStore` for easy store setup
- `createSlice` for reducers and actions in one place
- `createAsyncThunk` for async logic
- Built-in DevTools and middleware integration

## 2. Why use Redux Toolkit over traditional Redux?
**Answer:**
- Reduces boilerplate
- Simplifies store setup
- Provides opinionated best practices
- Built-in support for immutability with Immer
- Better developer experience (DevTools, TypeScript support)

## 3. Explain `configureStore`
**Answer:**
- Function to create the Redux store
- Accepts `reducer`, `middleware`, `devTools`, and `preloadedState`
- Automatically adds **Thunk middleware**

```javascript
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

const store = configureStore({
  reducer: { counter: counterReducer },
});
```

## 4. What is `createSlice`?
**Answer:**
- Combines action creators and reducers
- Automatically generates action types

```javascript
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => { state.value += 1 },
    decrement: state => { state.value -= 1 },
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

## 5. What is `createAsyncThunk`?
**Answer:**
- Handles asynchronous logic (API calls)
- Automatically dispatches `pending`, `fulfilled`, `rejected` actions

```javascript
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUser = createAsyncThunk(
  'users/fetchUser',
  async (userId) => {
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
  }
);
```

## 6. How to handle loading, success, error states with `createAsyncThunk`?
```javascript
const userSlice = createSlice({
  name: 'user',
  initialState: { data: null, loading: false, error: null },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => { state.loading = true })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  },
});
```

## 7. Explain Immer integration in Redux Toolkit
**Answer:**
- RTK uses **Immer** to allow mutable updates in reducers
- Internally produces immutable updates
- Example: `state.value += 1` is safe

## 8. How to structure Redux Toolkit project?
**Answer:**
- Slice-based folder structure:
```
src/
  store/
    index.js
    counterSlice.js
    userSlice.js
  components/
  pages/
```

## 9. How to use Redux Toolkit with TypeScript?
**Answer:**
- Define `RootState` and `AppDispatch` types
- Use `TypedUseSelectorHook` and `useDispatch` hooks
```typescript
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

## 10. Middleware in Redux Toolkit
**Answer:**
- `configureStore` allows adding custom middleware
- Default includes Thunk
```javascript
const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
});
```

## 11. What is RTK Query?
**Answer:**
- Advanced data fetching tool included in Redux Toolkit
- Simplifies API calls, caching, invalidation
- Reduces boilerplate compared to createAsyncThunk

## 12. Example of RTK Query usage
```javascript
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const api = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  endpoints: (builder) => ({
    getUsers: builder.query({ query: () => 'users' }),
  }),
});

export const { useGetUsersQuery } = api;
```

## 13. Advantages of Redux Toolkit
- Less boilerplate
- Immer for immutable state updates
- Async handling with createAsyncThunk
- Built-in DevTools support
- RTK Query for data fetching

## 14. Common mistakes
- Forgetting to use unique slice names
- Mutating state outside of reducers
- Not handling loading/error states properly
- Using index as key for dynamic lists in connected components




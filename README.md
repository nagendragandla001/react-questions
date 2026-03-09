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


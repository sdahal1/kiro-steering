# Frontend Standards

## React 19 Best Practices

### Component Structure
- Define components and hooks at module level, never inside other components or functions
- Use PascalCase for component names (e.g., `Button`, not `button`)
- Avoid factory functions that create components dynamically
- Never nest component definitions - it causes performance issues and state resets

### Hooks Rules
- Call hooks only at the top level of function components or custom hooks
- Never call hooks conditionally, in loops, or after early returns
- Never call hooks inside event handlers, `useMemo`, or class components
- Custom hooks must start with `use` prefix and call at least one other hook
- Don't pass hooks as props or create higher-order hooks

### Performance Optimization
- Use `useMemo` to cache expensive calculations based on dependencies
- Use `useCallback` to memoize callback functions
- Wrap components with `memo` to prevent unnecessary re-renders when props haven't changed
- Memoize JSX nodes with `useMemo` when appropriate
- Consider React Compiler for automatic optimization (React 19+)

### State Management
- Use `useActionState` for form state management with async actions (React 19)
- Use `useFormStatus` to access form submission status in child components (React 19)
- Use `useTransition` to manage pending states for async operations
- Separate concerns by using multiple `useEffect` hooks for different synchronization processes

### Data Fetching
- Use custom hooks to encapsulate data fetching logic
- Consider the `use` hook for sequential data fetching (future pattern)
- Implement proper cleanup in `useEffect` for subscriptions and connections

### Component Patterns
- Pass JSX as children instead of using factory functions for dynamic components
- Extract list item logic into separate components when using `useCallback`
- Use context with custom hooks to avoid prop drilling
- Keep components pure - avoid mutations of external variables during render

### Forms (React 19)
- Use Actions pattern with `useTransition` for automatic pending state management
- Leverage `useFormStatus` for submit button states
- Handle errors within transition callbacks

### Code Quality
- Ensure components are pure functions - same props should produce same output
- Local mutations within render are acceptable (e.g., creating and modifying local arrays)
- Use `<Profiler>` component to measure rendering performance
- Follow consistent response formats and error handling patterns

# React Interview Questions for 1 Year Experience (50+ Questions)

## 1. What is React and what are its key features?

**Answer:** React is a JavaScript library for building user interfaces, particularly web applications. Key features include:
- **Component-based architecture** - Build encapsulated components that manage their own state
- **Virtual DOM** - Efficient updates through a virtual representation of the DOM
- **JSX** - JavaScript syntax extension that allows writing HTML-like code
- **Unidirectional data flow** - Data flows down from parent to child components
- **Declarative** - Describe what the UI should look like for any given state

## 2. What is JSX?

**Answer:** JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code within JavaScript. It gets transpiled to regular JavaScript function calls.

```jsx
// JSX
const element = <h1>Hello, World!</h1>;

// Transpiles to:
const element = React.createElement('h1', null, 'Hello, World!');
```

## 3. What is the difference between functional and class components?

**Answer:**

**Functional Components:**
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Or with arrow function
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
};
```

**Class Components:**
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Key differences:
- Functional components are simpler and use hooks for state/lifecycle
- Class components use `this.state` and lifecycle methods
- Functional components are the modern preferred approach

## 4. What are React Hooks? Name some commonly used hooks.

**Answer:** Hooks are functions that let you use state and other React features in functional components.

Common hooks:
- `useState` - Manage component state
- `useEffect` - Handle side effects and lifecycle events
- `useContext` - Access React context
- `useReducer` - Manage complex state logic
- `useMemo` - Memoize expensive calculations
- `useCallback` - Memoize functions

## 5. Explain useState hook with an example.

**Answer:** `useState` allows you to add state to functional components.

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

## 6. What is useEffect and when would you use it?

**Answer:** `useEffect` handles side effects in functional components, replacing lifecycle methods from class components.

```jsx
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  // Effect runs after every render
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // Dependency array - only re-run if userId changes

  // Cleanup function
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('Timer tick');
    }, 1000);

    return () => clearInterval(timer); // Cleanup
  }, []); // Empty array = run once on mount

  return user ? <div>{user.name}</div> : <div>Loading...</div>;
}
```

## 7. What are props in React?

**Answer:** Props (properties) are read-only data passed from parent to child components.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Alice" />
      <Welcome name="Bob" />
    </div>
  );
}
```

## 8. What is prop drilling and how can you avoid it?

**Answer:** Prop drilling occurs when you pass props through multiple component layers just to reach a deeply nested component.

**Problem:**
```jsx
function App() {
  const user = { name: 'John' };
  return <Parent user={user} />;
}

function Parent({ user }) {
  return <Child user={user} />;
}

function Child({ user }) {
  return <GrandChild user={user} />;
}

function GrandChild({ user }) {
  return <div>{user.name}</div>;
}
```

**Solutions:**
1. **Context API**
2. **State management libraries** (Redux, Zustand)
3. **Component composition**

## 9. What is React Context API?

**Answer:** Context provides a way to share data between components without prop drilling.

```jsx
import React, { createContext, useContext, useState } from 'react';

// Create context
const UserContext = createContext();

// Provider component
function UserProvider({ children }) {
  const [user, setUser] = useState({ name: 'John' });
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}

// Consumer component
function Profile() {
  const { user } = useContext(UserContext);
  return <div>Welcome, {user.name}!</div>;
}

// App component
function App() {
  return (
    <UserProvider>
      <Profile />
    </UserProvider>
  );
}
```

## 10. What is the Virtual DOM?

**Answer:** The Virtual DOM is a JavaScript representation of the actual DOM. React uses it to:
- Calculate the most efficient way to update the real DOM
- Batch multiple updates together
- Improve performance by minimizing direct DOM manipulation

Process:
1. State changes trigger a new Virtual DOM tree
2. React compares (diffs) the new tree with the previous one
3. React updates only the changed elements in the real DOM

## 11. What are keys in React and why are they important?

**Answer:** Keys help React identify which list items have changed, been added, or removed.

```jsx
// Bad - using index as key
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo.text}</li>
      ))}
    </ul>
  );
}

// Good - using unique identifier
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

## 12. What is the difference between controlled and uncontrolled components?

**Answer:**

**Controlled Components:** Form data is handled by React state
```jsx
function ControlledInput() {
  const [value, setValue] = useState('');

  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}
```

**Uncontrolled Components:** Form data is handled by the DOM
```jsx
function UncontrolledInput() {
  const inputRef = useRef();

  const handleSubmit = () => {
    console.log(inputRef.current.value);
  };

  return <input ref={inputRef} />;
}
```

## 13. What are React lifecycle methods?

**Answer:** Lifecycle methods are hooks that allow you to tap into different phases of a component's life.

**Class Component Lifecycle:**
```jsx
class MyComponent extends React.Component {
  componentDidMount() {
    // After component mounts
  }

  componentDidUpdate(prevProps, prevState) {
    // After component updates
  }

  componentWillUnmount() {
    // Before component unmounts
  }

  render() {
    return <div>Hello World</div>;
  }
}
```

**Functional Component Equivalent:**
```jsx
function MyComponent() {
  useEffect(() => {
    // componentDidMount
    console.log('Component mounted');

    return () => {
      // componentWillUnmount
      console.log('Component will unmount');
    };
  }, []);

  useEffect(() => {
    // componentDidUpdate
    console.log('Component updated');
  });

  return <div>Hello World</div>;
}
```

## 14. What is event handling in React?

**Answer:** React uses SyntheticEvents, which wrap native events to provide consistent behavior across browsers.

```jsx
function Button() {
  const handleClick = (event) => {
    event.preventDefault();
    console.log('Button clicked!');
    console.log('Event type:', event.type);
    console.log('Target:', event.target);
  };

  const handleMouseOver = () => {
    console.log('Mouse over!');
  };

  return (
    <button 
      onClick={handleClick}
      onMouseOver={handleMouseOver}
    >
      Click me
    </button>
  );
}
```

## 15. How do you handle forms in React?

**Answer:**

```jsx
function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });

  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## 16. What is conditional rendering in React?

**Answer:**

```jsx
function UserGreeting({ user, isLoggedIn }) {
  // Using if-else
  if (!isLoggedIn) {
    return <div>Please sign in</div>;
  }

  // Using ternary operator
  return (
    <div>
      {user ? (
        <h1>Welcome back, {user.name}!</h1>
      ) : (
        <h1>Welcome, Guest!</h1>
      )}
    </div>
  );
}

// Using logical AND
function Notification({ messages }) {
  return (
    <div>
      {messages.length > 0 && (
        <h2>You have {messages.length} unread messages.</h2>
      )}
    </div>
  );
}
```

## 17. How do you render lists in React?

**Answer:**

```jsx
function TodoList() {
  const todos = [
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build an app', completed: true },
    { id: 3, text: 'Deploy to production', completed: false }
  ];

  return (
    <ul>
      {todos.map(todo => (
        <li 
          key={todo.id}
          style={{
            textDecoration: todo.completed ? 'line-through' : 'none'
          }}
        >
          {todo.text}
        </li>
      ))}
    </ul>
  );
}
```

## 18. What are React Fragments?

**Answer:** Fragments let you group multiple elements without adding extra DOM nodes.

```jsx
// Using React.Fragment
function MyComponent() {
  return (
    <React.Fragment>
      <h1>Title</h1>
      <p>Description</p>
    </React.Fragment>
  );
}

// Using short syntax
function MyComponent() {
  return (
    <>
      <h1>Title</h1>
      <p>Description</p>
    </>
  );
}
```

## 19. What is the useRef hook?

**Answer:** `useRef` returns a mutable ref object that persists across re-renders.

```jsx
function TextInput() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

// Storing previous values
function Counter() {
  const [count, setCount] = useState(0);
  const prevCountRef = useRef();

  useEffect(() => {
    prevCountRef.current = count;
  });

  return (
    <div>
      <p>Current: {count}, Previous: {prevCountRef.current}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
}
```

## 20. What is prop validation in React?

**Answer:** PropTypes provide runtime type checking for React props.

```jsx
import PropTypes from 'prop-types';

function UserCard({ name, age, email, isActive }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
      {isActive && <span>Active User</span>}
    </div>
  );
}

UserCard.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
  email: PropTypes.string.isRequired,
  isActive: PropTypes.bool
};

UserCard.defaultProps = {
  age: 0,
  isActive: false
};
```

## 21. What is the difference between state and props?

**Answer:**

| State | Props |
|-------|-------|
| Mutable (can be changed) | Immutable (read-only) |
| Owned by the component | Passed from parent |
| Can trigger re-renders when changed | Received from parent component |
| Local to component | External data |

```jsx
function Parent() {
  const [count, setCount] = useState(0); // State
  
  return <Child count={count} />; // Passing as props
}

function Child({ count }) {
  // count is props here - cannot be modified directly
  return <div>Count: {count}</div>;
}
```

## 22. How do you optimize React performance?

**Answer:**

1. **React.memo** - Prevent unnecessary re-renders
```jsx
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
  return <div>{data.name}</div>;
});
```

2. **useMemo** - Memoize expensive calculations
```jsx
function ExpensiveList({ items }) {
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]);

  return <div>Total: {expensiveValue}</div>;
}
```

3. **useCallback** - Memoize functions
```jsx
function Parent({ items }) {
  const handleClick = useCallback((id) => {
    // Handle click logic
  }, []);

  return (
    <div>
      {items.map(item => (
        <Child key={item.id} onClick={handleClick} />
      ))}
    </div>
  );
}
```

## 23. What is React.memo?

**Answer:** React.memo is a higher-order component that memoizes the result and skips re-rendering if props haven't changed.

```jsx
const MyComponent = React.memo(function MyComponent({ name, age }) {
  console.log('Rendering MyComponent');
  return <div>{name} is {age} years old</div>;
});

// With custom comparison
const MyComponent = React.memo(function MyComponent(props) {
  return <div>{props.name}</div>;
}, (prevProps, nextProps) => {
  return prevProps.name === nextProps.name;
});
```

## 24. What are Error Boundaries?

**Answer:** Error Boundaries catch JavaScript errors in component trees and display fallback UI.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.log('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

## 25. What is the useReducer hook?

**Answer:** `useReducer` is used for complex state logic, similar to Redux.

```jsx
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return initialState;
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

## 26. How do you handle API calls in React?

**Answer:**

```jsx
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const response = await fetch('/api/users');
        if (!response.ok) {
          throw new Error('Failed to fetch users');
        }
        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

## 27. What is component composition in React?

**Answer:** Component composition is building complex UIs by combining smaller, reusable components.

```jsx
// Container component
function Card({ children, title }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

// Usage
function App() {
  return (
    <Card title="User Profile">
      <img src="avatar.jpg" alt="User" />
      <p>John Doe</p>
      <p>Software Developer</p>
    </Card>
  );
}
```

## 28. What are Higher-Order Components (HOCs)?

**Answer:** HOCs are functions that take a component and return a new component with additional functionality.

```jsx
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const [isAuthenticated, setIsAuthenticated] = useState(false);

    useEffect(() => {
      // Check authentication
      checkAuth().then(setIsAuthenticated);
    }, []);

    if (!isAuthenticated) {
      return <div>Please log in</div>;
    }

    return <WrappedComponent {...props} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);
```

## 29. What is the difference between useMemo and useCallback?

**Answer:**

- **useMemo**: Memoizes a computed value
- **useCallback**: Memoizes a function

```jsx
function MyComponent({ items, onItemClick }) {
  // useMemo - memoizes the result of the computation
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]);

  // useCallback - memoizes the function itself
  const handleClick = useCallback((id) => {
    onItemClick(id);
  }, [onItemClick]);

  return (
    <div>
      <p>Total: {expensiveValue}</p>
      <button onClick={() => handleClick(1)}>Click</button>
    </div>
  );
}
```

## 30. How do you handle routing in React?

**Answer:** Using React Router:

```jsx
import { BrowserRouter, Routes, Route, Link, useParams } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:id" element={<UserDetail />} />
      </Routes>
    </BrowserRouter>
  );
}

function UserDetail() {
  const { id } = useParams();
  return <div>User ID: {id}</div>;
}
```

## 31. What are custom hooks?

**Answer:** Custom hooks are JavaScript functions that use other hooks and allow you to reuse stateful logic.

```jsx
// Custom hook for API calls
function useApi(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

// Usage
function UserProfile({ userId }) {
  const { data: user, loading, error } = useApi(`/api/users/${userId}`);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>No user found</div>;

  return <div>Welcome, {user.name}!</div>;
}
```

## 32. What is lazy loading in React?

**Answer:** Lazy loading allows you to load components only when they're needed.

```jsx
import React, { Suspense, lazy } from 'react';

// Lazy load components
const LazyComponent = lazy(() => import('./LazyComponent'));
const AnotherLazyComponent = lazy(() => import('./AnotherLazyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
        <AnotherLazyComponent />
      </Suspense>
    </div>
  );
}
```

## 33. How do you test React components?

**Answer:** Using Jest and React Testing Library:

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('renders counter with initial value', () => {
  render(<Counter initialValue={0} />);
  expect(screen.getByText('Count: 0')).toBeInTheDocument();
});

test('increments counter when button is clicked', () => {
  render(<Counter initialValue={0} />);
  const button = screen.getByText('Increment');
  fireEvent.click(button);
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

## 34. What is the useLayoutEffect hook?

**Answer:** `useLayoutEffect` runs synchronously after all DOM mutations but before the browser paints.

```jsx
function MyComponent() {
  const [width, setWidth] = useState(0);
  const divRef = useRef();

  useLayoutEffect(() => {
    // Runs before browser paint
    setWidth(divRef.current.offsetWidth);
  });

  return (
    <div ref={divRef}>
      Width: {width}px
    </div>
  );
}
```

## 35. What are render props?

**Answer:** Render props is a pattern where a component receives a function as a prop that returns a React element.

```jsx
function DataProvider({ render }) {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(setData);
  }, []);

  return render(data);
}

// Usage
function App() {
  return (
    <DataProvider
      render={(data) => (
        <div>
          {data ? <UserList users={data} /> : <div>Loading...</div>}
        </div>
      )}
    />
  );
}
```

## 36. How do you handle forms with multiple inputs efficiently?

**Answer:**

```jsx
function MultiInputForm() {
  const [formData, setFormData] = useState({
    firstName: '',
    lastName: '',
    email: '',
    phone: ''
  });

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setFormData(prevState => ({
      ...prevState,
      [name]: value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      {Object.keys(formData).map(key => (
        <input
          key={key}
          type={key === 'email' ? 'email' : 'text'}
          name={key}
          value={formData[key]}
          onChange={handleInputChange}
          placeholder={key.charAt(0).toUpperCase() + key.slice(1)}
        />
      ))}
      <button type="submit">Submit</button>
    </form>
  );
}
```

## 37. What is the difference between createElement and JSX?

**Answer:**

```jsx
// JSX
const element = <h1 className="greeting">Hello, world!</h1>;

// createElement equivalent
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, world!'
);

// Nested elements
// JSX
const element = (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// createElement equivalent
const element = React.createElement(
  'div',
  null,
  React.createElement('h1', null, 'Title'),
  React.createElement('p', null, 'Content')
);
```

## 38. How do you prevent component re-renders?

**Answer:**

```jsx
// 1. React.memo for functional components
const MyComponent = React.memo(({ name, count }) => {
  return <div>{name}: {count}</div>;
});

// 2. useMemo for expensive calculations
function ExpensiveComponent({ items }) {
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]);

  return <div>{expensiveValue}</div>;
}

// 3. useCallback for event handlers
function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    console.log('clicked');
  }, []); // Empty dependency array

  return <Child onClick={handleClick} />;
}
```

## 39. What is reconciliation in React?

**Answer:** Reconciliation is the process React uses to determine what changes need to be made to the DOM. React compares the new Virtual DOM tree with the previous one and calculates the minimum number of changes needed.

Key concepts:
- **Diffing Algorithm**: Compares trees element by element
- **Keys**: Help React identify which items have changed
- **Component Type**: Different component types result in complete re-renders

## 40. How do you handle side effects in React?

**Answer:**

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Side effect: API call
    const fetchUser = async () => {
      const response = await fetch(`/api/users/${userId}`);
      const userData = await response.json();
      setUser(userData);
    };

    fetchUser();

    // Cleanup function
    return () => {
      // Cancel any ongoing requests if needed
    };
  }, [userId]); // Dependency array

  useEffect(() => {
    // Side effect: DOM manipulation
    document.title = user ? `${user.name}'s Profile` : 'Loading...';
  }, [user]);

  return user ? <div>{user.name}</div> : <div>Loading...</div>;
}
```

## 41. What are the rules of hooks?

**Answer:**

1. **Only call hooks at the top level** - Don't call inside loops, conditions, or nested functions
2. **Only call hooks from React functions** - From React function components or custom hooks

```jsx
// ❌ Wrong
function MyComponent({ condition }) {
  if (condition) {
    const [state, setState] = useState(0); // Don't do this
  }

  return <div>Hello</div>;
}

// ✅ Correct
function MyComponent({ condition }) {
  const [state, setState] = useState(0);

  if (condition) {
    // Use the state here
  }

  return <div>Hello</div>;
}
```

## 42. How do you share state between components?

**Answer:**

**1. Lifting State Up:**
```jsx
function Parent() {
  const [sharedState, setSharedState] = useState('');

  return (
    <div>
      <ChildA state={sharedState} setState={setSharedState} />
      <ChildB state={sharedState} />
    </div>
  );
}
```

**2. Context API:**
```jsx
const StateContext = createContext();

function StateProvider({ children }) {
  const [state, setState] = useState('');
  return (
    <StateContext.Provider value={{ state, setState }}>
      {children}
    </StateContext.Provider>
  );
}
```

## 43. What is the difference between development and production builds?

**Answer:**

**Development Build:**
- Includes helpful warnings and error messages
- Larger file size
- Includes source maps for debugging
- Not optimized for performance

**Production Build:**
- Minified and optimized code
- Smaller file size
- No development warnings
- Optimized for performance

```bash
# Development
npm start

# Production build
npm run build
```

## 44. How do you handle loading states in React?

**Answer:**

```jsx
function DataComponent() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        const response = await fetch('/api/data');
        if (!response.ok) throw new Error('Failed to fetch');
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!data) return <div>No data available</div>;

  return <div>{/* Render data */}</div>;
}

// Custom hook for loading states
function useAsyncData(url) {
  const [state, setState] = useState({
    data: null,
    loading: true,
    error: null
  });

  useEffect(() => {
    const fetchData = async () => {
      setState(prev => ({ ...prev, loading: true, error: null }));
      
      try {
        const response = await fetch(url);
        const data = await response.json();
        setState({ data, loading: false, error: null });
      } catch (error) {
        setState({ data: null, loading: false, error: error.message });
      }
    };

    fetchData();
  }, [url]);

  return state;
}
```

## 45. What is server-side rendering (SSR) in React?

**Answer:** SSR renders React components on the server and sends HTML to the client, improving initial page load and SEO.

```jsx
// Next.js example
function HomePage({ data }) {
  return (
    <div>
      <h1>Welcome</h1>
      <p>{data.message}</p>
    </div>
  );
}

// This runs on the server
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data }
  };
}

export default HomePage;
```

## 46. How do you handle authentication in React?

**Answer:**

```jsx
// Auth Context
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Check if user is logged in
    const token = localStorage.getItem('token');
    if (token) {
      validateToken(token).then(userData => {
        setUser(userData);
        setLoading(false);
      });
    } else {
      setLoading(false);
    }
  }, []);

  const login = async (credentials) => {
    const response = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(credentials)
    });
    
    const { user, token } = await response.json();
    localStorage.setItem('token', token);
    setUser(user);
  };

  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

// Protected Route Component
function ProtectedRoute({ children }) {
  const { user, loading } = useContext(AuthContext);

  if (loading) return <div>Loading...</div>;
  if (!user) return <Navigate to="/login" />;

  return children;
}
```

## 47. What are portals in React?

**Answer:** Portals allow rendering children into a DOM node outside the parent component's DOM hierarchy.

```jsx
import { createPortal } from 'react-dom';

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;

  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        <button onClick={onClose}>×</button>
        {children}
      </div>
    </div>,
    document.getElementById('modal-root') // Renders outside component tree
  );
}

// Usage
function App() {
  const [showModal, setShowModal] = useState(false);

  return (
    <div>
      <button onClick={() => setShowModal(true)}>Open Modal</button>
      <Modal isOpen={showModal} onClose={() => setShowModal(false)}>
        <h2>Modal Content</h2>
        <p>This is rendered in a portal!</p>
      </Modal>
    </div>
  );
}
```

## 48. How do you handle global state management?

**Answer:**

**Using Context + useReducer:**
```jsx
// State management with useReducer
const initialState = {
  user: null,
  todos: [],
  loading: false
};

function appReducer(state, action) {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'ADD_TODO':
      return { ...state, todos: [...state.todos, action.payload] };
    case 'SET_LOADING':
      return { ...state, loading: action.payload };
    default:
      return state;
  }
}

const AppContext = createContext();

function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, initialState);

  const actions = {
    setUser: (user) => dispatch({ type: 'SET_USER', payload: user }),
    addTodo: (todo) => dispatch({ type: 'ADD_TODO', payload: todo }),
    setLoading: (loading) => dispatch({ type: 'SET_LOADING', payload: loading })
  };

  return (
    <AppContext.Provider value={{ state, actions }}>
      {children}
    </AppContext.Provider>
  );
}

// Custom hook
function useApp() {
  const context = useContext(AppContext);
  if (!context) {
    throw new Error('useApp must be used within AppProvider');
  }
  return context;
}
```

## 49. What are the differences between React and other frameworks?

**Answer:**

**React vs Vue:**
- React uses JSX, Vue uses templates
- React has a steeper learning curve
- Vue is more opinionated with built-in solutions
- React has a larger ecosystem

**React vs Angular:**
- React is a library, Angular is a full framework
- Angular uses TypeScript by default
- Angular has more built-in features (routing, HTTP client, etc.)
- React is more flexible and lightweight

**Key React advantages:**
- Large ecosystem and community
- Virtual DOM for performance
- Component reusability
- Backed by Meta (Facebook)
- Flexible and unopinionated

## 50. How do you debug React applications?

**Answer:**

**1. React Developer Tools:**
```jsx
// Install React DevTools browser extension
// Inspect component props, state, and hooks
```

**2. Console logging:**
```jsx
function MyComponent({ data }) {
  console.log('MyComponent rendered with:', data);
  
  useEffect(() => {
    console.log('Effect triggered');
  }, [data]);

  return <div>{data.name}</div>;
}
```

**3. Error boundaries for catching errors:**
```jsx
class ErrorBoundary extends React.Component {
  componentDidCatch(error, errorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // Send to error reporting service
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

**4. React Strict Mode:**
```jsx
function App() {
  return (
    <React.StrictMode>
      <MyApp />
    </React.StrictMode>
  );
}
```

**5. Debugging hooks:**
```jsx
function useDebugValue(value) {
  const [debugValue, setDebugValue] = useState(value);
  
  // Shows in React DevTools
  useDebugValue(debugValue, value => `Debug: ${value}`);
  
  return [debugValue, setDebugValue];
}
```

## Bonus Questions:

## 51. What is React Fiber?

**Answer:** React Fiber is the new reconciliation algorithm that allows React to pause, abort, or reuse work as new updates come in. It enables features like:
- Time slicing
- Suspense
- Concurrent rendering
- Better error handling

## 52. What are synthetic events?

**Answer:** SyntheticEvents are React's wrapper around native events that provide consistent behavior across different browsers.

```jsx
function Button() {
  const handleClick = (syntheticEvent) => {
    console.log(syntheticEvent.type); // 'click'
    console.log(syntheticEvent.currentTarget); // button element
    
    // Access native event
    console.log(syntheticEvent.nativeEvent);
    
    // Prevent default behavior
    syntheticEvent.preventDefault();
  };

  return <button onClick={handleClick}>Click me</button>;
}
```

## 53. How do you implement code splitting in React?

**Answer:**

```jsx
import { lazy, Suspense } from 'react';

// Dynamic imports with lazy loading
const LazyComponent = lazy(() => import('./LazyComponent'));

// Route-based code splitting
const Home = lazy(() => import('./Home'));
const About = lazy(() => import('./About'));

function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </Suspense>
    </Router>
  );
}
```

---

## 📝 Tips for the Interview:

1. **Practice coding**: Be ready to write code on a whiteboard or computer
2. **Understand the fundamentals**: Focus on core concepts before advanced topics
3. **Build projects**: Have examples ready to discuss
4. **Know the ecosystem**: Be familiar with common libraries (React Router, Axios, etc.)
5. **Stay updated**: Keep up with React's latest features and best practices
6. **Ask questions**: Show curiosity about the company's React setup and challenges

Good luck with your React interview! 🚀

---
tags:
  - interviews
  - frontend
---
### 1. What is Vite and why is it preferred over CRA (Create React App)?

**Answer:**  
Vite is a next-generation frontend build tool that provides instant dev server start and lightning-fast HMR (Hot Module Replacement).  
It uses ESBuild for dependency pre-bundling and native ES modules in the browser, unlike CRA which bundles everything upfront using Webpack.  
This makes Vite much faster, especially for large React projects.

---

### 2. How does Vite achieve faster builds?

**Answer:**  
Vite serves source files via native ES modules during development. Only the code that changes is recompiled on save.  
For production, it uses Rollup for optimized bundling and code splitting.  
This hybrid setup gives fast cold starts and optimized final builds.

---

### 3. What are the key differences between React’s `useMemo` and `useCallback`?

**Answer:**

- `useMemo` memoizes the value of a computation (returns result).
    
- `useCallback` memoizes a function definition (returns function).  
    Use `useCallback` to prevent re-renders when passing callback props to child components.
    

---

### 4. How do you type a functional React component in TypeScript?

**Answer:**

```tsx
type Props = {
  name: string;
  age?: number;
};

const UserCard: React.FC<Props> = ({ name, age }) => (
  <div>{name} - {age}</div>
);
```

or without `React.FC` (preferred for better `children` control):

```tsx
interface Props {
  name: string;
  age?: number;
}

function UserCard({ name, age }: Props) {
  return <div>{name} - {age}</div>;
}
```

---

### 5. What is the difference between `useEffect` and `useLayoutEffect`?

**Answer:**

- `useEffect`: runs after the browser paints the screen.
    
- `useLayoutEffect`: runs before paint, blocking visual updates until done.  
    Use `useLayoutEffect` when you need DOM measurements or synchronous layout updates.
    

---

### 6. How do you prevent unnecessary re-renders in React?

**Answer:**

- Wrap components in `React.memo`.
    
- Use `useCallback` and `useMemo` for stable function and value references.
    
- Avoid creating new objects/arrays inside render.
    
- Use keys properly in lists.
    
- Split components logically for isolation.
    

---

### 7. What is the purpose of `tsconfig.json` in a React + Vite project?

**Answer:**  
`tsconfig.json` configures TypeScript compilation — controlling module resolution, JSX type, strictness, aliases (`paths`), and output behavior.  
Vite uses this file to understand TypeScript imports and compile code correctly.

---

### 8. What are “discriminated unions” in TypeScript, and how do they help in React?

**Answer:**  
They allow creating type-safe variants of an object using a common discriminator property.

```ts
type ButtonProps =
  | { variant: "text"; label: string }
  | { variant: "icon"; icon: string };

function Button(props: ButtonProps) {
  if (props.variant === "text") return <span>{props.label}</span>;
  return <img src={props.icon} />;
}
```

TypeScript narrows down the type automatically, preventing invalid prop access.

---

### 9. Explain React’s Virtual DOM and how it improves performance.

**Answer:**  
React maintains a virtual copy of the DOM in memory.  
When state changes, React compares (diffs) the new virtual tree with the previous one and updates only the changed nodes in the actual DOM.  
This minimizes costly real DOM operations, making updates efficient.

---

### 10. How does React handle component re-renders when using state and props?

**Answer:**  
A component re-renders when its state changes or when its props change (causing new references).  
React compares the new and old virtual DOM; only differences trigger actual DOM updates.  
Memoization (`React.memo`) helps prevent re-renders if props haven’t changed.

---

### 11. How do you type an `onChange` event for an input in TypeScript?

**Answer:**

```tsx
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value);
};
```

---

### 12. How do you configure path aliases in a Vite + TS project?

**Answer:**  
Add in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@components/*": ["src/components/*"]
    }
  }
}
```

and in `vite.config.ts`:

```ts
import path from "path";

export default {
  resolve: {
    alias: {
      "@components": path.resolve(__dirname, "src/components"),
    },
  },
};
```

---

### 13. How would you lazy load a React component in Vite?

**Answer:**

```tsx
const Profile = React.lazy(() => import('./Profile'));

function App() {
  return (
    <React.Suspense fallback={<div>Loading...</div>}>
      <Profile />
    </React.Suspense>
  );
}
```

Vite automatically code-splits it into a separate chunk.

---

### 14. How do you handle environment variables in Vite?

**Answer:**  
Vite exposes them via the `import.meta.env` object.  
You define them in `.env` files with the prefix `VITE_`:

```
VITE_API_URL=https://api.example.com
```

Access:

```ts
const api = import.meta.env.VITE_API_URL;
```

---

### 15. How do you optimize a large React app built with Vite?

**Answer:**

- Use code splitting and lazy loading.
    
- Memoize heavy components.
    
- Optimize images with `vite-imagetools`.
    
- Enable `build.minify` and `build.sourcemap` in `vite.config.ts`.
    
- Use the React Developer Tools Profiler to find slow renders.
    
- Prefer React Query / SWR for cached data fetching.
    

---

### 16. What are ES Modules and how does Vite use them?

**Answer:**  
ES Modules use native `import/export` syntax in the browser.  
Vite serves unbundled ES modules directly to the browser during development, so no bundling delay is needed.  
That’s why Vite’s startup time is near instant.

---

### 17. How does TypeScript improve the React developer experience?

**Answer:**

- Prevents runtime errors through static typing.
    
- Provides intellisense and autocompletion in IDEs.
    
- Ensures type-safe props, states, and hooks.
    
- Makes refactoring safe and predictable.
    

---

### 18. How can you handle API responses with proper typing in TypeScript?

**Answer:**

```ts
interface User {
  id: number;
  name: string;
}

async function fetchUser(): Promise<User> {
  const res = await fetch('/api/user');
  return res.json();
}
```

Now you get full intellisense and type safety when accessing user data.

---

### 19. How would you deploy a Vite + React + TS project?

**Answer:**

1. Run `npm run build` → creates `/dist` folder.
    
2. Deploy `/dist` to a static host like Vercel, Netlify, or GitHub Pages.
    
3. Ensure correct base path in `vite.config.ts` for GitHub Pages deployments.
    

---


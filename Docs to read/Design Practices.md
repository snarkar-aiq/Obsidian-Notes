## 1. **Container–Presentational Pattern** #CP-Pattern

👉 **Separates logic from UI rendering**.

- **Container components**: Handle data fetching, business logic, state.
- **Presentational components**: Handle only how things _look_.

```tsx
// Container (logic)
const UserContainer = () => {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    fetch("/api/user")
      .then((res) => res.json())
      .then(setUser);
  }, []);

  return <UserProfile user={user} />;
};

// Presentational (UI)
const UserProfile = ({ user }: { user: User | null }) => (
  <div>
    {user ? <h2>{user.name}</h2> : <p>Loading...</p>}
  </div>
);
```

✅ Benefit: Keeps code maintainable & reusable.


## 2. **Atomic Design Pattern** #atomic-design  

👉 Build UIs from **small to large units** (hierarchical structure).
- **Atoms** → basic elements (button, input)
- **Molecules** → combination of atoms (form field)
- **Organisms** → larger sections (header, card)
- **Templates/Pages** → full layouts

```tsx
// Atom
const Button = ({ children }) => <button className="btn">{children}</button>;

// Molecule
const SearchBar = () => (
  <div>
    <input type="text" placeholder="Search..." />
    <Button>Go</Button>
  </div>
);

// Organism
const Navbar = () => (
  <nav>
    <h1>MyApp</h1>
    <SearchBar />
  </nav>
);
```

✅ Benefit: Scalable design system, reusable UI components.

---

## 3. **State Store Pattern with Zustand**

👉 Keep all **global, cross-component state** in a centralized store (instead of Flux/Redux).

```typescript
// Zustand Store
// Zustand Store
import { create } from "zustand";

type UserStore = {
  user: any;
  fetchUser: () => void;
};

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  fetchUser: async () => {
    const res = await fetch("/api/user");
    const data = await res.json();
    set({ user: data });
  },
}));

// Usage in Container
const UserContainer = () => {
  const { user, fetchUser } = useUserStore();

  useEffect(() => {
    fetchUser();
  }, [fetchUser]);

  return <UserProfile user={user} />;
};

```

✅ **Benefit**: Lightweight, avoids prop drilling, predictable state updates.


⚡ To recap:

1. **Container–Presentational** → separates logic from UI.
    
2. **Atomic Design** → scalable, reusable UI structure.
    
3. **State Store Pattern (Zustand)** → centralized, predictable state management.

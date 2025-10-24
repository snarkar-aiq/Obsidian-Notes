
### **The Frontend Clean Code Ruleset (Inspired by Uncle Bob)**

**Preamble for the Team:**
"The goal of these rules is not to restrict creativity but to empower it. By writing clean code, we spend less time debugging and deciphering, and more time building features and delivering value. This is our shared contract for quality."

---

### **Rule #1: Meaningful Names are Non-Negotiable**
*This is the first and most impactful rule.*

*   **Reveal Intent:** Names should answer the *what*, *why*, and *how* without needing a comment.
    *   **Bad:** `data`, `fn`, `temp`, `div`
    *   **Good:** `userProfile`, `calculateOrderTotal`, `isFormValid`, `searchInput`
*   **Avoid Disinformation:** Don't use words that mean something else.
    *   **Bad:** A list of accounts named `accountList` (if it's not a `List` data structure). Use `accounts` or `accountArray` if the type is important.
*   **Use Pronounceable and Searchable Names:**
    *   **Bad:** `ymdStr` (for creation date)
    *   **Good:** `creationTimestamp`
*   **Use Consistent Vocabulary:** Pick one word per concept and stick to it.
    *   *Everywhere* it's called "User Profile," not sometimes `UserProfile`, `UserInfo`, or `UserData`.
*   **Component Naming:**
    *   **PascalCase** for components: `ProductCard.jsx`, `ShippingAddressForm.vue`
    *   Names should be noun-based, describing their purpose: `Header`, `SearchBar`, `PrimaryButton`.

---

### **Rule #2: Functions & Components Should Do One Thing**
*This is the Single Responsibility Principle (SRP) applied to our code units.*

*   **Small is Beautiful:** Functions should be short. If a function is doing more than one logical task, split it.
*   **One Level of Abstraction per Function:** A function should not mix high-level logic with low-level details.
    *   **Bad:**
        ```javascript
        function processUser(user) {
          // High-level: Validate
          if (!user.email) { /* ... */ }
          // Low-level: Format string
          user.name = user.name.trim().toLowerCase();
          // High-level: Save to API
          api.save(user);
        }
        ```
    *   **Good:**
        ```javascript
        function processUser(user) {
          if (isValidUser(user)) {
            const formattedUser = formatUserData(user);
            saveUser(formattedUser);
          }
        }
        // Each of these functions does ONE thing.
        ```
*   **Component SRP:** A component should represent one, and only one, well-defined piece of the UI.
    *   **Bad:** A `ProductCard` that also handles its own add-to-cart API call and state management.
    *   **Good:** A presentational `ProductCard` that displays data and emits an `onAddToCart` event. The state and logic are handled by a parent component or a state management store.

---

### **Rule #3: Keep it DRY (Don't Repeat Yourself)**
*Duplication is the root of all evil. It's a maintenance nightmare.*

*   **Extract Reusable Logic:** If you see the same code pattern in more than two places, extract it.
    *   Create a custom hook (React), composable (Vue), or utility function.
    *   **Example:** `useApiCall()`, `formatCurrency()`, `useLocalStorage()`.
*   **Extract Reusable UI:** Create base components for repeated UI patterns.
    *   **Example:** ` <Button variant="primary">`, ` <Modal>`, ` <InputField>`.

---

### **Rule #4: Favor Functional, Predictable State**
*Minimize state and keep its mutations predictable.*

*   **Immutability:** Never mutate state directly. Always use the setter function or return a new object/array.
    *   **Bad:** `user.name = 'New Name';`
    *   **Good:** `setUser({ ...user, name: 'New Name' });`
*   **Colocate State:** Keep state as close as possible to where it's used. Avoid lifting state too high unnecessarily.
*   **Prefer Pure Functions:** For logic, data transformations, and reducers, use pure functions (same input always produces the same output, no side-effects). This makes code incredibly easy to test and reason about.

---

### **Rule #5: Structure and Dependencies**
*A clean codebase is a well-organized one.*

*   **The Boy Scout Rule:** "Leave the code cleaner than you found it." Every time you touch a file, improve it slightly.
*   **Stable Dependencies Principle:** Depend in the direction of stability. Your core business logic (e.g., `utils/`, `lib/`) should not depend on volatile, framework-specific code (e.g., components).
*   **Dependency Injection:** Don't hardcode dependencies inside components or functions. Inject them as props or parameters. This makes testing trivial.
    *   **Bad:** `api.fetch('/users')` hardcoded inside a component.
    *   **Good:** A component that accepts a `fetchUsers` function as a prop.

---

### **Rule #6: Error Handling is a Feature, Not an Afterthought**
*Write robust code that anticipates failure.*

*   **Use Try/Catch Gracefully:** Handle errors at the right level of abstraction. Don't just swallow them.
*   **Provide Context with Errors:** When throwing an error, provide a meaningful message that helps with debugging.
*   **Use Error Boundaries (React):** Catch JavaScript errors anywhere in the component tree and display a fallback UI instead of crashing the whole app.

---

### **Rule #7: Comments are a Failure of Expression**
*The best comment is one you didn't need to write.*

*   **Don't Comment Bad Code - Rewrite It:** A comment is often an apology for unreadable code.
*   **Good Comments are Rare:**
    *   Legal comments (e.g., copyright).
    *   Explanations of *why* a non-intuitive approach was necessary.
    *   Warnings about consequences.
*   **Bad Comments are Noise:**
    *   Redundant comments: `// increments i by 1` next to `i++;`
    *   Journal comments: `// Changed by Bob on 10/25...` (Use Git for this!).

---

### **Rule #8: TDD (Test-Driven Development) is Your Safety Net**
*Clean code that doesn't work is worthless. Tests give you the courage to refactor.*

*   **Write Tests:** You are *required* to write unit and integration tests for your components and logic.
*   **The Three Laws of TDD:**
    1.  You are not allowed to write any production code unless it is to make a failing unit test pass.
    2.  You are not allowed to write any more of a unit test than is sufficient to fail.
    3.  You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
*   **FIRST Principles for Tests:**
    *   **F**ast
    *   **I**ndependent
    *   **R**epeatable
    *   **S**elf-Validating
    *   **T**imely

### **Implementation Steps for Your Team:**

1.  **Socialize the Rules:** Don't just email this. Have a meeting to walk through it. Explain the *why* behind each rule.
2.  **Lead by Example:** In your new designs and code reviews, consistently apply these rules.
3.  **Integrate with Code Reviews:** Make this ruleset the foundation of your Pull Request checklist.
4.  **Use Linting & Formatting:** Enforce code style automatically with tools like **ESLint** and **Prettier**. This frees up mental energy to focus on the architectural rules above.
5.  **Refactor Legacy Code Incrementally:** Don't try to rewrite everything at once. Apply the "Boy Scout Rule" every time someone touches an old file.

By adopting this ruleset, you're not just writing code; you're building a professional, sustainable, and high-quality codebase that will be a joy to work on for years to come.
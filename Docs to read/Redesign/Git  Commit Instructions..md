Of course. Here are instructions for writing excellent Git commit messages, following Clean Code principles of clarity, simplicity, and intentionality.

Think of a commit message as a **clear, concise, and contextual note to your future self and your teammates.** A well-crafted message is a fundamental part of the codebase's documentation.

---

### The Golden Rule: The 50/72 Rule

*   **First Line (Subject):** **50 characters** or less.
*   **Body (Description):** Wrap text at **72 characters**.

This makes the log readable in terminals and tools like `git log`, `gitk`, and GitHub.

---

### The Structure of a Clean Commit Message

A perfect commit message has three distinct parts, separated by a blank line.

```
<type>(<scope>): <Subject> in imperative mood

<body>

<footer>
```

Let's break down each part.

#### 1. The Subject Line (The Headline)

This is the most critical part. It must stand alone and summarize the change.

*   **Format:** `<type>(<scope>): <Short Description>`
    *   **`<type>`:** Categorizes the purpose of the change. Keeps the history logical.
        *   `feat`: A new feature.
        *   `fix`: A bug fix.
        *   `docs`: Documentation changes.
        *   `style`: Changes that do not affect meaning (white-space, formatting, etc.).
        *   `refactor`: A code change that neither fixes a bug nor adds a feature.
        *   `perf`: A code change that improves performance.
        *   `test`: Adding missing tests or correcting existing tests.
        *   `chore`: Changes to the build process, tools, or libraries (no production code change).
    *   **`<scope>` (Optional):** Specifies the part of the codebase affected. E.g., `(auth)`, `(user-api)`, `(navbar)`. Use a short, descriptive term.
    *   **`<Short Description>`:** A concise summary of the change.

*   **Use Imperative Mood:** Write the subject line as if you are giving a command.
    *   **Good:** `feat(auth): add password strength meter`
    *   **Bad:** `feat(auth): added password strength meter` (Uses past tense)
    *   **Bad:** `feat(auth): adding password strength meter` (Uses continuous tense)

    *Why?* This convention matches the language used by Git itself in commands like `Merge` and `Revert`. It's consistent and sounds authoritative.

#### 2. The Body (The Explanation)

Separated from the subject by a blank line, the body provides the *context* and *motivation* for the change, not the details of *what* changed (the diff shows that).

*   **Explain the *Why*, not the *What*:** The code diff shows *what* changed. Your message should explain *why* you made the change.
    *   What was the problem?
    *   How does this commit solve it?
    *   Are there any side effects?

*   **Use Bullet Points:** If the explanation is complex, use bullet points with hyphens or asterisks.

**Example:**
```
feat(api): rate limit user login endpoint

- The /login endpoint was vulnerable to brute-force attacks, allowing unlimited requests from a single IP.
- Implement a token-bucket algorithm to limit requests to 10 per minute per IP.
- Exceeds limits will return a 429 Too Many Requests response.

This change directly mitigates the risk of credential stuffing attacks.
```

#### 3. The Footer (For Metadata)

The footer is for referencing external resources and noting breaking changes.

*   **Referencing Issues:** Link your commit to a ticket in your project management tool (e.g., Jira, GitHub Issues).
    *   `Closes #123`
    *   `Fixes JIRA-456`
    *   `Refs #789`

*   **Breaking Changes:** If your commit introduces a change that is not backward-compatible, you MUST start the footer with `BREAKING CHANGE:`, followed by a description of what changed and how to migrate.
    ```
    BREAKING CHANGE: The `getUserProfile` API response format has been updated.

    The `name` field is now split into `firstName` and `lastName`.
    To migrate, update your clients to use the new field names.
    ```

---

### Putting It All Together: A Complete Example

**Poor Commit Message:**
```
fixed bug
```
*(This is useless. What bug? Where?)*

**Good Commit Message:**
```
fix(login): prevent null pointer exception on missing user profile

- The `UserService.getProfile()` method could return null for orphaned accounts.
- The login controller did not handle this case, causing a 500 error.

This fix adds a null check and redirects the user to a profile setup page instead of crashing.

Fixes #224
```

---

### Summary: The Clean Code Commit Checklist

1.  **[ ] Subject is 50 chars or less.**
2.  **[ ] Subject uses imperative mood ("Add", not "Added").**
3.  **[ ] Subject includes a type and optional scope (`feat:`, `fix(api):`).**
4.  **[ ] Body and subject are separated by a blank line.**
5.  **[ ] Body explains the *why* and the context, not the *what*.**
6.  **[ ] Body text is wrapped at 72 characters.**
7.  **[ ] Footer references issues (`Closes #123`).**
8.  **[ ] Breaking changes are explicitly marked.**
9.  **[ ] The entire message is clear enough for a teammate to understand without asking you.**

By following these rules, your Git history becomes a valuable, readable story of the project's evolution, which is the ultimate goal of Clean Code.
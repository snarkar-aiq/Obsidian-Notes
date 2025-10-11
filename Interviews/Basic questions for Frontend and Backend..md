---
tags:
  - interviews
  - full-stack
---
## ⚛️ Frontend (React) Basic Questions

1. **What is React?**
    
    - _Answer:_ A **JavaScript library** for building user interfaces, particularly single-page applications where data changes over time.
        
2. **What is the Virtual DOM (VDOM)?**
    
    - _Answer:_ The **Virtual DOM** is a programming concept where a virtual representation of a UI is kept in memory and synced with the "real" DOM. When the state changes, React compares the new VDOM with the previous one (a process called **"diffing"**) and only updates the minimal necessary part of the real DOM. This makes updates faster and more efficient.
        
3. **What are Components in React?**
    
    - _Answer:_ **Components** are independent, reusable pieces of code that represent a part of the user interface. They can be broadly categorized as **Functional Components** (preferred, use Hooks) and **Class Components** (use class syntax).
        
4. **What is State and Props?**
    - _Answer:_
        
        - **State:** An object that holds data that may change over the lifetime of the component. It's **internal** and **managed within** the component.
            
        - **Props (Properties):** Read-only data passed from a **parent component down to a child component**. They allow components to communicate and are **immutable** within the child component.
            
5. **What are Hooks?**
    - _Answer:_ **Hooks** are functions that let you "hook into" React state and lifecycle features from functional components. The most fundamental ones are `useState` (for managing state) and `useEffect` (for handling side effects like data fetching or subscriptions).
    - useState, useEffect, useReducer, useContext, useLayoutEffect, useCallback.

---

<div style="page-break-after: always;"></div>

## ⚙️ Backend (General/Conceptual) Basic Questions

1. **What is the primary role of the Backend?**
    
    - _Answer:_ The Backend's primary role is to **store, organize, and process data** and make it accessible to the Frontend. It handles **business logic**, **security/authentication**, and **database communication**.
        
2. **What is an API (Application Programming Interface)?**
    
    - _Answer:_ An **API** is a set of rules and protocols that allows different software applications to communicate with each other. In a web context, this usually refers to a set of **endpoints** (URLs) that the Frontend uses to request or send data to the Backend.
        
3. **What are the main HTTP Methods and what are they used for?**
    
    - _Answer:_ The main methods (often associated with **RESTful APIs**) are:
        
        - **GET:** Retrieve data from a specified resource (Read).
            
        - **POST:** Submit data to be processed to a specified resource (Create).
            
        - **PUT:** Update a specified resource entirely (Replace/Update).
            
        - **DELETE:** Delete a specified resource (Delete).
            
4. **What is a Database?**
    
    - _Answer:_ A **Database** is an organized collection of structured information, or data, typically stored electronically in a computer system. They are crucial for persistent storage of application data.
        
5. **What is the difference between SQL and NoSQL databases?**
    
    - _Answer:_
        
        - **SQL (Relational):** Data is stored in **tables** with a predefined, fixed schema. They are good for complex queries and applications requiring strong transactional integrity (e.g., banking systems). Examples: PostgreSQL, MySQL.
            
        - **NoSQL (Non-relational):** Data is stored in flexible schemas (e.g., documents, key-value pairs, graphs). They offer horizontal scalability and are good for large amounts of rapidly changing, unstructured data. Examples: MongoDB, Cassandra.
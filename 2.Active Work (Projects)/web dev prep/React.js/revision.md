Perfect. Now we reach the **most important technology** in your internship description.

> If I had to rank the entire stack:
> 
> 1. ⭐⭐⭐⭐⭐ React
>     
> 2. ⭐⭐⭐⭐⭐ JavaScript
>     
> 3. ⭐⭐⭐⭐ Node + Express
>     
> 4. ⭐⭐⭐ SQL
>     
> 5. ⭐⭐⭐ Git
>     

If you become comfortable with React, you'll already understand a huge part of modern frontend development.

---

# React.js

**Official Name:** React

**Created By:** Meta (Facebook)

**Purpose:** A JavaScript library for building user interfaces (UI).

> **React is a library, not a framework.**

---

# Why React?

Imagine building a website using only HTML and JavaScript.

```text
Navbar

Sidebar

Cards

Footer

Button

Profile

Login

Dashboard
```

Without React, you'd manually create and update each part.

With React, you split the UI into **small reusable pieces** called **components**.

---

# Component-Based Architecture ⭐⭐⭐⭐⭐

Think of LEGO.

```text
Website

│

├── Navbar

├── Sidebar

├── Card

├── Footer

├── Button

└── Profile
```

Each piece is a **Component**.

Example

```jsx
function Navbar() {
    return <h1>Navbar</h1>;
}
```

Use it

```jsx
<Navbar />
```

Reuse it 100 times.

---

# JSX ⭐⭐⭐⭐⭐

**Full Form:** JavaScript XML

JSX lets you write HTML-like syntax inside JavaScript.

Example

```jsx
const element = <h1>Hello</h1>;
```

Actually becomes

```javascript
React.createElement(...)
```

The browser never sees JSX.

Flow:

```text
JSX

↓

Babel

↓

JavaScript

↓

Browser
```

---

# Babel

A JavaScript compiler.

Converts

```jsx
<h1>Hello</h1>
```

into JavaScript React understands.

---

# Virtual DOM ⭐⭐⭐⭐⭐

One of the most common interview questions.

Normal DOM

```text
Change button

↓

Browser updates entire page
```

Virtual DOM

```text
Change button

↓

React creates virtual copy

↓

Finds differences

↓

Updates ONLY changed parts
```

This process is called **Reconciliation**.

---

# Props ⭐⭐⭐⭐⭐

Props = **Properties**

They allow a parent component to send data to a child.

Example

```jsx
function Welcome(props) {
    return <h1>Hello {props.name}</h1>;
}
```

Use

```jsx
<Welcome name="Retr0" />
```

Output

```text
Hello Retr0
```

### Important

Props are **Read Only**.

Child components should not modify them.

---

# State ⭐⭐⭐⭐⭐

Props = data from parent.

State = data owned by the component.

Example

Counter

```text
0

↓

1

↓

2
```

The value changes.

That changing value belongs in **State**.

---

# useState ⭐⭐⭐⭐⭐

Hook used to create state.

```jsx
const [count, setCount] = useState(0);
```

Explanation

```text
count

Current value

↓

setCount()

Updates value
```

Example

```jsx
<button onClick={() => setCount(count + 1)}>
Increment
</button>
```

---

# Hooks ⭐⭐⭐⭐⭐

Hooks let functional components use React features.

Common Hooks

```text
useState

useEffect

useContext

useMemo

useRef
```

For internships, focus mainly on:

- `useState`
    
- `useEffect`
    

---

# useEffect ⭐⭐⭐⭐⭐

Runs side effects.

Examples:

- Fetch API data
    
- Timers
    
- Event listeners
    

Example

```jsx
useEffect(() => {
    console.log("Component Mounted");
}, []);
```

The empty dependency array (`[]`) means it runs once after the component mounts.

---

# Component Lifecycle (Simplified)

```text
Component Created

↓

Rendered

↓

Updated

↓

Removed
```

`useEffect` helps you react to these stages.

---

# Event Handling

Example

```jsx
<button onClick={handleClick}>
Click
</button>
```

Common Events

```text
onClick

onChange

onSubmit

onMouseOver
```

---

# Conditional Rendering ⭐⭐⭐⭐⭐

Show UI only if a condition is true.

```jsx
{isLoggedIn ? <Home /> : <Login />}
```

---

# Lists ⭐⭐⭐⭐⭐

```jsx
users.map(user =>
    <User />
)
```

React loves `map()`.

---

# Keys ⭐⭐⭐⭐⭐

Whenever rendering lists

```jsx
users.map(user =>
    <User key={user.id}/>
)
```

Keys help React identify which item changed.

Interview question:

**Why shouldn't you use array indexes as keys?**

Because when items are added, removed, or reordered, indexes change, causing React to reuse the wrong components and potentially preserve incorrect state.

---

# Forms ⭐⭐⭐⭐☆

Controlled Components

```jsx
const [name,setName]=useState("");
```

```jsx
<input
value={name}
onChange={(e)=>setName(e.target.value)}
/>
```

React controls the input value.

---

# React Router ⭐⭐⭐⭐☆

Navigation without reloading.

```jsx
<Route path="/about"/>
```

Concepts

- BrowserRouter
    
- Routes
    
- Route
    
- Link
    
- useNavigate
    

---

# Fetch API ⭐⭐⭐⭐⭐

Get backend data.

```jsx
useEffect(()=>{
fetch("/api/users")
},[])
```

Usually inside `useEffect`.

---

# Lifting State Up ⭐⭐⭐⭐☆

Two child components need the same data.

Instead of each storing its own copy:

```text
Parent

│

├── Child A

└── Child B
```

Move the state into the parent and pass it down via props.

---

# Context API ⭐⭐⭐☆

Avoid "prop drilling" (passing props through many layers).

Example

Theme

```text
Dark

Light
```

User login

Language

---

# React Fragment

Instead of

```jsx
<div>

</div>
```

Use

```jsx
<>

</>
```

Adds no extra HTML element.

---

# useRef ⭐⭐⭐☆

Stores a value that doesn't trigger a re-render.

Commonly used for:

- Accessing DOM elements
    
- Managing focus
    
- Holding mutable values
    

---

# useMemo ⭐⭐⭐☆

Optimizes expensive calculations.

Avoids recalculating values unnecessarily.

---

# React.memo ⭐⭐⭐☆

Prevents unnecessary re-renders of components when props haven't changed.

---

# Folder Structure

Typical React project

```text
src/

components/

pages/

hooks/

assets/

App.jsx

main.jsx
```

---

# Vite

Modern React build tool.

Create project

```bash
npm create vite@latest
```

Install

```bash
npm install
```

Run

```bash
npm run dev
```

---

# npm

**Full Form:** Node Package Manager

Used to install libraries.

```bash
npm install react-router-dom
```

---

# Common React Packages

```text
react-router-dom

axios

tailwindcss

redux

react-icons
```

---

# React vs JavaScript

|JavaScript|React|
|---|---|
|Programming language|UI library|
|Manipulates DOM manually|Uses Virtual DOM|
|General-purpose|Builds user interfaces|

---

# Props vs State

|Props|State|
|---|---|
|Passed from parent|Owned by component|
|Read-only|Can change|
|External data|Internal data|

---

# Controlled vs Uncontrolled Components

|Controlled|Uncontrolled|
|---|---|
|Managed by React state|Managed by DOM|
|Preferred in React|Simpler but less flexible|

---

# Virtual DOM vs Real DOM

|Virtual DOM|Real DOM|
|---|---|
|In memory|Browser DOM|
|Fast diffing|Direct updates|
|Updates only changed parts|Manipulates actual page|

---

# Full Forms

React → (It's just the library's name; not an acronym.)

JSX → JavaScript XML

DOM → Document Object Model

VDOM → Virtual DOM

SPA → Single Page Application

npm → Node Package Manager

---

# Interview Questions

- What is React?
    
- Why React?
    
- Library vs Framework?
    
- What is JSX?
    
- What is the Virtual DOM?
    
- What is Reconciliation?
    
- Props vs State?
    
- What is `useState`?
    
- What is `useEffect`?
    
- Why are keys important?
    
- What is prop drilling?
    
- What is the Context API?
    
- What is React Router?
    
- What is a controlled component?
    
- Why use Vite instead of older tooling?
    

---

# 📌 Priority Order

1. Components ⭐⭐⭐⭐⭐
    
2. JSX ⭐⭐⭐⭐⭐
    
3. Props ⭐⭐⭐⭐⭐
    
4. State ⭐⭐⭐⭐⭐
    
5. `useState` ⭐⭐⭐⭐⭐
    
6. `useEffect` ⭐⭐⭐⭐⭐
    
7. Event Handling ⭐⭐⭐⭐⭐
    
8. Conditional Rendering ⭐⭐⭐⭐⭐
    
9. Lists & Keys ⭐⭐⭐⭐⭐
    
10. Forms ⭐⭐⭐⭐☆
    
11. React Router ⭐⭐⭐⭐☆
    
12. Fetch API ⭐⭐⭐⭐⭐
    
13. Lifting State Up ⭐⭐⭐⭐☆
    
14. Context API ⭐⭐⭐☆
    
15. `useRef`, `useMemo`, `React.memo` ⭐⭐⭐☆
    

---

## 💡 For this internship

If you can build these **five projects without looking at tutorials**, you're in excellent shape:

1. **Todo App** (CRUD + `useState`)
    
2. **Weather App** (Fetch API + `useEffect`)
    
3. **Notes App** (Forms + Lists)
    
4. **Simple Blog** (React Router + Components)
    
5. **Employee Dashboard** (REST API + React + CRUD)
    

Those projects cover nearly every React concept listed in the job description and give you concrete examples to discuss during interviews.

The next topic is **Node.js**, where you'll learn how the backend receives requests from React, talks to a database, and sends responses back. That completes the frontend-to-backend connection.
Perfect. We'll treat this like an interview bootcamp. Instead of just memorizing, you'll understand **why** each technology exists, how it fits into a full-stack application, and the kinds of questions interviewers ask.

Here's our roadmap:

1. HTML
    
2. CSS
    
3. JavaScript
    
4. TypeScript
    
5. React
    
6. Node.js
    
7. Express.js
    
8. REST APIs
    
9. SQL
    
10. MongoDB
    
11. Git/GitHub
    
12. Debugging
    
13. Performance
    
14. Deployment
    
15. Interview Questions
    
16. Build a mini full-stack project
    

---

# Part 1 — HTML

## What is HTML?

Imagine you're building a house.

- **HTML** → The structure (walls, rooms, doors)
    
- **CSS** → The paint, furniture, decorations
    
- **JavaScript** → The electricity, lights, automatic doors
    

Without HTML, there is nothing for the browser to display.

HTML stands for **HyperText Markup Language**.

Notice it's called a **markup language**, not a programming language.

Why?

Because HTML **describes** content—it doesn't make decisions, perform calculations, or contain programming logic.

For example:

```html
<h1>Welcome</h1>
<p>This is my website.</p>
```

This tells the browser:

- "This is a heading."
    
- "This is a paragraph."
    

---

# How does a browser read HTML?

When you visit a website:

```
Browser
    ↓
Downloads HTML
    ↓
Reads it from top to bottom
    ↓
Creates a DOM Tree
    ↓
Displays the page
```

The browser doesn't show raw HTML. It first converts it into a **DOM (Document Object Model)**.

For example:

```html
<body>
    <h1>Hello</h1>
    <p>World</p>
</body>
```

becomes something like:

```
Document
   │
 body
 ├── h1
 │     └── "Hello"
 └── p
       └── "World"
```

This tree is what JavaScript later interacts with.

---

# What are Tags?

Everything in HTML is built using **elements**, which are written using **tags**.

Example:

```html
<p>Hello</p>
```

Here:

- `<p>` → Opening tag
    
- `Hello` → Content
    
- `</p>` → Closing tag
    

Together, they form an **HTML element**.

---

# Basic HTML Document

Every HTML page follows the same structure:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>My Website</title>
</head>

<body>

    <h1>Hello World</h1>

</body>

</html>
```

### What each part does

**`<!DOCTYPE html>`**

- Tells the browser this is an HTML5 document.
    

**`<html>`**

- The root element containing the whole page.
    

**`<head>`**

- Information about the page that isn't shown directly (title, metadata, CSS links, scripts).
    

**`<body>`**

- Everything visible to the user.
    

---

# Block vs Inline Elements

This is a favorite interview topic.

## Block Elements

They start on a new line and usually take the full available width.

Examples:

```html
<div>
<p>
<h1>
<section>
<header>
<footer>
```

Visualize:

```
-------------
Heading
-------------

-------------
Paragraph
-------------

-------------
Another Div
-------------
```

---

## Inline Elements

They stay within the same line as surrounding text.

Examples:

```html
<span>
<a>
<strong>
<em>
<img>
```

Example:

```html
Hello <strong>World</strong>
```

Output:

```
Hello World
```

The `<strong>` doesn't force a new line.

---

# Semantic HTML

Older HTML often used many `<div>` elements:

```html
<div>
    <div>
        <div>
        </div>
    </div>
</div>
```

This works, but it doesn't describe the page.

Semantic HTML uses meaningful tags:

```html
<header>
<nav>
<main>
<section>
<article>
<aside>
<footer>
```

Benefits:

- Easier to read and maintain
    
- Better for accessibility
    
- Better for SEO
    
- Easier for screen readers
    

---

# Forms

Forms are how users send data.

Example:

```html
<form>
    <input type="text">
    <input type="email">
    <input type="password">
    <button>Login</button>
</form>
```

Common input types:

- `text`
    
- `email`
    
- `password`
    
- `number`
    
- `date`
    
- `checkbox`
    
- `radio`
    
- `file`
    

---

# Attributes

Attributes provide extra information to elements.

Example:

```html
<a href="https://google.com">Google</a>
```

- `href` is an attribute.
    
- It tells the browser where the link goes.
    

Another example:

```html
<img src="cat.jpg" alt="A cat">
```

- `src` → image location
    
- `alt` → alternative text for accessibility or if the image fails to load
    

---

# Common Interview Questions

- What is HTML?
    
- Is HTML a programming language?
    
- What is semantic HTML?
    
- Difference between `<div>` and `<span>`?
    
- Difference between block and inline elements?
    
- What is the purpose of `<!DOCTYPE html>`?
    
- What is the DOM?
    
- Why is the `alt` attribute important?
    
- What is the difference between `id` and `class`?
    

---

## Your task (15–20 minutes)

Without looking at any tutorial, create a simple HTML page that includes:

- A proper HTML5 document structure
    
- A `<header>` with a title
    
- A `<nav>` with three links
    
- A `<main>` section
    
- An `<article>` containing a heading and paragraph
    
- An image with an `alt` attribute
    
- A form with:
    
    - Name
        
    - Email
        
    - Password
        
    - Submit button
        
- A `<footer>`
    

This one exercise will cover most of the HTML concepts companies expect for an internship.

Once you're done (or if you get stuck), we'll move on to **CSS**, where we'll learn how the browser lays out elements using the box model, Flexbox, and Grid. Those are among the most frequently tested frontend concepts in interviews.
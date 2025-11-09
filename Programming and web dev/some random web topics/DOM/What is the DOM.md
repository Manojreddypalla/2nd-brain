## 🌍 What is the DOM (Document Object Model)?

### 🧩 Definition

The **DOM** is a _programming interface_ for web documents.

When a browser loads an HTML page:

- It reads the HTML and builds a **tree-like structure** (called the DOM Tree).
- Each HTML tag becomes a **node** (element node, text node, comment node, etc.).
- JavaScript can use this structure to **access, modify, or delete** any part of the page.

---

## 🧱 DOM Structure

Let’s take this HTML:

```html
<html>
  <body>
    <h1>Hello!</h1>
    <p>Welcome to the DOM tutorial.</p>
  </body>
</html>

```

Browser creates this tree:

```
Document
 └── html
      └── body
           ├── h1 → "Hello!"
           └── p  → "Welcome to the DOM tutorial."

```

### 🔹 Node Types

|Node Type|Example|Description|
|---|---|---|
|`Document`|`document`|Root of the DOM tree|
|`Element`|`<h1>`, `<p>`|HTML elements|
|`Text`|`"Hello!"`|Text inside elements|
|`Attribute`|`class="header"`|Attributes of elements|
|`Comment`|`<!-- comment -->`|Comment nodes|

---

## ⚙️ Accessing DOM Elements

There are multiple ways to find/select elements in the DOM.

|Method|Description|Example|
|---|---|---|
|`document.getElementById("id")`|Selects element by `id`|`document.getElementById("title")`|
|`document.getElementsByClassName("class")`|Returns HTMLCollection|`document.getElementsByClassName("btn")`|
|`document.getElementsByTagName("tag")`|Returns all elements with a tag name|`document.getElementsByTagName("p")`|
|`document.querySelector("selector")`|Selects **first** matching element|`document.querySelector("#title")`|
|`document.querySelectorAll("selector")`|Selects **all** matching elements|`document.querySelectorAll(".btn")`|

---

## ✍️ Reading & Modifying Content

|Property|Description|Example|
|---|---|---|
|`innerHTML`|Gets/sets HTML inside an element|`div.innerHTML = "<b>Hi</b>"`|
|`innerText`|Gets/sets plain text|`div.innerText = "Hello"`|
|`textContent`|Gets/sets all text (even hidden)|`div.textContent = "Hidden Text"`|

---

## 🎨 Changing Styles (CSS via DOM)

You can change any CSS property using `.style`.

```jsx
document.getElementById("title").style.color = "blue";
document.getElementById("title").style.backgroundColor = "yellow";

```

To add or remove CSS classes dynamically:

```jsx
let box = document.querySelector(".box");
box.classList.add("active");
box.classList.remove("hidden");
box.classList.toggle("dark-mode");

```

---

## 🧱 Creating and Removing Elements

### 🪄 Create new elements

```jsx
let newPara = document.createElement("p");
newPara.textContent = "This paragraph was added dynamically!";
document.body.appendChild(newPara);

```

### ❌ Remove elements

```jsx
let oldPara = document.getElementById("removeMe");
oldPara.remove();

```

---

## 🧩 Traversing the DOM Tree

You can move around the DOM like navigating a family tree.

|Property|Description|
|---|---|
|`parentNode`|Moves up to parent element|
|`childNodes`|Returns all children (includes text nodes)|
|`children`|Returns only element children|
|`firstChild`, `lastChild`|First and last node|
|`nextSibling`, `previousSibling`|Move between siblings|
|`nextElementSibling`, `previousElementSibling`|Skip text nodes, go to elements only|

Example:

```jsx
let div = document.getElementById("container");
let first = div.firstElementChild;

```

---

## 🖱️ Handling Events (Interaction)

Events are **actions** that happen on the page (like click, hover, keypress).

```html
<button id="btn">Click Me</button>

```

### Example 1: Using Event Listener

```jsx
document.getElementById("btn").addEventListener("click", function() {
  alert("Button was clicked!");
});

```

### Example 2: Changing Content Dynamically

```jsx
document.getElementById("btn").addEventListener("click", () => {
  document.body.style.backgroundColor = "lightblue";
});

```

---

## 📢 Common Event Types

|Event|Description|
|---|---|
|`click`|User clicks an element|
|`mouseover` / `mouseout`|Mouse enters/leaves element|
|`keydown` / `keyup`|Key pressed/released|
|`submit`|Form submission|
|`load`|Page loaded|
|`change`|Input value changed|
|`input`|Real-time typing detection|

---

## 🧠 Event Bubbling & Capturing

Events move through the DOM in two phases:

1. **Capturing phase** — from the top (document) down to the target.
2. **Bubbling phase** — from the target element back up.

By default, events **bubble up** (you can handle them at a parent level).

```jsx
document.body.addEventListener("click", function() {
  console.log("Body clicked!");
}, true); // true = capturing phase

```

---

## 🧰 Useful DOM Properties

|Property|Description|
|---|---|
|`document.title`|Get or set the title of the page|
|`document.URL`|Get the page URL|
|`document.forms`|Get all forms|
|`document.images`|Get all images|
|`document.links`|Get all links (`<a>` tags)|

---

## 🪞 Attributes

You can also read or modify HTML attributes.

```jsx
let img = document.querySelector("img");

// Get attribute
let src = img.getAttribute("src");

// Set attribute
img.setAttribute("alt", "Profile Picture");

// Remove attribute
img.removeAttribute("width");

```

---

## 💾 DOM Collections

Methods like `getElementsByTagName` return **HTMLCollections**, not arrays.

So you must convert them to arrays to use functions like `forEach`:

```jsx
let paras = document.getElementsByTagName("p");
Array.from(paras).forEach(p => console.log(p.textContent));

```

---

## ⚡ Performance Tips

- Use `documentFragment` for adding many elements (reduces reflow/repaint).
    
- Cache DOM selections:
    
    ```jsx
    const box = document.getElementById("box"); // reuse this instead of re-selecting
    
    ```
    
- Use `classList` instead of setting `.className` directly.
    
- Use `querySelector` for flexible selection.
    

---

## 🌐 Advanced Topics (When You’re Comfortable)

1. **MutationObserver** — watch for DOM changes automatically.
    
    ```jsx
    const observer = new MutationObserver(mutations => console.log(mutations));
    observer.observe(document.body, { childList: true });
    
    ```
    
2. **Shadow DOM** — used in Web Components to isolate styles and elements.
    
3. **Virtual DOM** — used by React/Vue (not the real browser DOM but a faster copy).
    
4. **DocumentFragment** — temporary container to reduce re-rendering.
    

---

## 🔥 Summary Table

| Concept  | What it Does            | Example                         |
| -------- | ----------------------- | ------------------------------- |
| Select   | Access elements         | `document.querySelector("#id")` |
| Modify   | Change content or style | `element.innerText = "Hi"`      |
| Add      | Create elements         | `createElement()`               |
| Remove   | Delete elements         | `remove()`                      |
| Traverse | Move around tree        | `parentNode`, `children`        |
| Events   | Add interactivity       | `addEventListener("click", fn)` |
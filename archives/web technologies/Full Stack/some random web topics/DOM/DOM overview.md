**DOM = Document Object Model**

When a web page loads, the **browser creates a model (a tree structure)** of the page called the **DOM**.  
JavaScript can use the DOM to **read**, **change**, or **add** anything on the page.

Think of it like this:

> The DOM is like a _blueprint_ of your webpage that JavaScript can use to manipulate every element (text, image, button, etc).

### 🧠 Example:

Let’s say you have a simple HTML page:

`<!DOCTYPE html> <html>   <body>     <h1 id="title">Hello, World!</h1>     <button id="btn">Click Me</button>      <script>       // JavaScript will go here     </script>   </body> </html>`

When this page loads, the browser builds this **DOM tree** 👇

`Document  └── html       └── body            ├── h1 (id="title")            └── button (id="btn")`

### 🧩 Accessing Elements (Reading the DOM)

JavaScript can **select** elements from the DOM using methods like:

|Method|Description|Example|
|---|---|---|
|`getElementById()`|Selects an element by ID|`document.getElementById("title")`|
|`getElementsByClassName()`|Selects elements by class|`document.getElementsByClassName("myClass")`|
|`getElementsByTagName()`|Selects by tag (like `<p>`, `<div>`)|`document.getElementsByTagName("p")`|
|`querySelector()`|Selects the **first** match (CSS style)|`document.querySelector("#title")`|
|`querySelectorAll()`|Selects **all** matches|`document.querySelectorAll("p")`|

---

### 🪄 Example: Changing Text

`document.getElementById("title").innerText = "Hello, Manoj!";`

✅ This will change  
`<h1>Hello, World!</h1>`  
to  
`<h1>Hello, Manoj!</h1>`

---

### 🎨 Example: Changing Style

`document.getElementById("title").style.color = "red";`

✅ Changes the text color of `<h1>` to red.

---

### 🧱 Example: Adding or Removing Elements

`// Create a new element let newPara = document.createElement("p"); newPara.innerText = "This is added using JavaScript!";  // Add it to the body document.body.appendChild(newPara);`

✅ This creates and adds a new paragraph at the end of the page.

---

### 🖱️ Example: Event Handling (Interactive DOM)

`document.getElementById("btn").addEventListener("click", function() {   document.getElementById("title").innerText = "Button was clicked!"; });`

✅ When you click the button, it changes the text of the `<h1>`.

---

### 🧠 In short:

|Concept|Meaning|
|---|---|
|**DOM**|Tree-like structure of an HTML document|
|**Node**|Each element, text, or attribute in the tree|
|**Manipulation**|Changing, adding, or removing elements|
|**Events**|User interactions like clicks, mouseover, etc.|

---

### 💡 Real-Life Analogy

Think of the **DOM as a map of a house**:

- Each **room** is an element (`div`, `p`, `button`).
    
- **JavaScript** is like a person walking through the house, turning on lights (changing styles), or moving furniture (adding/removing elements).
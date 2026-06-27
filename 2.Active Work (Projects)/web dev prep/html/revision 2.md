# HTML Interview Notes (Internship Revision)

## HTML

**Full Form:** HyperText Markup Language

**Purpose:** Defines the structure of a web page. It tells the browser what content exists (headings, paragraphs, images, forms, etc.).

---

# 1. Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Website</title>
</head>
<body>

</body>
</html>
```

## Tags

### `<!DOCTYPE html>`

- Declares the document as HTML5.
    
- Helps browsers render the page in standards mode.
    

---

### `<html>`

- Root element.
    
- Wraps the entire HTML document.
    

Example:

```html
<html lang="en">
```

`lang="en"` tells browsers and search engines that the page language is English.

---

### `<head>`

Contains metadata that is **not displayed** on the webpage.

Examples:

- Title
    
- CSS links
    
- JavaScript links
    
- Meta tags
    

---

### `<body>`

Contains everything visible on the webpage.

Examples:

- Text
    
- Images
    
- Forms
    
- Buttons
    
- Videos
    

---

# 2. Headings

```html
<h1>Main Heading</h1>
<h2>Section</h2>
<h3>Subsection</h3>
<h4>Heading</h4>
<h5>Heading</h5>
<h6>Smallest Heading</h6>
```

## Uses

`<h1>`

- Main title of the page.
    
- Usually only one per page.
    

`<h2>`

- Major sections.
    

`<h3>`

- Subsections.
    

Continue similarly until `<h6>`.

---

# 3. Paragraph

```html
<p>This is a paragraph.</p>
```

Used for normal text.

---

# 4. Line Break

```html
<br>
```

Moves text to the next line.

Example

```
Hello
World
```

---

# 5. Horizontal Line

```html
<hr>
```

Creates a horizontal divider.

Used to separate sections.

---

# 6. Strong

```html
<strong>Important</strong>
```

Purpose

- Important text
    
- Screen readers emphasize it
    
- Usually appears bold
    

---

# 7. Bold

```html
<b>Bold Text</b>
```

Purpose

- Makes text bold
    
- No semantic meaning
    

---

# 8. Emphasis

```html
<em>Important</em>
```

Purpose

- Adds emphasis
    
- Screen readers stress the word
    
- Usually italic
    

---

# 9. Italic

```html
<i>Italic</i>
```

Purpose

- Only changes appearance
    
- No semantic meaning
    

---

# 10. Mark

```html
<mark>Highlighted</mark>
```

Highlights text with a yellow background.

---

# 11. Small

```html
<small>Small Text</small>
```

Displays slightly smaller text.

Commonly used for:

- Copyright
    
- Notes
    
- Legal text
    

---

# 12. Lists

## Unordered List

```html
<ul>
    <li>Apple</li>
    <li>Mango</li>
</ul>
```

Creates bullet points.

---

## Ordered List

```html
<ol>
    <li>Step 1</li>
    <li>Step 2</li>
</ol>
```

Creates numbered lists.

---

## List Item

```html
<li>Item</li>
```

Represents one item inside `<ul>` or `<ol>`.

---

# 13. Links

```html
<a href="https://google.com">Google</a>
```

Creates hyperlinks.

Common attributes

`href`

- Destination URL.
    

`target="_blank"`

- Opens in a new tab.
    

`rel="noopener noreferrer"`

- Security best practice when opening new tabs.
    

---

# 14. Images

```html
<img src="cat.jpg" alt="Cat">
```

Attributes

`src`

- Image location.
    

`alt`

- Alternative text.
    
- Used for accessibility.
    
- Displayed if the image cannot load.
    

---

# 15. Tables

```html
<table>
<tr>
<th>Name</th>
<th>Age</th>
</tr>

<tr>
<td>John</td>
<td>22</td>
</tr>

</table>
```

Tags

`<table>`

- Creates a table.
    

`<tr>`

- Table Row.
    

`<th>`

- Table Header.
    

`<td>`

- Table Data (cell).
    

---

# 16. Forms

## Form

```html
<form>

</form>
```

Collects user input.

---

## Input

```html
<input type="text">
```

Creates an input field.

Common Types

```
text
email
password
number
date
checkbox
radio
file
submit
```

---

## Label

```html
<label>Name</label>
```

Connects text with an input.

Benefits

- Better accessibility.
    
- Larger clickable area.
    

---

## Textarea

```html
<textarea></textarea>
```

Multi-line text input.

Example

Feedback

Comments

Descriptions

---

## Select

```html
<select>

<option>India</option>

<option>USA</option>

</select>
```

Creates a dropdown.

---

## Option

```html
<option>Apple</option>
```

Represents one dropdown item.

---

## Button

```html
<button>Submit</button>
```

Creates clickable buttons.

---

# 17. Semantic HTML

## Header

```html
<header>
```

Top section of the webpage.

Contains

- Logo
    
- Navigation
    
- Website title
    

---

## Navigation

```html
<nav>
```

Contains navigation links.

---

## Main

```html
<main>
```

Main content of the webpage.

Only one per page.

---

## Section

```html
<section>
```

Groups related content.

Example

About

Services

Contact

---

## Article

```html
<article>
```

Independent content.

Examples

- Blog post
    
- News article
    
- Forum post
    

---

## Aside

```html
<aside>
```

Side content.

Examples

- Advertisements
    
- Related posts
    
- Sidebar
    

---

## Footer

```html
<footer>
```

Bottom section.

Contains

- Copyright
    
- Contact
    
- Social media links
    

---

# 18. Block Elements

Start on a new line.

Examples

```
<div>
<p>
<h1>
<section>
<header>
<footer>
```

---

# 19. Inline Elements

Stay on the same line.

Examples

```
<span>
<a>
<strong>
<em>
<img>
```

---

# 20. div

```html
<div>
```

Generic block container.

Used for grouping larger sections.

---

# 21. span

```html
<span>
```

Generic inline container.

Used to style or manipulate a small portion of text.

---

# 22. id

```html
<div id="header"></div>
```

Purpose

- Unique identifier.
    
- Only one element should have a particular id.
    
- Used by CSS and JavaScript.
    

---

# 23. class

```html
<div class="card"></div>
```

Purpose

- Reusable identifier.
    
- Multiple elements can share the same class.
    
- Mostly used for CSS styling.
    

---

# 24. Audio

```html
<audio controls>
```

Embeds audio.

---

# 25. Video

```html
<video controls>
```

Embeds video.

---

# 26. iframe

```html
<iframe src="..."></iframe>
```

Embeds another webpage.

Examples

- YouTube videos
    
- Google Maps
    
- Documents
    

---

# 27. Meta Tags

## Character Encoding

```html
<meta charset="UTF-8">
```

UTF = Unicode Transformation Format.

UTF-8 supports almost every language and special character.

---

## Viewport

```html
<meta
name="viewport"
content="width=device-width, initial-scale=1.0">
```

Makes the page responsive on mobile devices.

---

# Important Interview Differences

## `<strong>` vs `<b>`

|strong|b|
|---|---|
|Semantic|Visual only|
|Important text|Bold text|

---

## `<em>` vs `<i>`

|em|i|
|---|---|
|Semantic emphasis|Visual italic|

---

## `div` vs `span`

|div|span|
|---|---|
|Block element|Inline element|
|Large sections|Small text portions|

---

## `id` vs `class`

|id|class|
|---|---|
|Unique|Reusable|
|One element|Multiple elements|

---

# Full Forms

HTML → HyperText Markup Language

DOM → Document Object Model

SEO → Search Engine Optimization

UTF → Unicode Transformation Format

URL → Uniform Resource Locator

HTTP → HyperText Transfer Protocol

HTTPS → HyperText Transfer Protocol Secure

CSS → Cascading Style Sheets

JS → JavaScript
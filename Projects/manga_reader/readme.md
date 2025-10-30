# 💻 1. **Python Cheat Sheet (Basics for this project)**

### 📁 Files & Paths

```python
import os

# Current file path
os.path.dirname(__file__)

# Join paths safely
os.path.join('manga', 'Batman', 'comic.cbz')

# List all files/folders
os.listdir('manga')

# Check if folder or file
os.path.isdir(path)
os.path.isfile(path)

```

### ⚙️ Sorting & Filtering

```python
files = [f for f in os.listdir('folder') if f.endswith('.cbz')]
files.sort()  # Alphabetical

```

---

# 🧠 2. **Flask Cheat Sheet**

Flask = a *micro web framework* — it handles routes, responses, and HTML rendering.

### 🪄 Basic App

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello World"

app.run(port=5050)

```

### 📜 Dynamic Route

```python
@app.route('/category/<name>')
def category(name):
    return f"This is {name}"

```

### 🧾 Render Templates

```python
from flask import render_template

@app.route('/')
def home():
    return render_template('index.html', title="Homepage", items=['A', 'B'])

```

### 🔁 Template Folder Structure

```
project/
│
├── app.py
└── templates/
    ├── index.html
    └── comics.html

```

---

# 🧩 3. **Jinja2 (HTML Template Language)**

Jinja2 is the “brain” that lets HTML understand Python data.

### 🔁 Loop

```html
{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}

```

### 🧩 Conditional

```html
{% if items %}
  <p>There are comics available!</p>
{% else %}
  <p>No comics found.</p>
{% endif %}

```

### 🧮 Variables

```html
<h1>{{ title }}</h1>

```

### 🔗 URL with variable

```html
<a href="/category/{{ name }}">View</a>

```

---

# 🗂️ 4. **File Handling (for .CBZ and .CBR)**

### 📦 .CBZ → ZIP file

```python
import zipfile

with zipfile.ZipFile('comic.cbz', 'r') as cbz:
    cbz.extractall('output_folder')

```

### 📦 .CBR → RAR file

```python
import rarfile

with rarfile.RarFile('comic.cbr', 'r') as cbr:
    cbr.extractall('output_folder')

```

> ⚠️ Requires: pip install rarfile and unrar installed in system.
> 

---

# 🎨 5. **HTML Cheat Sheet (Frontend Basics)**

### 🏗️ Page Structure

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Comics</title>
</head>
<body>
  <h1>Welcome!</h1>
</body>
</html>

```

### 🧱 Lists & Links

```html
<ul>
  <li><a href="/category/Batman">Batman</a></li>
</ul>

```

### 🖼️ Images

```html
<img src="{{ url_for('static', filename='temp_pages/page_01.jpg') }}" alt="Page 1">

```

---

# 🎨 6. **CSS Cheat Sheet (Design & Layout)**

```css
body {
  background: #121212;
  color: #eee;
  font-family: sans-serif;
}

h1 {
  text-align: center;
}

ul {
  list-style: none;
  padding: 0;
}

li a {
  display: block;
  background: #222;
  color: #fff;
  padding: 10px;
  margin: 5px;
  border-radius: 8px;
  text-decoration: none;
}

li a:hover {
  background: #444;
}

```

---

# 🧳 7. **Tailscale Cheat Sheet (Private Hosting)**

You’ll host this Flask app inside your **private network** (like a LAN, but safer).

### 🔌 Steps:

1. Install Tailscale
2. Log in with your account (same on all devices)
3. Run Flask app:
    
    ```bash
    python app.py
    
    ```
    
4. Visit the Flask server using your **Tailscale IP**:
    
    ```
    http://100.x.x.x:5050/
    
    ```
    
    or use your **MagicDNS name**, e.g.
    
    `http://yourpcname.tailnet-yourname.ts.net:5050/`
    

✅ This means your comics are **100% private** — no need for public hosting.

---

# 🧹 8. **Optional: Cleanup Tools (later stage)**

We can later add:

- **`tempfile`** → to handle temporary directories safely
- **`shutil.rmtree()`** → to delete old extracted files
- **`threading.Timer`** → to auto-clean after inactivity

Example:

```python
import shutil, threading

def cleanup_temp():
    shutil.rmtree('static/temp_pages', ignore_errors=True)

threading.Timer(600, cleanup_temp).start()  # Every 10 mins

```

---

# 🧩 9. **Optional Enhancements**

| Feature | Library | Purpose |
| --- | --- | --- |
| 🧱 Database | SQLite (optional) | Store favorites or reading progress |
| 🖼️ Image Viewer | JavaScript (Lightbox.js) | Swipe between pages |
| 🧮 Image Processing | Pillow | Resize/optimize extracted images |
| 🧾 Config File | YAML / JSON | Custom settings (theme, paths, etc.) |

---

# 🧠 Summary

| Technology | What it does |
| --- | --- |
| **Python** | Main programming language |
| **Flask** | Runs the web app |
| **Jinja2** | Generates HTML from Python data |
| **os** | Reads folders/files |
| **zipfile / rarfile** | Opens comic archives |
| **HTML/CSS** | Frontend structure & design |
| **Tailscale** | Makes the app private & accessible on your devices |
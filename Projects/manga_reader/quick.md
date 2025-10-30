## 🧩 **Full Tech Stack Overview**

### 🖥️ **Backend**

| Tech | Purpose | Notes |
| --- | --- | --- |
| **Python** | Main programming language | Clean, easy, flexible for backend scripting |
| **Flask** | Web framework | Serves routes, HTML pages, and comic images |
| **os / pathlib** | File handling | To scan folders and manage file paths |
| **zipfile / rarfile** | Extract `.cbz` and `.cbr` archives | Convert to image sets on the fly |
| **Tailscale (optional)** | Secure access | Lets you view your local server privately over your network |

---

### 🌐 **Frontend**

| Tech | Purpose | Notes |
| --- | --- | --- |
| **HTML5** | Structure | Base layout for all pages |
| **CSS3 / Tailwind CSS** *(optional)* | Styling | Clean, responsive design |
| **JavaScript (Vanilla)** | Dynamic behavior | Handles navigation, image gallery switching |

---

### 🧱 **Templating**

| Tech | Purpose | Notes |
| --- | --- | --- |
| **Jinja2** (built into Flask) | Dynamic templates | Lets you reuse one layout (e.g., comic_view.html) for all comics |

---

### 🗂️ **File Structure**

| Folder/File | Purpose |
| --- | --- |
| `/app.py` | Main server script (handles routes) |
| `/static/` | Stores static assets like CSS/JS/images |
| `/templates/` | Contains HTML templates (Jinja2) |
| `/manga/` | Folder with your comics (ignored by Git) |
| `.gitignore` | Keeps your manga files private |
| `/utils.py` *(optional)* | File parsing helpers (CBR/CBZ extractors) |

---

### 🔒 **Privacy + Hosting**

| Tool | Use |
| --- | --- |
| **Tailscale** | Allows secure access to your local Flask app (VPN-based) |
| **Localhost** | You can run everything locally (`http://127.0.0.1:5000`) |
| **Custom Port** | Set in Flask: `app.run(host='0.0.0.0', port=8080)` |

---

### 🧠 **Cheat Sheet Summary**

| Category | Command / Concept |
| --- | --- |
| **Run Flask App** | `python app.py` |
| **Change Port** | `app.run(port=8080)` |
| **Access via Tailscale** | Visit your device’s Tailscale IP: `http://100.x.x.x:8080` |
| **Ignore Manga Folder** | Add `manga/` to `.gitignore` |
| **Template Variable in Jinja2** | `{{ variable_name }}` |
| **For Loop in Jinja2** | `{% for item in items %} ... {% endfor %}` |
| **Static File Ref** | `{{ url_for('static', filename='style.css') }}` |
| **Serving Images** | Flask `send_from_directory()` |

---

Would you like me to give you **one complete cheat sheet** for all the technologies (Python, Flask, Jinja2, HTML, CSS, JS) in one place next?

It’ll be like a single-page “everything you need for this project” guide.
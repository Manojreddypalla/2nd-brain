# Untitled

**Today, I was a little frustrated with Docker because I had forgotten everything, but I decided to rebuild my understanding by creating a Docker image and running a container. I chose my own project, ICNSIET, a website I created, and it turned out to be a great experience to relearn and improve my Docker skills.**

# docker file structure

```docker
# Use Node.js base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package.json and lockfile first (better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the code
COPY . .

# Build Astro project
RUN npm run build

# Expose the same port Astro uses (5000 from your logs)
EXPOSE 5000

# Run Astro preview in production mode
CMD ["npm", "run", "preview", "--", "--host", "--port", "5000"]

```

### 1. **Base Image**

```docker
FROM node:18-alpine

```

- **What it does:** Starts with Node.js 18 on Alpine Linux.
- **Why:**
    - Node.js 18 is stable LTS (Long-Term Support).
    - **Alpine** keeps the image very small (≈ 5MB base vs 100MB+ for full Debian).
    - Smaller images = faster downloads, fewer security risks.

---

### 2. **Working Directory**

```docker
WORKDIR /app

```

- **What it does:** All commands now run in `/app`.
- **Why:**
    - Keeps your project organized in one folder.
    - Prevents cluttering the container’s root (`/`).
    - Easier to debug and manage.

---

### 3. **Copy Only Package Files**

```docker
COPY package*.json ./

```

- **What it does:** Copies `package.json` and `package-lock.json`.
- **Why:**
    - Optimizes **Docker build caching**:
        - If you change source code but not dependencies, Docker won’t rerun `npm install`.
        - Saves build time (sometimes minutes).
    - This is a common **best practice** in Node.js Dockerfiles.

---

### 4. **Install Dependencies**

```docker
RUN npm install

```

- **What it does:** Installs dependencies inside the container.
- **Why:**
    - Ensures dependencies are available for the app to build and run.
    - Keeps `node_modules` inside the container (not mixed with your local machine).
    - Makes the container **self-contained** — anyone can run it without installing Node/npm locally.

---

### 5. **Copy Rest of the Code**

```docker
COPY . .

```

- **What it does:** Copies all your project files into `/app`.
- **Why:**
    - Needed to actually build and run your Astro project.
    - Done **after `npm install`** so that changes in code don’t force reinstalling dependencies.

---

### 6. **Build the Project**

```docker
RUN npm run build

```

- **What it does:** Runs Astro’s build step → outputs static production files.
- **Why:**
    - Ensures the app is **ready for production** before running.
    - Avoids building at container startup (faster startup, fewer runtime errors).
    - Produces optimized HTML/CSS/JS (smaller, faster for users).

---

### 7. **Expose Port**

```docker
EXPOSE 5000

```

- **What it does:** Documents that the container listens on port 5000.
- **Why:**
    - Makes it clear to anyone using this image which port to use.
    - Tools like Docker Compose can auto-detect this.
    - Without this, you might forget which port your app runs on.

---

### 8. **Default Command**

```docker
CMD ["npm", "run", "preview", "--", "--host", "--port", "5000"]

```

- **What it does:** Runs Astro’s preview server on port 5000, bound to all network interfaces.
- **Why:**
    - `npm run preview` → serves the built site (like a mini production server).
    - `-host` → makes it accessible **outside** the container (otherwise only localhost inside).
    - `-port 5000` → ensures consistency with your logs and `EXPOSE`.
    - Using `CMD` lets you **override it** when running (`docker run ...`) if needed.

---

✅ **Reasons for overall design:**

- Uses **small image** (faster, secure).
- Separates **dependency install from code copy** (caching efficiency).
- Builds the app **ahead of time** (fewer runtime surprises).
- Exposes the **correct port** for clarity.
- Runs in **production mode** (not dev server).

### 

### **1️⃣ Check running containers**

```bash
docker ps

```

- Shows **only running containers**.

---

### **2️⃣ Check all containers (including stopped)**

```bash
docker ps -a

```

- Lists **all containers**, including exited ones.

---

### **3️⃣ Start a stopped container interactively**

```bash
docker start -ai <container-id>

```

Example:

```bash
docker start -ai 209db1c2fe1a

```

- `a` = attach to container logs
- `i` = keep STDIN open (interactive)
- Used to **see why container exited**.

---

### **4️⃣ Open a shell inside the container**

```bash
docker run -it <image-name> sh

```

Example:

```bash
docker run -it astro-app sh

```

- `it` = interactive terminal
- `sh` = open shell instead of running default command
- Useful for **debugging inside the container**.

---

### **5️⃣ Run dev server inside container**

Inside container shell:

```bash
npm run dev

```

- Starts **Astro dev server** (works with Netlify adapter, unlike `preview`)
- Access via `http://localhost:5000` (with port mapping)

---

### **6️⃣ Run container directly with dev server and port mapping**

```bash
docker run -it -p 5000:5000 astro-app npm run dev

```

- `p 5000:5000` → maps container port 5000 to host
- Runs **dev server directly** without entering container
# docker file

Here’s a simple Dockerfile for a **Node.js / Astro project in dev mode**:

```docker
# Use Node.js base image
FROM node:18-alpine

# Set working directory inside container
WORKDIR /app

# Copy dependency files first (better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of your project
COPY . .

# Expose the port that Astro dev server uses (default 4321)
EXPOSE 4321

# Run Astro in dev mode, binding to all interfaces so Docker can access it
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]

```

### ✅ Notes:

1. `FROM node:18-alpine` → lightweight Node.js base image.
2. `WORKDIR /app` → sets the working folder inside container.
3. `COPY package*.json ./` and `RUN npm install` → only install dependencies first (faster caching).
4. `COPY . .` → copy your source code into the container.
5. `EXPOSE 4321` → tells Docker the app uses this port.
6. `CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]` → starts Astro in dev mode and allows access from host.

## **2️⃣ Docker Commands to Build and Run**

### **Step 1: Build the Docker image**

```powershell
docker build -t my-astro-app .

```

- `t my-astro-app` → name/tag of your image.
- `.` → current folder (Dockerfile location).

Check the image:

```powershell
docker images

```

---

### **Step 2: Run the container**

Map the container port to your host:

```powershell
docker run -d -p 9090:4321 --name my-astro-container my-astro-app

```

- `d` → detached (background).
- `p 9090:4321` → host port 9090 → container port 4321.
- `-name` → container name.

Access in browser:

```
http://localhost:9090

```

---

### **Step 3: Check running containers**

```powershell
docker ps

```

---

### **Step 4: View logs (optional)**

```powershell
docker logs -f my-astro-container

```

---

### **Step 5: Stop / Remove container**

```powershell
docker stop my-astro-container
docker rm my-astro-container

```
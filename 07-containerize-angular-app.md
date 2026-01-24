# Dockerizing an Angular App (with Common Pitfalls Explained)

This tutorial documents a **real-world Docker learning journey** while containerizing an Angular application. Instead of hiding problems, we intentionally highlight common mistakes and explain *why* they happen and *how* to fix them.

---

## Prerequisites

* Node.js installed locally (for understanding, not mandatory to run Docker)
* Docker Desktop (Windows / Mac / Linux)
* Basic Angular knowledge
* Basic Docker concepts (image, container)

---

## Project Overview

* **Frontend**: Angular app
* **Backend (Mock API)**: `json-server`
* **Ports**:

  * Angular → `4200`
  * json-server → `3200`

The app fetches posts from:

```
GET /posts
```

---

## Step 1: Project Setup

Repository:

```
https://github.com/neutral-00/practice-angular
```

Branch:

```
16-template-forms
```

The app uses:

* `pnpm`
* Angular dev server
* `json-server` for mock APIs

---

## Step 2: `.dockerignore`

Create a `.dockerignore` file:

```text
node_modules
dist
```

This keeps the Docker image small and avoids conflicts.

---

## Step 3: Writing the Dockerfile

```dockerfile
# Use Node + pnpm base image
FROM guergeiro/pnpm:22-10

# Set working directory
WORKDIR /app

# Copy project files
COPY . .

# Install dependencies
RUN pnpm install

# Expose Angular and API ports
EXPOSE 4200
EXPOSE 3200

# Run Angular + json-server together
CMD ["pnpm", "run", "dev"]
```

---

## Step 4: Understanding the `package.json` scripts

```json
"scripts": {
  "start": "ng serve --host 0.0.0.0 --port 4200",
  "server": "json-server --watch db.json --host 0.0.0.0 --port 3200",
  "dev": "concurrently \"pnpm start\" \"pnpm run server\""
}
```

### Why `--host 0.0.0.0` is critical

Inside Docker:

* `localhost` → container itself
* Docker port mapping works **only if the service listens on all interfaces**

Using `0.0.0.0` allows Docker to forward traffic correctly.

---

## Step 5: Building the Image

```bash
docker build -t angular-blogger-app .
```

> Any time you change application code or scripts, you **must rebuild the image**.

---

## Step 6: Running the Container

```bash
docker run -d --rm \
  --name blogger \
  -p 4200:4200 \
  -p 3200:3200 \
  angular-blogger-app
```

---

## Step 7: First Common Pitfall – App loads but not accessible

### Symptom

* Container runs
* No errors
* `http://localhost:4200` does not load

### Cause

Angular was binding to `localhost` **inside the container**.

### Fix

```bash
ng serve --host 0.0.0.0
```

---

## Step 8: Second Common Pitfall – API calls fail

### Symptom

* Angular UI loads
* API calls to `http://localhost:3200/posts` fail

### Root Cause

Inside Docker:

```
localhost === same container
```

Angular was calling the API correctly, but `json-server` was not reachable because it was bound incorrectly.

### The Fix (Chosen Approach)

Run `json-server` with:

```bash
--host 0.0.0.0
```

Now both services communicate **inside the same container**.

---

## Mental Model: Docker Networking (Very Important)

```
Browser (Host)
  │
  ├── localhost:4200 → Angular (Container)
  │
  └── localhost:3200 → json-server (Container)
```

Inside the container:

```
Angular → localhost:3200 → json-server
```

---

## Rule of Thumb (Remember This)

> * `localhost` inside Docker = the container
> * `0.0.0.0` = allow external access
> * Rebuild image after code changes
> * Port mapping only works if the app listens correctly

---

## What This Setup Is (and Is Not)

✅ Perfect for:

* Learning Docker
* Local development
* Understanding container networking

❌ Not ideal for:

* Production builds
* Performance
* Security

---

## Next Steps

* Convert this into **Docker Compose**
* Create a **production Angular + Nginx image**
* Split frontend and API into separate containers

---

## Final Takeaway

Docker issues are rarely "Docker bugs".

They are usually:

* Incorrect assumptions about `localhost`
* Missing host bindings
* Mental model mismatches

Once you understand **container boundaries**, everything clicks 🚀

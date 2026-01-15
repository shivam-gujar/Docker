# 🐳 Docker Multistage Builds & Distroless Images (Beginner Friendly)

This README explains **Multistage Docker Builds** and **Distroless Images** in a simple, practical way.

No hype. No theory overload. Just what you actually need to understand and use them correctly.

---

## 1. What is a Multistage Docker Build?

A **multistage build** is a Docker feature that allows you to use **multiple `FROM` statements** in a single Dockerfile.

Each `FROM` creates a **new stage**.

### Core idea:

* Use **heavy images** to *build* the application
* Use **lightweight images** to *run* the application

This drastically reduces image size and improves security.

---

## 2. Why Multistage Builds Are Needed

### Normal (Bad) Build Problem

In a normal Docker build:

* You use large images like `golang`, `node`, `python`
* Final image contains:

  * Compilers
  * Build tools
  * Package managers
  * Source code
  * Cache & temp files

### Result:

❌ Huge image size
❌ Slower deployments
❌ Bigger attack surface

### Solution → Multistage Builds

Build with everything → run with **only what is needed**.

---

## 3. How Multistage Builds Work (Simple)

Example structure:

```dockerfile
FROM golang:1.21 AS builder
# build the app

FROM ubuntu:22.04 AS runner
# run the app
```

### What happens:

* **Stage 1 (builder)** compiles the app
* **Stage 2 (runner)** copies only the final output
* Build tools never reach the final image

---

## 4. How Multistage Builds Work in Production

### Production Flow:

1. CI/CD builds Dockerfile
2. Stage 1 compiles app using full SDK
3. Stage 2 contains only runtime files
4. CI pushes **small final image** to registry
5. Servers / Kubernetes pull fast

### Why production prefers multistage:

* Smaller image → faster deploys
* Smaller image → faster scaling
* Fewer packages → fewer CVEs
* Cleaner security audits

---

## 5. Advantages of Multistage Builds

✅ Smaller Images
✅ More Secure
✅ Cleaner Dockerfiles
✅ Faster Deployments
✅ Ideal for Go, Java, Node, Python, React, Angular

---

## 6. What Are Distroless Images?

A **distroless image** contains:

* Only your application
* Only required runtime libraries

### What it does NOT contain:

* ❌ Shell (`/bin/sh`)
* ❌ Package manager
* ❌ OS tools
* ❌ Bash
* ❌ Debug utilities

Example:

```dockerfile
FROM gcr.io/distroless/python3
```

Distroless images are created by Google.

---

## 7. How Distroless Images Work (Internally)

### Normal Linux Image Contains:

* OS packages
* Shell & utilities
* Debugging tools
* Package managers

### Distroless Image Contains:

* Application
* Minimal runtime

Nothing extra.

---

---

## 9. Multistage Build Example (Go App)

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o app

# Stage 2: Run
FROM ubuntu:22.04
WORKDIR /app
COPY --from=builder /app/app .
CMD ["./app"]
```

## 10. Multistage + Distroless (Go Production)

```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o app

# Stage 2: Distroless
FROM gcr.io/distroless/base
COPY --from=builder /app/app /app
CMD ["/app"]
```

Final image size: **~8 MB**

---

## 11. Multistage Build Example (Python App)

```dockerfile
# Stage 1: Builder
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt
COPY . .

# Stage 2: Runner
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /install /usr/local
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## 12. Multistage + Distroless (Python)

```dockerfile
# Stage 1: Build
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt
COPY . .

# Stage 2: Distroless
FROM gcr.io/distroless/python3
COPY --from=builder /install /usr/local
COPY app.py /app/app.py
WORKDIR /app
CMD ["app.py"]
```


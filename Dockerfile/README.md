# Dockerfile – README

This README explains **what a Dockerfile is**, **how to read it**, and **how to write one from scratch**. No fluff. If you can’t explain each line, you don’t understand Docker yet.

---

## What is a Dockerfile?

A **Dockerfile** is a step-by-step instruction file that tells Docker:

* which base environment to start from
* what files to copy
* what commands to run
* how to start the application

Docker reads it **top to bottom** and builds an image layer by layer.

---

## Basic Dockerfile Structure

```dockerfile
FROM base-image
WORKDIR /app
COPY . .
RUN command
CMD ["executable", "arg"]
```

If you can’t explain why each line exists, stop copying Dockerfiles from GitHub.

---

## Example 1: Java Application Dockerfile

```dockerfile
# 1. Base image (Java already installed)
FROM eclipse-temurin:17-jdk-alpine

# 2. Set working directory inside container
WORKDIR /app

# 3. Copy source code
COPY src/Main.java .

# 4. Compile Java code
RUN javac Main.java

# 5. Run the application
CMD ["java", "Main"]
```

### Line-by-line truth

* `FROM`: You are NOT installing Java yourself. This image already has it.
* `WORKDIR`: Avoid absolute paths everywhere. Keeps things clean.
* `COPY`: Copies from **host → container**, not the other way.
* `RUN`: Executes at **build time**.
* `CMD`: Runs at **container start time**.

---

## Example 2: Python Flask App

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

### Key points you must remember

* Dependencies are installed **before** copying full code (build cache optimization)
* `EXPOSE` is documentation, not port mapping
* `CMD` should be ONE per Dockerfile

---

---

## How to Build and Run

```bash
docker build -t my-app .
docker run -p 8080:8080 my-app
```
---

## Very Simple Mental Model (Beginner Friendly)

Think like this:

* Image = Blueprint (read-only)
* Container = Running instance of that blueprint
* Dockerfile = Recipe to create the blueprint


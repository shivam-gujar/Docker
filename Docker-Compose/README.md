# Docker Compose Setup

This repository contains a **Docker Compose configuration** to run a multi-container application locally in a simple, reproducible way.

The goal of this setup is to:

* Define all services in one place
* Manage networking automatically
* Start the entire stack with a single command

---

## 📦 Prerequisites

Make sure you have the following installed:

* Docker (v20+ recommended)
* Docker Compose (v2+ recommended)

Verify installation:

```bash
docker --version
docker compose version
```

---

## 📁 Project Structure

```text
.
├── docker-compose.yml
├── Dockerfile        # (if applicable)
├── .env              # (optional environment variables)
└── README.md
```

---

## 🚀 Services Overview

The `docker-compose.yml` file defines multiple services that work together.

Typical examples include:

* **Application service** (backend / frontend)
* **Database service** (MySQL / PostgreSQL / MongoDB)
* **Supporting services** (Redis, Nginx, etc.)

Each service runs in its own container and communicates over a shared Docker network.

---

## ⚙️ How It Works

* Docker Compose creates a **default bridge network**
* Services discover each other using **service names as hostnames**
* Volumes (if defined) persist data outside containers
* Environment variables configure runtime behavior

---

## ▶️ Running the Application

Start all services:

```bash
docker compose up
```

Start in detached mode:

```bash
docker compose up -d
```

---

## ⏹️ Stopping the Application

Stop containers (without removing data):

```bash
docker compose down
```

Stop and remove volumes:

```bash
docker compose down -v
```

---

## 🔍 Useful Commands

View running containers:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs
```

Rebuild images:

```bash
docker compose up --build
```


# Dockerfile Basics

A Dockerfile is a text file that contains instructions to build a Docker Image.

When we execute:

```bash
docker build -t my-app .
```

Docker:

- Reads the Dockerfile line by line.
- Executes each instruction.
- Creates a Docker Image.

> **Note:** `docker build` prepares the image; it does not run the application.

## 1. FROM

### Syntax
```dockerfile
FROM ubuntu
```
or
```dockerfile
FROM openjdk:21
```

### Purpose
- Specifies the base image for the new Docker image.
- It is always the first instruction in a Dockerfile.

### What happens internally?
- Docker checks whether the base image already exists locally.
- If it exists, Docker reuses it.
- Otherwise, Docker downloads it from Docker Hub (or another registry).
- Docker uses the base image as the foundation for the new image.

### Important Points
- Docker does not create Ubuntu from scratch.
- Docker reuses the existing base image.
- Multiple Docker images can share the same base image.

### Interview Definition
> FROM specifies the base image that Docker uses as the starting point for building a new Docker image.

## 2. WORKDIR

### Syntax
```dockerfile
WORKDIR /app
```

### Purpose
Sets the working directory for all subsequent Dockerfile instructions.

### What happens internally?
If `/app` does not exist:
- Docker automatically creates it.

It is similar to executing:
```bash
mkdir -p /app
cd /app
```

### Example
```dockerfile
WORKDIR /app

COPY app.jar .
```
is equivalent to
```dockerfile
COPY app.jar /app
```

### If WORKDIR is not specified
Docker uses the default working directory of the base image.

Since this may vary between images, using WORKDIR is considered a best practice.

### Interview Definition
> WORKDIR sets the working directory for subsequent Dockerfile instructions. If the directory does not exist, Docker creates it automatically.

## 3. COPY

### Syntax
```dockerfile
COPY <source> <destination>
```

Example:
```dockerfile
COPY app.jar .
```

### Purpose
Copies files or directories from the local build context into the Docker image.

### What happens internally?

Suppose the project contains:
```
shopping-app/
├── Dockerfile
└── app.jar
```

Docker copies:
```
Local Machine
app.jar

↓

Docker Image
/app/app.jar
```

### Important Points
- COPY copies files.
- It does not move them.
- The original files remain on your local machine.

### Interview Definition
> COPY copies files or directories from the local build context into the Docker image during the image build process.

## 4. ENTRYPOINT

### Syntax
```dockerfile
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Purpose
Specifies the command that Docker should automatically execute when a container starts.

### What happens during docker build?
Docker does not execute the command.

Instead, it stores it as metadata inside the Docker image.
```
Docker Image
│
├── app.jar
└── Startup Command
    java -jar app.jar
```

### What happens during docker run?
Docker:
- Creates a container.
- Reads the stored ENTRYPOINT.
- Executes:
```
java -jar app.jar
```

Now the application starts.

### Why use the square brackets?
```dockerfile
ENTRYPOINT ["java", "-jar", "app.jar"]
```

This is the Exec Form (recommended).

Docker directly starts:
```
java -jar app.jar
```
without creating an intermediate shell.

### Interview Definition
> ENTRYPOINT specifies the command that Docker automatically executes whenever a container is started from the image. It is stored during docker build and executed during docker run.

## Complete Flow

```
Dockerfile
    │
    │ docker build
    ▼
Read Dockerfile line by line
    │
    ├── FROM       → Use/Create Base Image
    ├── WORKDIR    → Set Working Directory
    ├── COPY       → Copy Files into Image
    ├── ENTRYPOINT → Store Startup Command
    │
    ▼
Docker Image
    │
    │ docker run
    ▼
Docker Container
    │
    ▼
Application Starts
```

## Interview Revision

**What is a Dockerfile?**
> A Dockerfile is a text file containing instructions to build a Docker Image.

**What does docker build do?**
> Reads the Dockerfile, executes each instruction, and creates a Docker Image.

**What does FROM do?**
> Specifies the base image for building a new Docker Image.

**What does WORKDIR do?**
> Sets the working directory for subsequent Dockerfile instructions.

**What does COPY do?**
> Copies files from the local build context into the Docker Image.

**What does ENTRYPOINT do?**
> Specifies the command that is automatically executed when a container starts.

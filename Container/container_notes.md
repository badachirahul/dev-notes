# Introduction to Containers

## Why were Containers introduced?

Containers were introduced to solve some of the limitations of Virtual Machines and traditional application deployment.

### Problem with Virtual Machines

- Each Virtual Machine has its own complete Operating System.
- Multiple VMs on the same physical server consume more CPU, RAM, and storage.
- In many cases, applications do not fully utilize the resources allocated to their VMs, leading to resource wastage.
- **"It works on my machine" problem:** An application may work on a developer's machine but fail on another developer's machine, the testing environment, or the production server due to differences in dependencies, libraries, operating systems, or configurations.

### Solution

Containers package the application along with its required dependencies, ensuring that the same container runs consistently across development, testing, and production environments while making better use of system resources.

---

## What is a Container Image?

A **container image** is the lightweight, standalone, executable **package** that contains everything needed to run an application:

- Application code
- Application Dependencies
- System Dependencies / libraries
- Configuration

A container image is static and portable — it can be stored in a registry and shipped across environments. A **container** is created by running an instance of this image, similar to how an object is an instance of a class.

---

## What is a Container?

A **container** is a isolated, running instance of an application created from a container image. 
- It packages together:
     - Application
     - Application Dependencies
     - System Dependencies

Unlike a Virtual Machine, a container **does not have a complete Operating System**. Instead, it shares the **Host Operating System kernel**, using kernel features like namespaces (isolation) and cgroups (resource limits) to run in its own isolated environment.

---

## What is Docker?

Docker is a containerization platform that enables us to create, run, and manage containers.

### What is Containerization?

- Containerization is the process of creating and running applications inside containers.

---

## Container Architecture

```
Physical Server / Virtual Machine
          │
   Host Operating System
          │
     Docker (Container Runtime)
          │
 ┌────────┼────────┐
 │        │        │
Container1 Container2 Container3
   │          │          │
 Application Application Application
```

- Multiple containers can run on the same VM or Physical Server.
- All containers share the Host Operating System.

---

## Virtual Machine vs Container

| Virtual Machine | Container |
| --- | --- |
| Has a complete Operating System | Shares the Host Operating System |
| Heavier | Lightweight |
| Higher resource usage | Lower resource usage |
| Slower startup | Faster startup |
| Stronger isolation | Lightweight isolation |

---

<br>

# Docker Image Optimization Notes

## 1. Single-stage Docker Build

### Definition

A **Single-stage Docker Build** uses **one Docker image** for both:

- Building the application
- Running the application

Everything remains inside the final image.

### Example

```dockerfile
FROM maven:3.9-eclipse-temurin-17

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

ENTRYPOINT ["java","-jar","target/blog-app.jar"]
```

### What happens?

```
Linux
│
├── Java JDK
├── Maven
├── Source Code
├── pom.xml
├── Build Files
├── target/
└── blog-app.jar
```

All these remain in the final image.

### Advantages

- Easy to write.
- Good for learning.
- Good for quick testing.

### Disadvantages

- Large image size.
- Build tools remain in the image.
- Source code is included.
- More security vulnerabilities.
- Slower image download/upload.

### Interview Definition
> A Single-stage Docker Build uses a single image to build and run an application. Since build tools and source code remain in the final image, the image becomes larger and less optimized.

## 2. Multi-stage Docker Build

### Definition

A **Multi-stage Docker Build** uses **multiple build stages**.

Typically:

- Stage 1 → Build the application.
- Stage 2 → Run the application.

Only the required artifacts (such as the JAR) are copied into the final image.

### Example

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre

WORKDIR /app

COPY --from=build /app/target/blog-app.jar .

ENTRYPOINT ["java","-jar","blog-app.jar"]
```

### Stage 1 (Build Stage)

Contains

```
Linux

Java JDK

Maven

Source Code

↓

Build blog-app.jar
```

### Stage 2 (Runtime Stage)

Contains

```
Linux

Java JRE

blog-app.jar
```

### How it works

```
Stage 1

Build Application

↓

Generate JAR

↓

Copy JAR

↓

Stage 2

↓

Discard Stage 1

↓

Final Image
```

### Advantages

- Smaller image.
- More secure.
- Faster deployment.
- Faster download/upload.
- Production-ready.

### Interview Definition
> A Multi-stage Docker Build separates the build environment from the runtime environment. The application is built in one stage and only the required artifacts are copied to the final runtime image.

## 3. Image Size Optimization

### Why optimize images?

Large images cause

- Slow downloads
- Slow uploads
- Slow deployments
- Higher storage usage
- Increased security risks

### Example

**Single-stage**
```
379 MB
```

Contains

```
Linux

Java JDK

Maven

Source Code

Build Files

Application
```

**Multi-stage**
```
176 MB
```

Contains

```
Linux

Java JRE

Application
```

### How Multi-stage reduces size

It removes

- Maven
- JDK
- Source Code
- Build Cache
- Build Files

Only the application is copied to the runtime image.

### Best Practices

- Use Multi-stage Builds.
- Use JRE instead of JDK for runtime.
- Use `.dockerignore`.
- Use Distroless Images when appropriate.

## 4. Distroless Images

### Definition

A **Distroless Image** is a minimal container image that contains **only the runtime and the libraries required to run the application**.

It removes unnecessary operating system utilities.

### Example

Instead of

```dockerfile
FROM eclipse-temurin:17-jre
```

Use

```dockerfile
FROM gcr.io/distroless/java17-debian12
```

### Normal Runtime Image

```
Linux
│
├── bash
├── sh
├── apt
├── vim
├── curl
├── grep
└── Java Runtime
```

### Distroless Image

```
Java Runtime

Required System Libraries

Application
```

### What is removed?

- bash
- sh
- apt
- curl
- vim
- nano
- grep
- Package Managers
- Debugging Tools

### Why?

Because the application never uses them.

### Advantages

- Smaller image.
- Better security.
- Fewer attack vectors.
- Faster deployment.

### Does Distroless have an OS?

**Yes.**

It contains a **minimal Linux userspace** and the libraries required by the application.

It does **not** contain a complete Linux distribution with common user-space tools.

### Distroless vs `scratch`

**Distroless**

Contains

- Runtime (Java, Node.js, etc.)
- Required libraries
- Minimal Linux userspace

Application can run.

**`FROM scratch`**

Contains

Nothing.

Only suitable for applications that are completely self-contained (for example, many statically compiled Go binaries).

A normal Java application cannot run directly from `scratch` because it requires a JVM and supporting libraries.

## Single-stage vs Multi-stage

| Feature                    | Single-stage | Multi-stage               |
| -------------------------- | ------------ | ------------------------- |
| Build & Run                | Same Image   | Separate Stages           |
| Maven in Final Image       | ✅ Yes        | ❌ No                      |
| Source Code in Final Image | ✅ Yes        | ❌ No                      |
| JDK in Final Image         | ✅ Yes        | ❌ No (typically JRE only) |
| Image Size                 | Large        | Small                     |
| Security                   | Lower        | Higher                    |
| Production Use             | Rare         | Very Common               |

## Runtime Image vs Distroless

| Feature         | JRE Image | Distroless  |
| --------------- | --------- | ----------- |
| Java Runtime    | ✅         | ✅           |
| Linux Userspace | ✅         | ✅ (minimal) |
| bash            | ✅         | ❌           |
| sh              | ✅         | ❌           |
| apt             | ✅         | ❌           |
| curl            | ✅         | ❌           |
| vim             | ✅         | ❌           |
| Image Size      | Small     | Smaller     |
| Security        | Good      | Better      |

## Interview Questions

**What is a Single-stage Docker Build?**
> A Single-stage Docker Build uses one image to build and run an application, so build tools and source code remain in the final image.

**What is a Multi-stage Docker Build?**
> A Multi-stage Docker Build separates the build and runtime environments. The application is built in one stage, and only the required artifacts are copied into the final runtime image.

**Why use Multi-stage Docker Builds?**

- Reduce image size.
- Improve security.
- Remove build tools.
- Remove source code.
- Improve deployment speed.

**What is a Distroless Image?**
> A Distroless Image is a minimal container image that contains only the runtime and the libraries required by the application. It excludes shells, package managers, and other unnecessary operating system utilities.

**Difference between Distroless and `scratch`**

| Distroless                          | `scratch`                                  |
| ------------------------------------ | ------------------------------------------- |
| Minimal runtime available           | Completely empty base image                |
| Suitable for Java applications      | Not suitable for a normal Java application |
| Contains required runtime libraries | Contains nothing                           |

<br>

# Docker Storage (Bind Mounts & Volumes)

## Why do we need Persistent Storage?

By default, **containers are ephemeral (short-lived)**.

When a container is deleted or recreated, **all the data stored inside the container is also lost**, which is a problem for applications that need to preserve data.

---

## Problems Without Persistent Storage

### 1. Log Files are Lost

Suppose an **Nginx container** stores user login information and access logs.

```
Nginx Container
       │
       ├── access.log
       └── error.log
```

If the container is deleted:

- Log files are deleted.
- Previous user activity cannot be tracked.
- Security auditing becomes impossible.

### 2. Data Sharing Between Containers

```
Backend Container
        │
 Writes HTML / JSON Files
        │
        ▼
Frontend Container
 Reads those files
```

If the backend container is deleted:

- Generated files are lost.
- Frontend cannot access previous data.

### 3. Accessing Host Files

Sometimes an application needs to read files already present on the host machine.

```
Host Machine
      │
 Creates report.json
      │
      ▼
Container
 Reads report.json
```

By default, a container **cannot directly access the host's filesystem**.

---

## Persistent Storage

Persistent Storage allows data to remain available **even after a container is stopped, removed, or recreated**.

Docker provides two ways to achieve this:

1. Bind Mounts
2. Docker Volumes

---

## Bind Mounts

### Definition

A **Bind Mount** connects a directory on the **Host Machine** to a directory inside the **Container**.

### Working

```
Host Machine
/app
      │
      ▼
Container
/app
```

Both directories point to the same storage. If a file is created in either location, it is visible from both.

### Advantage

Even if the container is deleted, the host directory still contains all the data:

```
Host
└── /app
```

A new container can be attached to the same directory.

---

## Docker Volumes

### Definition

A **Docker Volume** is a Docker-managed storage area used to persist container data.

Instead of manually choosing a host directory, Docker creates and manages the storage.

### Working

```
Docker Volume
       │
       ▼
Container
/app
```

Docker decides where the volume is stored on the host.

### Advantages

- Data persists after container deletion.
- Docker manages the storage.
- Easy to create, inspect, and delete using Docker commands.
- Can be attached to multiple containers.
- Can use external storage if required.

---

## Bind Mounts vs Docker Volumes

| Bind Mount                               | Docker Volume                           |
| ----------------------------------------- | ---------------------------------------- |
| Uses a directory on the Host Machine.    | Uses Docker-managed storage.            |
| User specifies the host directory.       | Docker creates and manages the storage. |
| Directly depends on the host filesystem. | Managed through Docker CLI.             |

---

## Docker Volume Lifecycle

Docker provides commands to manage the complete lifecycle of a volume.

### Create

```bash
docker volume create <volume-name>
```

### List

```bash
docker volume ls
```

### Inspect

```bash
docker volume inspect <volume-name>
```

Shows details such as:

- Volume name
- Driver
- Mount point
- Scope

### Delete

```bash
docker volume rm <volume-name>
```

A volume **cannot be deleted while it is attached to a running container**.

---

## Mounting a Volume

Volumes can be attached while creating a container.

```bash
docker run --mount source=my-volume,target=/app nginx
```

- **source** → Docker Volume.
- **target** → Directory inside the container.

---

## Inspecting Mounted Volumes

To view the volumes attached to a container:

```bash
docker inspect <container-name>
```

This displays:

- Source
- Destination
- Mount type
- Read/Write mode

---

## `-v` vs `--mount`

Docker supports two ways to mount storage.

### `-v`

Short syntax.

```bash
docker run -v my-volume:/app nginx
```

### `--mount`

Verbose and more readable syntax.

```bash
docker run \
  --mount source=my-volume,target=/app \
  nginx
```

`--mount` is recommended because the parameters are explicit and easier to understand.

---

## Important Points

- Containers are **ephemeral**, so data inside them is lost when they are removed.
- Persistent Storage keeps data safe beyond the lifecycle of a container.
- Docker provides **Bind Mounts** and **Volumes** for persistent storage.
- **Bind Mounts** map a host directory to a container directory.
- **Volumes** are managed by Docker and are the preferred approach in most cases.
- Docker Volumes can be created, listed, inspected, and deleted using Docker commands.
- A volume cannot be deleted while it is in use by a container.

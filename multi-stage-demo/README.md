
# Multi-Stage Docker Build Practice

## Objective
To understand the difference between a normal Docker build and a multi-stage Docker build and compare their image sizes.

## Project Structure
```text
multi-stage-demo/
├── app.py
├── Dockerfile
├── Dockerfile.multi
└── README.md
```

## 1. Python Application

**File:** `app.py`

```python
print("Hello! My multi-stage Docker build is working!")
```

This application prints a message when executed.

## 2. Normal Docker Build

**File:** `Dockerfile`

```dockerfile
FROM python:3.14

WORKDIR /app

COPY . .

CMD ["python", "app.py"]
```

### Explanation
- `FROM python:3.14` — Uses a base image containing Python.
- `WORKDIR /app` — Sets the working directory.
- `COPY . .` — Copies files from the build context into the image.
- `CMD` — Runs the Python application when the container starts.

### Build the Image
```bash
docker build -t python-normal -f Dockerfile .
```

### Run the Container
```bash
docker run --rm python-normal
```

**Expected Output:**
```text
Hello! My multi-stage Docker build is working!
```

## 3. Multi-Stage Docker Build

**File:** `Dockerfile.multi`

```dockerfile
FROM python:3.14 AS builder

WORKDIR /build

RUN echo "Preparing application in builder stage"

FROM python:3.14-slim AS final

WORKDIR /app

COPY app.py .

CMD ["python", "app.py"]
```

### Explanation
- `FROM python:3.14 AS builder` — Creates the first stage named `builder`.
- `RUN echo ...` — Demonstrates a build step in the builder stage.
- `FROM python:3.14-slim AS final` — Starts a separate final stage using a smaller Python image.
- `COPY app.py .` — Copies the application into the final image.
- `CMD` — Runs the Python application.

The final stage does not automatically include the builder stage's files. Only explicitly copied files and files created in the final stage are included.

**Note:** This is a basic demonstration. The builder stage does not yet produce an artifact used by the final stage.

### Build the Image
```bash
docker build -t python-multi -f Dockerfile.multi .
```

### Run the Container
```bash
docker run --rm python-multi
```

**Expected Output:**
```text
Hello! My multi-stage Docker build is working!
```

## 4. Compare Image Sizes

List the images:
```bash
docker images
```

Inspect the normal image size:
```bash
docker image inspect python-normal --format "{{.Size}}"
```

Inspect the multi-stage image size:
```bash
docker image inspect python-multi --format "{{.Size}}"
```

The multi-stage image may be smaller because its final stage uses `python:3.14-slim`. This example does not demonstrate excluding actual build dependencies.

## 5. Important Docker Concepts

| Concept | Meaning |
|---|---|
| Image | A template used to create containers. |
| Container | An instance of an image. |
| `-t` | Assigns an image name and optionally a version tag. |
| `-f` | Specifies which Dockerfile to use. |
| `.` | Sets the current directory as the build context. |
| `FROM` | Selects a base image and starts a build stage. |
| `WORKDIR` | Sets the working directory inside the image. |
| `COPY` | Copies files into the image. |
| `CMD` | Specifies the default command when a container starts. |
| Multi-stage build | Separates build stages from the final runtime image. |
<img width="1150" height="127" alt="docker image " src="https://github.com/user-attachments/assets/78d05d64-46a9-4159-9473-a38057c46e25" />

## 6. What I Learned
Industry-Level Use
In industry, multi-stage Docker builds are used to create lightweight, production-ready images by separating the build environment from the runtime environment. They help exclude unnecessary build tools and temporary files, reducing image size, improving deployment efficiency, and minimizing the attack surface. These images can be pushed to registries like Amazon ECR and deployed through CI/CD pipelines using Jenkins or GitHub Actions.
- How to create and run a Python application.
- How to write a Dockerfile.
- How to build and run Docker images.
- How to use `-t`, `-f`, and the build context.
- How multi-stage builds separate build and runtime stages.
- How to compare Docker image sizes.

# Multi-Platform Docker Images using BuildKit and Buildx

---

## Step 1: Introduction

In this guide, you will learn how to:
- Build multi-platform Docker images using **BuildKit** and **Buildx**.
- Use the `--push` flags with the `docker buildx build` command.

---

## Step 2: Review Dockerfile

```dockerfile
# Use nginx as base Docker Image
FROM nginx

# OCI Labels
LABEL org.opencontainers.image.authors="KMS Healthcare"
LABEL org.opencontainers.image.title="Demo: Create Multi-platform Docker Images using Docker BuildKit and Buildx"
LABEL org.opencontainers.image.description="A Dockerfile demo illustrating Multi-platform Docker Images using Docker BuildKit and Buildx"
LABEL org.opencontainers.image.version="1.0"

# Using COPY to copy a local file
COPY index.html /usr/share/nginx/html
```

---

## Step 3: Verify NGINX Base Image for Multi-Platform Support

- Check the [NGINX Docker Official Image](https://hub.docker.com/_/nginx) to verify if it supports multiple platforms.
- In the **Quick reference (cont.)** section, you can find the supported architectures:

```bash
Supported architectures:
amd64, arm32v5, arm32v6, arm32v7, arm64v8, i386, mips64le, ppc64le, riscv64, s390x
```

---

## Step 4: Build and Load Multi-Platform Docker Images Locally

```bash
# Create new Buildx and Use it
docker buildx create --name multiarch --driver docker-container
docker buildx use multiarch

# List Local builders
docker buildx ls
```

- When building multi-platform images, it is recommended to use `--push` instead of `--load`.
- The `--load` flag is useful for local development but **only supports one platform at a time in the local Docker daemon**.

---

## Step 5: Build and Push Multi-Platform Docker Images to Docker Hub

```bash
# Change Directory
cd $(git rev-parse --show-toplevel)/c38-Docker-BUILD-Multiplatform-Images/multiplatform-demo

# Login with your Docker ID
docker login --username {{YOUR_DOCKER_ID}}

# Build Multi-platform Docker Images and Push to Docker Hub
DOCKER_BUILDKIT=1 docker buildx build --platform linux/amd64,linux/arm64 -t deefmoo/demo-multiplatform-local:v1 --push .
```

- **Replace** `{{YOUR_DOCKER_ID}}` with your actual Docker Hub username.

---

## Step 6: Verify Multi-Platform Docker Image in Docker Hub

1. Go to [Docker Hub](https://hub.docker.com).
2. Navigate to your Docker image: `YOUR_DOCKER_ID/demo-multiplatform-local`.
3. Click on **Tags**.
4. Verify that there are multiple image digests corresponding to different architectures (e.g., `amd64` and `arm64`).

---

## Step 7: Pull, Run, and Verify the Docker Image

```bash
# Pull the Docker image
docker pull deefmoo/demo-multiplatform-local:v1

# Inspect the Docker image
docker image inspect deefmoo/demo-multiplatform-local:v1

# Observation:
# 1. Verify the "Architecture" field in the output.
# 2. The image pulled corresponds to your local machine's architecture.

# Run the Docker container
docker run --name my-multiplatform-demo -p 8080:80 -d deefmoo/demo-multiplatform-local:v1

# List Docker containers
docker ps
```

Access the application in your browser http://localhost:8080

**Clean Up**:

```bash
# Stop the Docker container
docker stop my-multiplatform-demo

# Remove the Docker container
docker rm my-multiplatform-demo
```

---

## Step 8: Stop and Remove Buildx Builder

```bash
# List Docker Buildx builders
docker buildx ls

# Stop Docker Buildx builder
docker buildx stop multiarch

# Remove Docker Buildx builder
docker buildx rm multiarch

# Use default
docker buildx use --default default

# Verify the builders
docker buildx ls
```

---

## Conclusion

By using `docker buildx build` with the `--platform`, and `--push` flags, you can create multi-architecture images and push them to Docker Hub.

- **Multi-Platform**:
  - Build images for multiple CPU architectures (e.g., `amd64`, `arm64`), supporting various devices and environments.

- **`--push` vs. `--load`**:
  - `--push`: Builds and pushes the image to a registry.
  - `--load`: Loads the image locally, supporting only the current platform.

---

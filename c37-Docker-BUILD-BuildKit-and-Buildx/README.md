# Learn with BuildKit (server-side or builder) and Buildx (client-side)

---

## Step 1: [Introduction](https://docs.docker.com/build/concepts/overview/)

- **[BuildKit](https://docs.docker.com/build/buildkit/)**: A next-gen Docker build engine replacing the legacy builder with:  
    - **Parallel builds**: Executes multiple steps at once for faster builds.  
    - **Advanced caching**: Caches individual layers to reduce rebuild times.  
    - **Incremental builds**: Transfers only changed files, minimizing overhead.  
    - **Better performance**: Enhanced speed, storage, and flexibility over the old builder.  

- **[Buildx](https://github.com/docker/buildx)**: A Docker CLI plugin extending BuildKit with:
    - **Multi-platform builds**: Create images for multiple architectures (e.g., `amd64`, `arm64`) from one system.  
    - **Parallel builds**: Run multiple builds simultaneously across nodes, ideal for large projects.

---

## Step 2: Docker Buildx Commands

```bash
# Show buildx version information
docker buildx version

# List builder instances
docker buildx ls

# Remove build cache
docker buildx prune

# Use default
docker buildx use --default default

# Create a new Buildx builder
docker buildx create --name mybuilder1 --driver docker-container --use

# Inspect Buildx builder
docker buildx inspect --builder mybuilder1

# Remove Buildx builder
docker buildx rm mybuilder1
```

---

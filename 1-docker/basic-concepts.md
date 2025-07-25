# 1️⃣ Basic Concepts (Theory)

## Q1: What is Docker, and how does it differ from VMs?

Docker is a platform for developing, shipping, and running applications in lightweight, portable containers. Unlike virtual machines (VMs) which virtualize an entire operating system including the kernel, Docker containers share the host system's kernel and isolate the application processes from the rest of the system. This makes containers more lightweight, faster to start, and more resource-efficient than VMs.

Key differences:
- VMs require a full OS for each instance, while containers share the host OS kernel
- Containers start in seconds vs minutes for VMs
- Containers have less overhead and better performance
- VMs provide stronger isolation boundaries

## Q2: Explain Docker images vs. containers.

Docker images are read-only templates used to create containers. They contain the application code, libraries, dependencies, and other files needed to run an application. Images are built from Dockerfiles and can be stored in registries like Docker Hub.

Docker containers are running instances of images. When you start an image, you create a container. Containers are lightweight, portable, and isolated environments where applications run. Multiple containers can be created from the same image.

Analogy: If an image is like a class in programming, then a container is like an instance of that class.

## Q3: What are Docker volumes, and why use them?

Docker volumes are a mechanism for persisting data generated by and used by Docker containers. They are separate from the container's writable layer, so data persists even when the container is removed.

Reasons to use volumes:
- Persist data beyond container lifecycle
- Share data between containers
- Backup and restore data
- Better I/O performance compared to container filesystem
- Avoid increasing container size with persistent data

Volumes can be managed using `docker volume` commands and can be mounted to specific paths in containers.

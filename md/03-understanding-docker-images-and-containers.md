# Lesson 3: Understanding Docker Images and Containers

## What are Docker Images?

A **Docker image** is a lightweight, standalone, and executable package that includes everything needed to run a piece of software, including the code, runtime, libraries, environment variables, and configuration files. Images are read-only templates used to create containers.

### Key Characteristics of Docker Images:
- **Immutable**: Once created, images cannot be changed. Any modifications require creating a new image.
- **Layered**: Images are built in layers, allowing for efficient storage and sharing. Each layer represents a change or addition to the image.
- **Portable**: Images can be shared and run on any system that has Docker installed, ensuring consistency across environments.

## What are Docker Containers?

A **Docker container** is a runnable instance of a Docker image. Containers are created from images and can be started, stopped, moved, and deleted. Unlike images, containers are mutable and can change during runtime.

### Key Characteristics of Docker Containers:
- **Isolated**: Each container runs in its own environment, isolated from other containers and the host system.
- **Ephemeral**: Containers can be created and destroyed quickly. They can also be stopped and started without losing their state if configured to use persistent storage.
- **Lightweight**: Containers share the host OS kernel, making them more efficient than traditional virtual machines.

## Difference Between Docker Images and Containers

| Feature         | Docker Image                        | Docker Container                   |
|-----------------|------------------------------------|------------------------------------|
| Definition      | A read-only template for creating containers | A running instance of an image     |
| State           | Immutable                          | Mutable                            |
| Purpose         | Used to create containers          | Executes applications               |
| Storage         | Stored as layers                   | Uses the image to run processes    |
| Lifecycle       | Built and versioned                | Created, started, stopped, deleted |

## Creating Docker Containers

To create a Docker container, you need to use the `docker run` command followed by the image name. Here’s how to do it:

### Step 1: Pull an Image

Before creating a container, ensure you have the desired image. For example, to pull the latest Ubuntu image, run:

```bash
docker pull ubuntu
```

### Step 2: Create and Run a Container

To create and run a container from the Ubuntu image, use the following command:

```bash
docker run -it ubuntu
```

- The `-it` flags allow you to run the container in interactive mode with a terminal.

### Step 3: Access the Container

Once the container is running, you will be inside the Ubuntu shell. You can run commands as if you were using a regular terminal.

## Managing Docker Containers

### 1. **Listing Containers**

To see all running containers, use:

```bash
docker ps
```

To list all containers, including stopped ones, use:

```bash
docker ps -a
```

### 2. **Stopping a Container**

To stop a running container, use the following command, replacing `<container-id>` with the actual ID or name of the container:

```bash
docker stop <container-id>
```

### 3. **Starting a Stopped Container**

To start a stopped container, use:

```bash
docker start <container-id>
```

### 4. **Removing a Container**

To remove a container, first ensure it is stopped. Then, use:

```bash
docker rm <container-id>
```

### 5. **Removing All Stopped Containers**

To remove all stopped containers at once, run:

```bash
docker container prune
```

You will be prompted to confirm the action.

## Creating a Container with Custom Commands

You can also create and run a container while executing a specific command. For example, to run a simple command in an Ubuntu container, use:

```bash
docker run ubuntu echo "Hello, Docker!"
```

This command will create a new container, execute the `echo` command, and then exit.

## Conclusion

In this lesson, you learned the difference between Docker images and containers, as well as how to create, manage, and delete Docker containers. Understanding these concepts is crucial for effectively using Docker in your development workflow. In the next lesson, we will explore how to build your first Docker image using a Dockerfile.
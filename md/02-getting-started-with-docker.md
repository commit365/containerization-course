# Lesson 2: Getting Started with Docker

## Introduction to Docker

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization. With Docker, you can easily create, deploy, and run applications in containers, ensuring consistency across different environments.

## Step 1: Install Docker on Your Local Machine

### For Windows

1. **Download Docker Desktop**:
   - Go to the [Docker Desktop for Windows page](https://www.docker.com/products/docker-desktop) and download the installer.

2. **Install Docker Desktop**:
   - Run the downloaded installer and follow the on-screen instructions. Ensure that you enable the WSL 2 feature if prompted.

3. **Start Docker Desktop**:
   - After installation, launch Docker Desktop from your Start menu. It may take a few moments for Docker to start.

### For macOS

1. **Download Docker Desktop**:
   - Visit the [Docker Desktop for Mac page](https://www.docker.com/products/docker-desktop) and download the installer.

2. **Install Docker Desktop**:
   - Open the downloaded `.dmg` file and drag the Docker icon to your Applications folder.

3. **Start Docker Desktop**:
   - Launch Docker from your Applications folder. Wait for Docker to initialize.

### For Linux

1. **Install Docker Engine**:
   - Open your terminal and run the following commands based on your Linux distribution:

   - **For Ubuntu**:
     ```bash
     sudo apt update
     sudo apt install docker.io
     ```

   - **For CentOS**:
     ```bash
     sudo yum install docker
     ```

   - **For Fedora**:
     ```bash
     sudo dnf install docker
     ```

2. **Start Docker**:
   - After installation, start the Docker service:
     ```bash
     sudo systemctl start docker
     ```

3. **Enable Docker to Start on Boot**:
   ```bash
   sudo systemctl enable docker
   ```

## Step 2: Verify the Installation

To ensure Docker is installed correctly, open your terminal or command prompt and run the following command:

```bash
docker --version
```

You should see output similar to:

```
Docker version 20.10.7, build f0df350
```

Next, run the following command to verify that Docker is running:

```bash
docker run hello-world
```

This command downloads a test image and runs it in a container. If everything is working correctly, you will see a message confirming that Docker is installed successfully.

## Step 3: Familiarize Yourself with the Docker CLI

The Docker Command Line Interface (CLI) allows you to interact with the Docker daemon to manage containers, images, and other resources. Here are some basic commands to get you started:

### 1. **List Docker Images**

To view the images you have on your local machine, use:

```bash
docker images
```

### 2. **List Running Containers**

To see the containers that are currently running, use:

```bash
docker ps
```

### 3. **List All Containers**

To view all containers, including those that are stopped, use:

```bash
docker ps -a
```

### 4. **Pull an Image from Docker Hub**

To download an image from Docker Hub (the default registry), use:

```bash
docker pull <image-name>
```

For example, to pull the latest version of the Ubuntu image:

```bash
docker pull ubuntu
```

### 5. **Run a Container**

To create and run a new container from an image, use:

```bash
docker run -it <image-name>
```

For example, to run an interactive Ubuntu container:

```bash
docker run -it ubuntu
```

### 6. **Stop a Running Container**

To stop a running container, use:

```bash
docker stop <container-id>
```

You can find the container ID by using the `docker ps` command.

### 7. **Remove a Container**

To remove a stopped container, use:

```bash
docker rm <container-id>
```

### 8. **Remove an Image**

To remove an image from your local machine, use:

```bash
docker rmi <image-name>
```

## Conclusion

In this lesson, you learned how to install Docker on your local machine and verify the installation. You also familiarized yourself with basic Docker CLI commands to manage images and containers. In the next lesson, we will dive deeper into understanding Docker images and containers, exploring how to create and manage them effectively.
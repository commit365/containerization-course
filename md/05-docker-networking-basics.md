# Lesson 5: Docker Networking Basics

## Introduction to Docker Networking

Networking is a crucial aspect of containerization, allowing containers to communicate with each other and with external systems. Docker provides a flexible networking model that enables you to configure how containers connect and interact.

## Understanding the Networking Model in Docker

Docker uses a client-server architecture, where the Docker client communicates with the Docker daemon (server). The Docker daemon manages the containers and their networking. Each container can be connected to one or more networks, enabling communication with other containers and external services.

### Key Concepts

- **Network Namespace**: Each Docker container runs in its own network namespace, providing isolation for network configurations.
- **IP Address**: Each container is assigned a unique IP address within its network, allowing it to communicate with other containers.
- **Ports**: Containers can expose ports to allow external access to services running inside them.

## How Containers Communicate

Containers can communicate with each other using their IP addresses or container names, depending on the network configuration. Docker provides several networking options to facilitate this communication.

## Types of Docker Networks

Docker supports several types of networks, each serving different use cases. Here are the most common types:

### 1. **Bridge Network**

- **Default Network Type**: When you create a container without specifying a network, Docker connects it to the default bridge network.
- **Use Case**: Suitable for standalone containers that need to communicate with each other on the same host.
- **Communication**: Containers can communicate using their IP addresses or container names.

**Example**: To create a new container on the bridge network:

```bash
docker run -d --name my-container --network bridge nginx
```

### 2. **Host Network**

- **Direct Access to Host**: Containers share the host's network stack, meaning they use the host's IP address and can access services on the host directly.
- **Use Case**: Useful for performance-sensitive applications that require low latency and need to access host services directly.
- **Limitation**: Containers cannot have their own IP addresses; they share the host's.

**Example**: To run a container using the host network:

```bash
docker run -d --name my-container --network host nginx
```

### 3. **Overlay Network**

- **Multi-Host Networking**: Allows containers running on different Docker hosts to communicate securely.
- **Use Case**: Ideal for applications deployed in a swarm mode or across multiple hosts, enabling service discovery and load balancing.
- **Configuration**: Requires a key-value store (like Consul or etcd) for managing network state.

**Example**: To create an overlay network:

```bash
docker network create --driver overlay my-overlay-network
```

### 4. **Macvlan Network**

- **Custom MAC Addresses**: Allows you to assign a MAC address to a container, making it appear as a physical device on the network.
- **Use Case**: Useful for applications that require direct access to the physical network, such as legacy applications.
- **Configuration**: Requires additional setup for the physical network interface.

**Example**: To create a macvlan network:

```bash
docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 my-macvlan
```

### 5. **None Network**

- **No Networking**: Containers connected to this network have no network access.
- **Use Case**: Useful for applications that do not require network connectivity or for testing purposes.

**Example**: To run a container with no networking:

```bash
docker run -d --name my-container --network none nginx
```

## Managing Docker Networks

### Listing Networks

To view all Docker networks on your system, use:

```bash
docker network ls
```

### Inspecting a Network

To get detailed information about a specific network, use:

```bash
docker network inspect <network-name>
```

### Removing a Network

To remove a network that is no longer in use, use:

```bash
docker network rm <network-name>
```

## Conclusion

In this lesson, you learned about the Docker networking model and how containers communicate. You also explored the different types of Docker networks and their use cases. Understanding Docker networking is essential for building scalable and efficient containerized applications. In the next lesson, we will dive into data management in Docker, focusing on volumes and persistent storage.
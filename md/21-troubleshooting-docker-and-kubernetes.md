# Lesson 21: Troubleshooting Docker and Kubernetes

## Introduction to Troubleshooting

Troubleshooting is an essential skill for any developer or operator working with Docker and Kubernetes. As with any complex system, issues can arise that may affect the performance, availability, or functionality of your applications. In this lesson, we will cover common troubleshooting techniques and explore tools and commands that can help you diagnose and resolve issues effectively.

## Troubleshooting Docker

### Common Docker Issues

1. **Container Fails to Start**: This can occur due to misconfigurations, missing dependencies, or incorrect commands in the Dockerfile.

2. **Application Crashes**: If your application crashes, it may be due to resource constraints, bugs in the application code, or misconfigured environment variables.

3. **Networking Issues**: Containers may have difficulty communicating with each other or with external services due to network misconfigurations.

### Troubleshooting Techniques

#### 1. Check Container Logs

To view the logs of a specific container, use the following command:

```bash
docker logs <container-id>
```

You can also follow the logs in real-time using the `-f` flag:

```bash
docker logs -f <container-id>
```

#### 2. Inspect Container Status

To get detailed information about a container, including its status and configuration, use:

```bash
docker inspect <container-id>
```

This command provides JSON output with all the details about the container.

#### 3. Check Running Containers

To see a list of running containers and their statuses, use:

```bash
docker ps
```

To view all containers, including stopped ones, use:

```bash
docker ps -a
```

#### 4. Resource Usage

To check the resource usage of containers, use:

```bash
docker stats
```

This command displays real-time statistics about CPU, memory, and network usage for all running containers.

#### 5. Networking Troubleshooting

If you suspect networking issues, you can check the network configuration of a container:

```bash
docker network inspect <network-name>
```

You can also use `ping` or `curl` commands inside the container to test connectivity to other services.

## Troubleshooting Kubernetes

### Common Kubernetes Issues

1. **Pod Not Starting**: Pods may fail to start due to insufficient resources, misconfigured environment variables, or issues with the container image.

2. **CrashLoopBackOff**: This occurs when a Pod repeatedly fails to start and Kubernetes keeps restarting it.

3. **Service Not Accessible**: Services may not be reachable due to misconfigured networking or firewall rules.

### Troubleshooting Techniques

#### 1. Check Pod Logs

To view the logs of a specific Pod, use:

```bash
kubectl logs <pod-name>
```

To view logs from all containers in a Pod:

```bash
kubectl logs <pod-name> --all-containers=true
```

#### 2. Describe the Pod

To get detailed information about a Pod, including events and status, use:

```bash
kubectl describe pod <pod-name>
```

This command provides insights into why a Pod may not be starting or is in a failed state.

#### 3. Check Pod Status

To see the status of all Pods in a namespace, use:

```bash
kubectl get pods
```

You can add the `-n <namespace>` flag to check Pods in a specific namespace.

#### 4. Inspect Events

Kubernetes records events that can provide clues about issues. To view events in a namespace, use:

```bash
kubectl get events -n <namespace>
```

This command lists recent events, including warnings and errors related to Pods and other resources.

#### 5. Resource Usage

To check the resource usage of Pods, you can use the Metrics Server:

```bash
kubectl top pods
```

This command shows CPU and memory usage for each Pod, helping you identify resource constraints.

#### 6. Networking Troubleshooting

To troubleshoot networking issues, you can check the following:

- **Service Endpoints**: Verify that the service endpoints are correctly configured:

  ```bash
  kubectl get endpoints <service-name>
  ```

- **Network Policies**: Ensure that network policies are not blocking traffic between Pods.

- **DNS Resolution**: Use `kubectl exec` to get a shell in a Pod and test DNS resolution:

  ```bash
  kubectl exec -it <pod-name> -- /bin/sh
  nslookup <service-name>
  ```

## Tools for Troubleshooting

1. **kubectl**: The command-line tool for interacting with Kubernetes. It provides commands for managing resources and troubleshooting.

2. **Docker CLI**: The command-line interface for managing Docker containers. It provides commands for inspecting containers, viewing logs, and checking resource usage.

3. **Kubernetes Dashboard**: A web-based UI for managing and troubleshooting Kubernetes clusters. It provides insights into resource usage, logs, and events.

4. **Prometheus and Grafana**: Monitoring tools that provide insights into the performance and health of your applications. They can help identify issues related to resource usage and performance.

5. **Jaeger or Zipkin**: Distributed tracing tools that help diagnose performance issues in microservices architectures by tracking requests as they flow through the system.

## Conclusion

In this lesson, you learned common troubleshooting techniques for Docker and Kubernetes, including how to check logs, inspect resources, and diagnose networking issues. You also explored tools and commands that can help you effectively diagnose and resolve issues in your containerized applications. Mastering troubleshooting skills is essential for maintaining the health and performance of your Docker and Kubernetes environments. In the next lesson, we will summarize the key concepts covered throughout the course and discuss next steps for further learning and exploration.
# Lesson 14: Networking in Kubernetes

## Introduction to Kubernetes Networking

Kubernetes networking is a critical component of the architecture that allows communication between Pods, Services, and external clients. Understanding the Kubernetes networking model and the different types of Services is essential for deploying and managing applications effectively. In this lesson, we will explore the Kubernetes networking model, service types (ClusterIP, NodePort, LoadBalancer), and Ingress controllers for managing external access.

## Kubernetes Networking Model

Kubernetes networking is based on several key principles:

1. **Flat Network Space**: All Pods can communicate with each other without Network Address Translation (NAT). Each Pod receives its own IP address, and Pods can communicate with each other using these IP addresses.

2. **Service Discovery**: Kubernetes provides built-in service discovery, allowing Pods to find and communicate with each other using DNS names.

3. **Network Policies**: You can define network policies to control the traffic flow between Pods, enhancing security and isolation.

## Service Types in Kubernetes

Kubernetes offers several types of Services to expose applications running in Pods. The three primary service types are **ClusterIP**, **NodePort**, and **LoadBalancer**.

### 1. ClusterIP

- **Description**: The default service type. It exposes the Service on a cluster-internal IP. This means that the Service is only accessible from within the cluster.
- **Use Case**: Suitable for internal communication between Pods and Services that do not require external access.

**Example**: To create a ClusterIP service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

You can create this service using:

```bash
kubectl apply -f my-clusterip-service.yaml
```

### 2. NodePort

- **Description**: Exposes the Service on each Node’s IP at a static port (the NodePort). This allows external traffic to access the Service.
- **Use Case**: Useful for development and testing environments where you want to expose a service externally without a cloud provider.

**Example**: To create a NodePort service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30007
```

You can create this service using:

```bash
kubectl apply -f my-nodeport-service.yaml
```

To access the service, use the Node's IP address and the NodePort (e.g., `http://<Node-IP>:30007`).

### 3. LoadBalancer

- **Description**: Exposes the Service externally using a cloud provider’s load balancer. It automatically provisions a load balancer and assigns a public IP address.
- **Use Case**: Ideal for production environments where you need to expose services to the internet.

**Example**: To create a LoadBalancer service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

You can create this service using:

```bash
kubectl apply -f my-loadbalancer-service.yaml
```

Once created, you can check the external IP assigned to the service by running:

```bash
kubectl get services
```

## Ingress Controllers

**Ingress** is a Kubernetes resource that manages external access to services within a cluster, typically HTTP and HTTPS traffic. An Ingress controller is responsible for fulfilling the Ingress rules defined in your cluster.

### Key Features of Ingress:

- **Path-Based Routing**: Ingress can route traffic based on the request path, allowing you to direct traffic to multiple services using a single IP address.
- **TLS Termination**: Ingress can handle TLS termination, allowing you to secure your applications with HTTPS.
- **Load Balancing**: Ingress can provide load balancing for your services.

### Setting Up an Ingress Controller

1. **Install an Ingress Controller**: There are several Ingress controllers available, such as NGINX Ingress Controller, Traefik, and HAProxy. For this lesson, we will use the NGINX Ingress Controller.

   To install the NGINX Ingress Controller using Helm, run:

   ```bash
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   helm install my-nginx ingress-nginx/ingress-nginx
   ```

2. **Create an Ingress Resource**: After installing the Ingress controller, you can create an Ingress resource to manage external access to your services. Here’s an example:

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: my-ingress
   spec:
     rules:
     - host: myapp.example.com
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: my-loadbalancer-service
               port:
                 number: 80
   ```

   Save this configuration to a file named `my-ingress.yaml` and apply it using:

   ```bash
   kubectl apply -f my-ingress.yaml
   ```

3. **Accessing the Application**: To access your application through the Ingress, you need to configure your DNS to point to the external IP of the Ingress controller. You can find the external IP by running:

   ```bash
   kubectl get services -o wide -w -n ingress-nginx
   ```

## Conclusion

In this lesson, you learned about the Kubernetes networking model and the different types of Services (ClusterIP, NodePort, LoadBalancer) used to expose applications. You also explored Ingress controllers for managing external access to your services, including setting up an NGINX Ingress Controller and creating an Ingress resource. Understanding Kubernetes networking is essential for effectively deploying and managing applications in a Kubernetes cluster. In the next lesson, we will dive into advanced Kubernetes concepts, including Helm and custom resource definitions (CRDs).
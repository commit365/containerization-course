# Lesson 10: Deploying Your First Application on Kubernetes

## Introduction to Deployments and Services

In this lesson, you will learn how to deploy a simple application on Kubernetes using a **Deployment** and expose it as a **Service**. We will use a basic Nginx web server as our application. By the end of this lesson, you will understand the concepts of Pods, Deployments, and Services in Kubernetes.

## What are Pods, Deployments, and Services?

- **Pod**: The smallest deployable unit in Kubernetes. A Pod can contain one or more containers that share the same network namespace and storage. Pods are ephemeral and can be created, destroyed, or replicated based on the desired state defined by the user.

- **Deployment**: A higher-level abstraction that manages a set of identical Pods. It ensures that the desired number of Pods are running and can handle updates to the application seamlessly.

- **Service**: An abstraction that defines a logical set of Pods and a policy for accessing them. Services enable communication between different parts of your application and provide stable endpoints for accessing Pods.

## Step 1: Create a Simple Deployment

1. **Open Your Terminal**: Ensure you are connected to your Kubernetes cluster.

2. **Create a Deployment**: Use the following command to create a Deployment that runs an Nginx web server:

   ```bash
   kubectl create deployment nginx-deployment --image=nginx
   ```

   This command creates a Deployment named `nginx-deployment` using the official Nginx image.

3. **Verify the Deployment**: To check the status of the Deployment and see the Pods created by it, run:

   ```bash
   kubectl get deployments
   ```

   You should see output indicating that the `nginx-deployment` is running.

4. **List the Pods**: To see the Pods created by the Deployment, use:

   ```bash
   kubectl get pods
   ```

   You should see one or more Pods with names starting with `nginx-deployment`.

## Step 2: Expose the Deployment as a Service

Now that you have a Deployment running, you can expose it as a Service to make it accessible from outside the cluster.

1. **Expose the Deployment**: Run the following command to create a Service that exposes the Nginx Deployment:

   ```bash
   kubectl expose deployment nginx-deployment --type=NodePort --port=80
   ```

   - The `--type=NodePort` option exposes the Service on a static port on each Node in the cluster.
   - The `--port=80` option specifies that the Service will listen on port 80.

2. **Verify the Service**: To check the status of the Service, use:

   ```bash
   kubectl get services
   ```

   You should see a Service named `nginx-deployment` with a `NodePort` assigned.

3. **Get the NodePort**: To find out which port has been assigned, look for the `PORT(S)` column in the output of the previous command. It will look something like `80:<NodePort>/TCP`.

## Step 3: Access the Nginx Application

1. **Get the Node IP Address**: To access the Nginx application, you need the IP address of one of the Nodes in your cluster. If you are using Minikube, you can get the IP address by running:

   ```bash
   minikube ip
   ```

   If you are using Docker Desktop, you can access the application using `localhost`.

2. **Access the Application**: Open your web browser and navigate to `http://<Node-IP>:<NodePort>`. Replace `<Node-IP>` with the IP address you obtained and `<NodePort>` with the port number from the Service.

   You should see the Nginx welcome page, indicating that your application is running successfully.

## Step 4: Clean Up Resources

After testing your application, you can clean up the resources you created.

1. **Delete the Service**:

   ```bash
   kubectl delete service nginx-deployment
   ```

2. **Delete the Deployment**:

   ```bash
   kubectl delete deployment nginx-deployment
   ```

3. **Verify Deletion**: To ensure that the resources have been deleted, you can run:

   ```bash
   kubectl get deployments
   kubectl get services
   ```

   You should see no resources listed for the `nginx-deployment`.

## Conclusion

In this lesson, you learned how to deploy a simple application on Kubernetes using a Deployment and expose it as a Service. You explored the concepts of Pods, Deployments, and Services, gaining hands-on experience with Kubernetes. In the next lesson, we will dive deeper into scaling applications and managing updates in Kubernetes.
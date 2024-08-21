# Lesson 15: Persistent Storage in Kubernetes

## Introduction to Persistent Storage

In Kubernetes, managing stateful applications often requires persistent storage that survives beyond the lifecycle of individual Pods. Kubernetes provides a robust mechanism for managing persistent storage through **Persistent Volumes (PVs)** and **Persistent Volume Claims (PVCs)**. This lesson will explain these concepts and guide you on how to manage data persistence in your Kubernetes applications.

## Understanding Persistent Volumes (PV)

A **Persistent Volume (PV)** is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. PVs are resources in the cluster that exist independently of any individual Pod that uses the PV.

### Key Features of Persistent Volumes:
- **Lifecycle Management**: PVs have a lifecycle that is independent of the Pods that use them.
- **Storage Types**: PVs can be backed by various storage types, including NFS, AWS EBS, Google Cloud Persistent Disk, and many others.
- **Capacity and Access Modes**: PVs have specific capacity and access modes that define how they can be used.

### Example of a Persistent Volume

Here’s an example of how to create a Persistent Volume:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

In this example:
- The PV is named `my-pv`.
- It has a capacity of 5 GiB.
- The access mode is `ReadWriteOnce`, meaning it can be mounted as read-write by a single node.
- It uses a `hostPath`, which is suitable for testing but not recommended for production.

You can create this PV using:

```bash
kubectl apply -f my-pv.yaml
```

## Understanding Persistent Volume Claims (PVC)

A **Persistent Volume Claim (PVC)** is a request for storage by a user. It specifies the desired storage size and access modes. When a PVC is created, Kubernetes will try to find a matching PV to bind to it.

### Key Features of Persistent Volume Claims:
- **Dynamic Binding**: If no suitable PV is available, Kubernetes can dynamically provision a new PV based on the Storage Class specified in the PVC.
- **Decoupling**: PVCs decouple the storage request from the underlying storage technology, allowing users to request storage without needing to know the details of the underlying infrastructure.

### Example of a Persistent Volume Claim

Here’s an example of how to create a Persistent Volume Claim:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

In this example:
- The PVC is named `my-pvc`.
- It requests 5 GiB of storage with the access mode `ReadWriteOnce`.

You can create this PVC using:

```bash
kubectl apply -f my-pvc.yaml
```

## Using Persistent Volumes and Claims in Deployments

Once you have a PVC, you can use it in your Pods or Deployments to ensure that your application has persistent storage.

### Example of a Deployment Using a PVC

Here’s an example of a Deployment that uses the `my-pvc` Persistent Volume Claim:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: nginx
        volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: my-storage
      volumes:
      - name: my-storage
        persistentVolumeClaim:
          claimName: my-pvc
```

In this example:
- The Deployment is named `my-app`.
- It mounts the PVC `my-pvc` to the path `/usr/share/nginx/html` in the Nginx container.

You can create this Deployment using:

```bash
kubectl apply -f my-app-deployment.yaml
```

## Managing Data Persistence in Kubernetes Applications

### 1. **Creating and Binding PVs and PVCs**

- Create a Persistent Volume (PV) to define the storage resource.
- Create a Persistent Volume Claim (PVC) to request storage for your application.

### 2. **Using PVCs in Deployments**

- Reference the PVC in your Deployment to mount the persistent storage into your application.

### 3. **Monitoring and Managing Storage**

- Use the following commands to monitor your PVs and PVCs:

```bash
kubectl get pv
kubectl get pvc
```

- Ensure that the status of your PVC is `Bound`, indicating that it is successfully linked to a PV.

### 4. **Deleting Resources**

- When you no longer need the resources, you can delete the PVC, which may also delete the associated PV depending on the `reclaimPolicy` set on the PV.

```bash
kubectl delete pvc my-pvc
kubectl delete pv my-pv
```

## Conclusion

In this lesson, you learned about Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) in Kubernetes. You explored how to create and manage persistent storage for your applications, ensuring that data remains available even when Pods are restarted or rescheduled. Understanding persistent storage is essential for deploying stateful applications in Kubernetes. In the next lesson, we will explore advanced Kubernetes concepts, including Helm and custom resource definitions (CRDs).
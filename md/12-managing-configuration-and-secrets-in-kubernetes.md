# Lesson 12: Managing Configuration and Secrets in Kubernetes

## Introduction to ConfigMaps and Secrets

In Kubernetes, managing configuration data and sensitive information is crucial for application security and flexibility. Kubernetes provides two primary objects for this purpose: **ConfigMaps** and **Secrets**. 

- **ConfigMaps** are used to store non-sensitive configuration data in key-value pairs.
- **Secrets** are used to store sensitive information, such as passwords, OAuth tokens, and SSH keys, in a secure way.

By using ConfigMaps and Secrets, you can decouple configuration and sensitive data from your application code, making it easier to manage and update your applications without redeploying them.

## ConfigMaps

### What is a ConfigMap?

A **ConfigMap** is a Kubernetes object that allows you to store configuration data in key-value pairs. You can use ConfigMaps to manage application settings, environment variables, and command-line arguments.

### Creating a ConfigMap

1. **Create a ConfigMap from Literal Values**: You can create a ConfigMap directly from the command line using literal values. For example, to create a ConfigMap named `app-config` with some configuration data, run:

   ```bash
   kubectl create configmap app-config --from-literal=APP_ENV=production --from-literal=APP_DEBUG=false
   ```

2. **Verify the ConfigMap**: To check the created ConfigMap, run:

   ```bash
   kubectl get configmaps
   ```

   You should see `app-config` listed.

3. **Describe the ConfigMap**: To view the details of the ConfigMap, use:

   ```bash
   kubectl describe configmap app-config
   ```

### Using ConfigMaps in Deployments

You can use ConfigMaps in your deployments to inject configuration data into your applications.

1. **Create a Deployment Using a ConfigMap**: Here’s an example of how to create a Deployment that uses the `app-config` ConfigMap:

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
           env:
           - name: APP_ENV
             valueFrom:
               configMapKeyRef:
                 name: app-config
                 key: APP_ENV
           - name: APP_DEBUG
             valueFrom:
               configMapKeyRef:
                 name: app-config
                 key: APP_DEBUG
   ```

   Save this YAML to a file named `my-app-deployment.yaml`.

2. **Deploy the Application**: Apply the configuration using:

   ```bash
   kubectl apply -f my-app-deployment.yaml
   ```

3. **Verify the Deployment**: To check if the Deployment is running, use:

   ```bash
   kubectl get deployments
   ```

## Secrets

### What is a Secret?

A **Secret** is a Kubernetes object that stores sensitive information, such as passwords, tokens, or SSH keys. Secrets are encoded in base64 to provide a layer of obfuscation, but they are not encrypted by default.

### Creating a Secret

1. **Create a Secret from Literal Values**: You can create a Secret directly from the command line. For example, to create a Secret named `db-secret` with a username and password, run:

   ```bash
   kubectl create secret generic db-secret --from-literal=username=myuser --from-literal=password=mypassword
   ```

2. **Verify the Secret**: To check the created Secret, run:

   ```bash
   kubectl get secrets
   ```

   You should see `db-secret` listed.

3. **Describe the Secret**: To view the details of the Secret, use:

   ```bash
   kubectl describe secret db-secret
   ```

### Using Secrets in Deployments

You can use Secrets in your deployments to securely inject sensitive information into your applications.

1. **Create a Deployment Using a Secret**: Here’s an example of how to create a Deployment that uses the `db-secret` Secret:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-database-app
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: my-database-app
     template:
       metadata:
         labels:
           app: my-database-app
       spec:
         containers:
         - name: my-database-app
           image: nginx
           env:
           - name: DB_USERNAME
             valueFrom:
               secretKeyRef:
                 name: db-secret
                 key: username
           - name: DB_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: db-secret
                 key: password
   ```

   Save this YAML to a file named `my-database-app-deployment.yaml`.

2. **Deploy the Application**: Apply the configuration using:

   ```bash
   kubectl apply -f my-database-app-deployment.yaml
   ```

3. **Verify the Deployment**: To check if the Deployment is running, use:

   ```bash
   kubectl get deployments
   ```

## Conclusion

In this lesson, you learned how to manage application configurations and sensitive information in Kubernetes using ConfigMaps and Secrets. You created ConfigMaps to store non-sensitive configuration data and Secrets to store sensitive information securely. You also learned how to use them in your deployments to inject configuration and sensitive data into your applications. In the next lesson, we will explore monitoring and logging in Kubernetes to help you manage your applications effectively.
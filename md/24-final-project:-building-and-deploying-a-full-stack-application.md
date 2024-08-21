# Lesson 24: Final Project: Building and Deploying a Full-Stack Application

## Introduction to the Final Project

In this final project, you will apply everything you've learned throughout the course to build and deploy a full-stack application using Docker and Kubernetes. This project will involve creating a simple web application with a frontend and a backend, containerizing the application using Docker, and deploying it to a Kubernetes cluster. By the end of this lesson, you will have a complete understanding of how to integrate all the concepts covered in the course.

## Project Overview

### Application Architecture

For this project, we will create a simple full-stack application that consists of:

1. **Frontend**: A React application that interacts with the backend API.
2. **Backend**: A Node.js/Express application that serves data to the frontend.
3. **Database**: A MongoDB database to store application data.

### Technology Stack

- **Frontend**: React
- **Backend**: Node.js with Express
- **Database**: MongoDB
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Cloud Provider**: (Optional) Deploy to a cloud provider like AWS, GCP, or Azure, or run locally using Minikube.

## Step 1: Setting Up the Project Structure

1. **Create a Project Directory**:

   ```bash
   mkdir fullstack-app
   cd fullstack-app
   ```

2. **Create Subdirectories**:

   ```bash
   mkdir frontend backend
   ```

## Step 2: Building the Backend

### 1. Create the Backend Application

1. **Navigate to the Backend Directory**:

   ```bash
   cd backend
   ```

2. **Initialize a Node.js Application**:

   ```bash
   npm init -y
   ```

3. **Install Dependencies**:

   ```bash
   npm install express mongoose cors
   ```

4. **Create the Server File**:

   Create a file named `server.js` and add the following code:

   ```javascript
   const express = require('express');
   const mongoose = require('mongoose');
   const cors = require('cors');

   const app = express();
   const PORT = process.env.PORT || 5000;

   app.use(cors());
   app.use(express.json());

   // MongoDB connection
   mongoose.connect('mongodb://mongo:27017/mydatabase', {
       useNewUrlParser: true,
       useUnifiedTopology: true,
   });

   const ItemSchema = new mongoose.Schema({
       name: String,
   });

   const Item = mongoose.model('Item', ItemSchema);

   app.get('/items', async (req, res) => {
       const items = await Item.find();
       res.json(items);
   });

   app.post('/items', async (req, res) => {
       const newItem = new Item(req.body);
       await newItem.save();
       res.json(newItem);
   });

   app.listen(PORT, () => {
       console.log(`Server is running on port ${PORT}`);
   });
   ```

5. **Create a Dockerfile**:

   Create a file named `Dockerfile` in the backend directory with the following content:

   ```dockerfile
   FROM node:14
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   CMD ["node", "server.js"]
   ```

## Step 3: Building the Frontend

### 1. Create the Frontend Application

1. **Navigate to the Frontend Directory**:

   ```bash
   cd ../frontend
   ```

2. **Create a React Application**:

   You can use Create React App to set up the frontend:

   ```bash
   npx create-react-app .
   ```

3. **Install Axios**:

   Install Axios for making HTTP requests:

   ```bash
   npm install axios
   ```

4. **Update the App Component**:

   Replace the contents of `src/App.js` with the following code:

   ```javascript
   import React, { useEffect, useState } from 'react';
   import axios from 'axios';

   function App() {
       const [items, setItems] = useState([]);
       const [itemName, setItemName] = useState('');

       useEffect(() => {
           fetchItems();
       }, []);

       const fetchItems = async () => {
           const response = await axios.get('http://localhost:5000/items');
           setItems(response.data);
       };

       const addItem = async () => {
           await axios.post('http://localhost:5000/items', { name: itemName });
           setItemName('');
           fetchItems();
       };

       return (
           <div>
               <h1>Items</h1>
               <input
                   type="text"
                   value={itemName}
                   onChange={(e) => setItemName(e.target.value)}
               />
               <button onClick={addItem}>Add Item</button>
               <ul>
                   {items.map((item) => (
                       <li key={item._id}>{item.name}</li>
                   ))}
               </ul>
           </div>
       );
   }

   export default App;
   ```

5. **Create a Dockerfile**:

   Create a file named `Dockerfile` in the frontend directory with the following content:

   ```dockerfile
   FROM node:14
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   RUN npm run build
   CMD ["npx", "serve", "-s", "build"]
   ```

## Step 4: Creating Kubernetes Manifests

### 1. Create a Kubernetes Deployment for MongoDB

Create a file named `mongo-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: mongo:latest
        ports:
        - containerPort: 27017
---
apiVersion: v1
kind: Service
metadata:
  name: mongo
spec:
  ports:
  - port: 27017
  selector:
    app: mongo
```

### 2. Create a Kubernetes Deployment for the Backend

Create a file named `backend-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: <your-dockerhub-username>/backend:latest
        ports:
        - containerPort: 5000
        env:
        - name: MONGO_URI
          value: "mongodb://mongo:27017/mydatabase"
---
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  ports:
  - port: 5000
  selector:
    app: backend
```

### 3. Create a Kubernetes Deployment for the Frontend

Create a file named `frontend-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: <your-dockerhub-username>/frontend:latest
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  ports:
  - port: 3000
  selector:
    app: frontend
```

## Step 5: Building and Pushing Docker Images

1. **Build the Docker Images**:

   Navigate to the backend and frontend directories and build the Docker images:

   ```bash
   # For Backend
   cd backend
   docker build -t <your-dockerhub-username>/backend:latest .

   # For Frontend
   cd ../frontend
   docker build -t <your-dockerhub-username>/frontend:latest .
   ```

2. **Push the Docker Images to Docker Hub**:

   ```bash
   docker push <your-dockerhub-username>/backend:latest
   docker push <your-dockerhub-username>/frontend:latest
   ```

## Step 6: Deploying to Kubernetes

1. **Apply the MongoDB Deployment**:

   ```bash
   kubectl apply -f mongo-deployment.yaml
   ```

2. **Apply the Backend Deployment**:

   ```bash
   kubectl apply -f backend-deployment.yaml
   ```

3. **Apply the Frontend Deployment**:

   ```bash
   kubectl apply -f frontend-deployment.yaml
   ```

## Step 7: Accessing the Application

1. **Get the Frontend Service URL**:

   If you are using Minikube, you can access the frontend service using:

   ```bash
   minikube service frontend
   ```

   If you are using a cloud provider, you may need to configure an Ingress or use the LoadBalancer service type to access the frontend.

2. **Test the Application**:

   Open your web browser and navigate to the URL provided by the previous command. You should be able to add items to the list and see them displayed.

## Conclusion

In this lesson, you built and deployed a full-stack application using Docker and Kubernetes. You created a React frontend, a Node.js backend, and a MongoDB database, containerized each component, and deployed them to a Kubernetes cluster. This project demonstrates how to integrate the concepts learned throughout the course and provides a foundation for building more complex applications in the future. In the next lesson, we will summarize the key concepts covered throughout the course and discuss next steps for further learning and exploration.
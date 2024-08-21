# Lesson 7: Docker Compose for Multi-Container Applications

## Introduction to Docker Compose

**Docker Compose** is a powerful tool that simplifies the management of multi-container Docker applications. It allows you to define and run multiple containers as a single service using a simple YAML configuration file. With Docker Compose, you can easily orchestrate the deployment of applications that consist of multiple services, such as web servers, databases, and caching systems.

### Benefits of Using Docker Compose

1. **Simplified Configuration**: Docker Compose uses a single `docker-compose.yml` file to define all the services, networks, and volumes required for your application. This makes it easier to manage complex setups.

2. **Multi-Container Management**: You can start, stop, and manage multiple containers with a single command, streamlining your workflow.

3. **Environment Consistency**: Docker Compose ensures that your application runs in the same environment across different systems, making it easier to develop, test, and deploy.

4. **Service Dependencies**: Compose allows you to define dependencies between services, ensuring that they start in the correct order.

5. **Scaling Services**: You can easily scale services up or down by specifying the number of container instances in the `docker-compose.yml` file.

## Creating a `docker-compose.yml` File

To illustrate how to use Docker Compose, we will create a simple multi-container application that consists of a web server (using Flask) and a Redis database.

### Step 1: Create the Project Directory

1. Open your terminal and create a new directory for your project:

   ```bash
   mkdir my-flask-app
   cd my-flask-app
   ```

### Step 2: Create the Flask Application

2. Create a file named `app.py` with the following content:

   ```python
   from flask import Flask
   import redis

   app = Flask(__name__)
   redis_client = redis.StrictRedis(host='redis', port=6379, decode_responses=True)

   @app.route('/')
   def index():
       visits = redis_client.incr('counter')
       return f'Hello, World! You are visitor number {visits}.'

   if __name__ == '__main__':
       app.run(host='0.0.0.0')
   ```

### Step 3: Create the Requirements File

3. Create a file named `requirements.txt` with the following content:

   ```
   Flask
   redis
   ```

### Step 4: Create the `docker-compose.yml` File

4. In the same directory, create a file named `docker-compose.yml` and add the following content:

   ```yaml
   version: '3.8'

   services:
     web:
       build: .
       ports:
         - "5000:5000"
       depends_on:
         - redis

     redis:
       image: "redis:alpine"
   ```

### Explanation of the `docker-compose.yml` File

- **version**: Specifies the version of the Docker Compose file format.
- **services**: Defines the different services that make up your application.
  - **web**: The Flask web application service.
    - **build**: Specifies that Docker should build the image from the Dockerfile in the current directory.
    - **ports**: Maps port 5000 on the host to port 5000 in the container.
    - **depends_on**: Indicates that the `web` service depends on the `redis` service, ensuring that Redis starts before the web application.
  - **redis**: The Redis service, using the official Redis image from Docker Hub.

### Step 5: Create a Dockerfile

5. Create a file named `Dockerfile` in the same directory with the following content:

   ```dockerfile
   # Use the official Python image from the Docker Hub
   FROM python:3.9-slim

   # Set the working directory in the container
   WORKDIR /app

   # Copy the requirements file and install dependencies
   COPY requirements.txt requirements.txt
   RUN pip install --no-cache-dir -r requirements.txt

   # Copy the application code into the container
   COPY app.py app.py

   # Command to run the application
   CMD ["python", "app.py"]
   ```

## Running the Multi-Container Setup

### Step 1: Build and Start the Services

1. In your terminal, run the following command to build the images and start the services defined in the `docker-compose.yml` file:

   ```bash
   docker-compose up
   ```

   This command will build the web service image and start both the web and Redis services. You should see logs indicating that the services are running.

### Step 2: Access the Application

2. Open your web browser and navigate to `http://localhost:5000`. You should see the message indicating the number of visitors:

   ```
   Hello, World! You are visitor number 1.
   ```

   Refresh the page to see the visitor count increase.

### Step 3: Stopping the Services

3. To stop the services, you can press `Ctrl + C` in the terminal where Docker Compose is running. Alternatively, you can run:

   ```bash
   docker-compose down
   ```

   This command stops and removes the containers defined in the `docker-compose.yml` file.

## Conclusion

In this lesson, you learned about Docker Compose and its benefits for managing multi-container applications. You created a `docker-compose.yml` file to define a simple Flask application and Redis database, and you ran the multi-container setup with a single command. In the next lesson, we will explore advanced Docker Compose features, including environment variables and scaling services.
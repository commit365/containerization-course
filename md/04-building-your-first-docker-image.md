# Lesson 4: Building Your First Docker Image

## Introduction to Dockerfile

A **Dockerfile** is a text file that contains a series of instructions on how to build a Docker image. It defines the base image, application code, dependencies, and any other configurations needed to create the image.

### Key Components of a Dockerfile:
- **FROM**: Specifies the base image to use.
- **COPY**: Copies files from your local filesystem into the image.
- **RUN**: Executes commands in the image during the build process.
- **CMD**: Specifies the command to run when a container is started from the image.

## Step 1: Write a Simple Dockerfile

Let’s create a simple Dockerfile to build an image that runs a basic web application using Python. Follow these steps:

### Create a Project Directory

1. Open your terminal and create a new directory for your project:

   ```bash
   mkdir my-python-app
   cd my-python-app
   ```

### Create a Simple Python Application

2. Create a file named `app.py` in the `my-python-app` directory with the following content:

   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def hello():
       return "Hello, Docker!"

   if __name__ == '__main__':
       app.run(host='0.0.0.0')
   ```

### Create the Dockerfile

3. In the same directory, create a file named `Dockerfile` (without any file extension) and add the following content:

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

   # Expose the port the app runs on
   EXPOSE 5000

   # Command to run the application
   CMD ["python", "app.py"]
   ```

### Create the Requirements File

4. Create a file named `requirements.txt` in the same directory and add the following line:

   ```
   Flask
   ```

## Step 2: Build Your Docker Image

Now that you have your Dockerfile and application code ready, you can build your Docker image.

### Build the Image

1. In your terminal, navigate to the `my-python-app` directory (if you are not already there) and run the following command to build the image:

   ```bash
   docker build -t my-python-app .
   ```

   - The `-t` flag tags the image with a name (`my-python-app`).
   - The `.` at the end specifies the build context (the current directory).

### Verify the Image

2. After the build process completes, verify that your image has been created by running:

   ```bash
   docker images
   ```

   You should see `my-python-app` listed among the available images.

## Step 3: Run Your Docker Image

Now that your image is built, you can run it as a container.

### Run the Container

1. Use the following command to run your Docker container:

   ```bash
   docker run -p 5000:5000 my-python-app
   ```

   - The `-p` flag maps port 5000 on your host machine to port 5000 in the container.

### Access the Application

2. Open your web browser and navigate to `http://localhost:5000`. You should see the message:

   ```
   Hello, Docker!
   ```

## Step 4: Stopping the Container

To stop the running container, you can either press `Ctrl + C` in the terminal where the container is running or open a new terminal and run:

```bash
docker ps
```

Find the container ID or name, then stop it using:

```bash
docker stop <container-id>
```

## Conclusion

In this lesson, you learned how to write a simple Dockerfile to create a custom Docker image and how to build and run that image locally. You now have a basic understanding of how to package applications using Docker. In the next lesson, we will explore Docker networking basics and how containers communicate with each other.
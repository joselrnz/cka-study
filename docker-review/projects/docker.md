# Docker Commands Cheat Sheet

## 🐳 General Commands
```sh
# Check Docker version
docker --version

# Display Docker system information
docker info

# Show all available commands
docker --help
```

## 🏗️ Container Management
```sh
# Run a container from an image
docker run -d -p 8080:80 --name mycontainer myimage

# List running containers
docker ps

# List all containers (including stopped ones)
docker ps -a

# Stop a running container
docker stop <container_id>

# Start a stopped container
docker start <container_id>

# Restart a container
docker restart <container_id>

# Remove a container
docker rm <container_id>

# Remove all stopped containers
docker container prune

# Show container logs
docker logs <container_id>

# Execute a command in a running container
docker exec -it <container_id> bash
```

## 📦 Image Management
```sh
# List all images
docker images

# Pull an image from Docker Hub
docker pull <image_name>

# Build an image from a Dockerfile
docker build -t myimage:latest .

# Tag an image
docker tag myimage myrepo/myimage:latest

# Push an image to a registry
docker push myrepo/myimage:latest

# Remove an image
docker rmi <image_id>

# Remove all unused images
docker image prune
```

## 🖧 Network Management
```sh
# List all networks
docker network ls

# Create a new network
docker network create mynetwork

# Connect a container to a network
docker network connect mynetwork <container_id>

# Disconnect a container from a network
docker network disconnect mynetwork <container_id>

# Remove a network
docker network rm mynetwork
```

## 🗄️ Volume Management
```sh
# List all volumes
docker volume ls

# Create a volume
docker volume create myvolume

# Remove a volume
docker volume rm myvolume

# Inspect a volume
docker volume inspect myvolume

# Remove all unused volumes
docker volume prune
```

## 🏗️ Docker Compose
```sh
# Start services in a detached mode
docker-compose up -d

# Stop services
docker-compose down

# View running services
docker-compose ps

# Restart services
docker-compose restart

# Show logs of services
docker-compose logs
```

## ⚙️ Advanced Docker Commands
```sh
# Show resource usage statistics
docker stats

# Inspect container details
docker inspect <container_id>

# Export a container’s filesystem as a tar archive
docker export <container_id> -o mycontainer.tar

# Import a tar archive as an image
docker import mycontainer.tar mynewimage

# Save an image as a tar archive
docker save -o myimage.tar myimage:latest

# Load an image from a tar archive
docker load -i myimage.tar
```

## 🧹 Cleanup Commands
```sh
# Remove all stopped containers, unused networks, and dangling images
docker system prune

# Remove everything, including volumes (use with caution)
docker system prune -a --volumes
```


## 📝 Dockerfile Example
```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME=World

# Define the entry point script
ENTRYPOINT ["python"]

# Define default arguments for the container
CMD ["app.py"]
```

### Explanation of Dockerfile
1. **FROM python:3.8-slim** - Defines the base image as Python 3.8 (slim variant for minimal size).
2. **WORKDIR /app** - Sets `/app` as the working directory inside the container.
3. **COPY . /app** - Copies all files from the host directory to the container.
4. **RUN pip install --no-cache-dir -r requirements.txt** - Installs dependencies from `requirements.txt`.
5. **EXPOSE 80** - Informs Docker that the container listens on port 80.
6. **ENV NAME=World** - Sets an environment variable inside the container.
7. **ENTRYPOINT ["python"]** - Defines the executable that will always be run first when the container starts.
8. **CMD ["app.py"]** - Defines the default argument for the ENTRYPOINT.

### CMD vs ENTRYPOINT
- **CMD** provides default arguments for the container, which can be overridden at runtime.
  ```sh
  docker run myimage another_script.py
  ```
  This runs `python another_script.py` inside the container instead of `python app.py`.
- **ENTRYPOINT** defines the executable, making the container behave like a specific application.
  ```sh
  docker run myimage --version
  ```
  This runs `python --version` inside the container because `ENTRYPOINT` is `python`.
- **If both CMD and ENTRYPOINT are defined**, CMD acts as the default argument for ENTRYPOINT.

This Dockerfile is used to containerize a Python application, making it portable and easily deployable.




# More Dockerfile Examples with ENTRYPOINT and CMD

## **Example 1: Using CMD to Pass Arguments Dynamically**

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 80

ENV NAME=World

ENTRYPOINT ["python", "app.py"]

# CMD defines default arguments
CMD ["--host=0.0.0.0", "--port=8080"]
```

### **How it Works**
- **ENTRYPOINT**: Runs `python app.py` by default.
- **CMD**: Supplies optional arguments to `app.py`.  
  Example:
  ```sh
  docker run myimage
  ```
  Runs:
  ```sh
  python app.py --host=0.0.0.0 --port=8080
  ```
- You can **override CMD** at runtime:
  ```sh
  docker run myimage --host=127.0.0.1 --port=5000
  ```
  Runs:
  ```sh
  python app.py --host=127.0.0.1 --port=5000
  ```

## **Example 2: Using an ENTRYPOINT Script for More Flexibility**

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 80

ENV NAME=World

# Use an entrypoint script to allow more flexibility
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["app.py", "--host=0.0.0.0", "--port=8080"]
```

### **entrypoint.sh**
```sh
#!/bin/sh
exec python "$@"
```

### **How it Works**
- `entrypoint.sh` executes `python` with any given arguments.
- The default `CMD` is `app.py --host=0.0.0.0 --port=8080`, but you can override it:
  ```sh
  docker run myimage script.py --arg1 --arg2
  ```
  Runs:
  ```sh
  python script.py --arg1 --arg2
  ```

## **Example 3: Combining ENTRYPOINT and CMD for Argument Parsing**

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY . /app

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 80

ENV NAME=World

ENTRYPOINT ["python"]

CMD ["app.py", "--host=0.0.0.0", "--port=8080"]
```

### **How it Works**
- **ENTRYPOINT** is set to `python`, meaning it always runs Python.
- **CMD** provides the script and default arguments.
- Override behavior at runtime:
  ```sh
  docker run myimage script.py --debug
  ```
  Runs:
  ```sh
  python script.py --debug
  ```


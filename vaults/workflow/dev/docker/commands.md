
# ![[docker.png]] Basic Docker commands

Certainly! Here are short notes on the Docker commands you've learned in Markdown format:

### Docker Build

The `docker build` command is used to build a Docker image from a Dockerfile. It allows you to create a custom image for your application.

```sh
docker build -t my-web-container-image .
```

### Docker Image List

The `docker image ls` command lists all Docker images that are available on your system. It provides information about each image, such as its repository, tag, image ID, and creation date.

```sh
docker image ls
```

### Docker Containers (Running)

The `docker ps` command lists currently active (running) Docker containers. It shows details about the running containers, including their container ID, image, and status.

```sh
docker ps
```

### Docker Containers (All)

The `docker ps -a` command lists all Docker containers, including both running and stopped containers. It provides a comprehensive view of all containers on your system.

```sh
docker ps -a
```

### Docker Run

The `docker run` command is used to start a new Docker container based on an image. It allows you to specify various options, such as port mapping, volume mounting, and container naming.

```sh
docker run -v $(pwd):/app -p 3000:3000 -d --name my-web-container my-web-container-image
```

### Docker Stop

The `docker stop` command is used to stop a running Docker container gracefully. It sends a termination signal to the container's processes, allowing them to shut down properly.

```sh
docker stop my-web-container
```

### Docker Remove (Force)

The `docker rm -f` command is used to forcefully remove a Docker container. It stops the container (if running) and removes it, including its data.

```sh
docker rm my-web-container -f
```

### Docker Remove Image

The `docker image rm` command is used to remove a Docker image from your system. It's helpful when you no longer need a specific image.

```sh
docker image rm my-web-container-image
```

### Docker Exec

The `docker exec` command is used to execute a command inside a running Docker container. It allows you to access the container's shell or run specific commands within the container's environment.

```sh
docker exec -it my-web-container bash
```

### Docker logs

```sh
docker logs my-web-container
```

- Purpose: View the logs of a running or stopped container named `my-web-container`.
- Use Case: Troubleshoot container issues and view application logs.

### Docker run
 ```sh
docker run -v $(pwd):/app -v /app/node_modules -p 3000:3000 -d --name my-web-container my-web-container-image
 ```
- Purpose: Run a Docker container named `my-web-container` from the `my-web-container-image` image.
- Use Case: Start a container, map ports, mount local code, and create an anonymous volume for node_modules.

 ```sh
docker run -v $(pwd):/app:ro -v /app/node_modules -p 3000:3000 -d --name my-web-container my-web-container-image
```
- Purpose: Run a Docker container with read-only mount for local code.
- Use Case: Start a container with read-only access to the local codebase and a node_modules volume.

 ```sh
docker run -v $(pwd):/app:ro -v /app/node_modules --env-file ./.env -p 3000:4000 -d --name my-web-container my-web-container-image
 ```
- Purpose: Run a Docker container with environment variables and port mapping.
- Use Case: Start a container with environment variables loaded from an .env file and port mapping.

### Docker volume ```
```sh
docker volume ls
```

- Purpose: List all Docker volumes on the host.
- Use Case: Check available volumes on the host system.

 ```sh
docker rm my-web-container -fv
```
- Purpose: Remove a Docker container named `my-web-container` forcefully.
- Use Case: Delete a container, including its associated volumes.

```sh
docker volume prune
```
- Purpose: Remove all unused Docker volumes.
- Use Case: Clean up unused volumes to free up disk space.

### Docker Compose Commands:
```sh
docker-compose up -d
```
- Purpose: Start Docker Compose services in detached mode.
- Use Case: Start services defined in a `docker-compose.yml` file in the background.

```sh
docker-compose down -v
```
- Purpose: Stop and remove Docker Compose services and associated volumes.
- Use Case: Gracefully stop services and remove volumes defined in a `docker-compose.yml` file.

```sh
docker-compose up -d --build
```
- Purpose: Start Docker Compose services in detached mode, rebuilding images if necessary.
- Use Case: Rebuild and restart services with updated configurations.

```sh
docker-compose -f ./docker-compose.yml -f ./docker-compose.dev.yml up -d --build
```
- Purpose: Start Docker Compose services from multiple YAML files, in detached mode, rebuilding images if necessary.
- Use Case: Start services defined in multiple `docker-compose` files, e.g., for different environments.

```sh
docker-compose -f docker-compose.yml -f docker-compose.dev.yml down -v
```
- Purpose: Stop and remove Docker Compose services and associated volumes defined in multiple YAML files.
- Use Case: Gracefully stop services and remove volumes for different environments.
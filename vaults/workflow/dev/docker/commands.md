
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


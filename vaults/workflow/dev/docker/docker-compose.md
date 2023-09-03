# Docker-compose commands

```yml
version: "3"
services:
  my-web-container:
    build: .
    ports:
      - "3000:4000"
    volumes:
      - .:/app
      - /app/node_modules
    env_file:
      - ./.env
```
same as 
```sh
1. docker build -t my-web-container .

2. docker run -v $(pwd):/app:ro -v /app/node_modules --env-file ./.env -p 3000:4000 -d --name my-web-container my-web-container-image
```

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
- Purpose: Stop and remove Docker Compose services and associated volumes defined in multiple YAML files. -v flag will delete all anon volumes as well as all named volumes, 
  so don't use it when using named volumes that persist data, if persistence is required. 
  instead  use 
  ```sh
  docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
  docker volume prune
```

- Use Case: Gracefully stop services and remove volumes for different environments.




Certainly! Here are short notes on the Docker commands you've learned in Markdown format:

### Docker Inspect

The `docker inspect` command provides detailed information about Docker objects such as containers, images, volumes, or networks. It's a versatile tool for examining the configuration and attributes of these objects.

**Usage:**

```sh
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
```

**Example:**

```sh
docker inspect my-container
```

- Use this command to retrieve detailed information about a specific Docker object.
- The result is presented in JSON format, making it easy to extract specific details programmatically.

### Docker Network List

The `docker network ls` command lists all Docker networks available on your system. Docker networks facilitate communication between containers and can be essential for connecting different services within a Docker ecosystem.

**Usage:**

```sh
docker network ls [OPTIONS]
```

- Use this command to view a list of Docker networks, including their IDs, names, and driver types.
- Helpful for managing container communication and networking configurations.

### Docker Compose (start single container without its deps)

Docker Compose is a tool for defining and running multi-container Docker applications using a YAML file for configuration. It simplifies complex setups, allowing you to define services, networks, and volumes in a single file.

**Usage:**

```sh
docker-compose -f FILE [OPTIONS] [COMMAND] [ARGS...]
```

**Example:**

```sh
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d --no-deps
```

- Docker Compose helps orchestrate containers by defining services, dependencies, and network configurations in a YAML file.
- `-f` specifies the Docker Compose configuration file.
- `up -d` starts the defined services as detached (in the background).
- `--no-deps` starts services without recreating dependent containers.

These commands are essential for managing Docker containers, networks, and multi-container applications efficiently. Docker Compose, in particular, simplifies the deployment of complex applications by defining their structure in a single configuration file.


### Scaling the application

```shell
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build --scale blog-server=2
```

```shell
# Docker Compose Command Notes

The following Docker Compose command is used to start a multi-container application with specific configuration files and scaling for a development environment.

# Command:

```shell
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build --scale blog-server=2
```

#### Explanation:

- `docker-compose`: This command is used to manage multi-container Docker applications defined in a Compose file.

- `-f docker-compose.yml -f docker-compose.dev.yml`: Specifies the Compose files to use for configuring the services. In this case, it combines configurations from both `docker-compose.yml` and `docker-compose.dev.yml`.

- `up`: Starts the services defined in the Compose file.

- `-d`: Runs the services in detached mode, which means they will run in the background, and the terminal remains available for other commands.

- `--build`: Forces a rebuild of the service images before starting containers. This is useful when you want to ensure that your images are up-to-date with the latest code changes.

- `--scale blog-server=2`: Specifies that the `blog-server` service should be scaled to have two instances running. This is commonly used for load balancing or testing.

#### Use Cases:

- This command is typically used during development when you want to run multiple instances of a service for testing or debugging purposes.

- It's useful for simulating a load-balanced environment where multiple instances of a service handle incoming requests.

- The `--build` flag ensures that any code changes in your services are reflected in the containers.

- Be aware that running multiple instances of a service may require adjustments in your application code to handle load balancing or distributed configurations effectively.


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
- Purpose: Stop and remove Docker Compose services and associated volumes defined in multiple YAML files.
- Use Case: Gracefully stop services and remove volumes for different environments.

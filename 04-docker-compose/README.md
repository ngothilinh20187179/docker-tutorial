# Docker Compose
Docker Compose is a tool that allows you to define and run multi-container applications.

## How it Works

Instead of running multiple long `docker run` commands manually, you define your entire application stack in a single file called `docker-compose.yml`. This file uses YAML (Yet Another Markup Language) syntax.

With one command `docker-compose up` Docker reads that file and:
- Pulls the necessary images.
- Creates the required networks.
- Mounts the specified volumes.
- Starts all your containers in the correct order.

Inside a `docker-compose.yml` file, you typically define three main things:
- Services: These are your containers (e.g., a "frontend" service, a "backend" service, and a "database" service).
- Networks: These allow your services to communicate with each other. By default, Compose creates a single network for your app, and each container can find others simply by their service name (e.g., the web app can connect to db:5432).
- Volumes: These are used to persist data. Even if you stop and delete your containers, the data in the volumes remains safe.

### A Simple Example
Here is what a basic Compose file looks like for a Web App and a Database:
```bash
# docker-compose.yml / docker-compose.yaml
version: '3.8'
services:
  web:
    build: .             # Build the Dockerfile in the current directory
    ports:
      - "5000:5000"      # Map host port 5000 to container port 5000
  db:
    image: postgres:15   # Use a pre-built image from Docker Hub
    volumes:
      - db-data:/var/lib/postgresql/data
volumes:
  db-data:               # Define the persistent volume
```
```bash
docker-compose up
```

If you don't use Compose, what do you do?
```bash
# Create Volume
docker volume create db-data

# Create Network
docker network create my-app-net

# Create Container Database
docker run -d \
  --name db \
  --network my-app-net \
  -v db-data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=your_password \
  postgres:15

# Build image, run container
docker build -t my-web-app .
docker run -d \
  --name web \
  --network my-app-net \
  -p 5000:5000 \
  my-web-app
```

### 🐳 Docker Compose Cheat Sheet

#### 1. Lifecycle Management

* `docker-compose up`: Create and start all services defined in the `.yml` file.
* `docker-compose up -d`: Start containers in **detached mode** (runs in the background).
* `docker-compose up --build`: Rebuild images before starting containers.
* `docker-compose start`: Start services that were already created but stopped.
* `docker-compose stop`: Stop running containers without removing them.
* `docker-compose restart`: Restart all services.
* `docker-compose down`: Stop and **remove** containers, networks, and images created by `up`.
* `docker-compose down -v`: Stop and remove everything, including **Volumes** (Warning: This deletes your database data).

#### 2. Monitoring & Debugging

* `docker-compose ps`: List the status of all containers in the current project.
* `docker-compose logs`: View output logs from all services.
* `docker-compose logs -f`: Stream live logs (follow) from all containers.
* `docker-compose logs -f <service_name>`: Follow logs for a specific service (e.g., `db` or `web`).
* `docker-compose top`: Display the running processes inside each container.
* `docker-compose images`: List images used by the current project.

#### 3. Execution & Interaction

* `docker-compose exec <service_name> <command>`: Run a command in a **running** container (e.g., `docker-compose exec web sh`).
* `docker-compose run <service_name> <command>`: Run a **one-off** command for a service (commonly used for migrations or installing dependencies).
* `docker-compose build`: Only build or rebuild images without starting containers.

#### 4. Configuration & Maintenance

* `docker-compose config`: Validate and view the rendered Compose file configuration.
* `docker-compose pause`: Pause all processes within the containers.
* `docker-compose unpause`: Resume paused processes.
* `docker-compose pull`: Pull images for services defined in the file without starting them.

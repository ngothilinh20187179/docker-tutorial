# Docker Network
The Docker Network is a subsystem of Docker that allows Containers to communicate with each other and with the outside world (Internet/Physical Server).

DNS (Domain Name System)

# Key Commands and Usage

```bash
docker network ls
docker network create my-secure-net
docker run -d --name my-app --network my-secure-net my-image
docker network connect my-secure-net existing-container
```

# Practice: Basic Connection: Web - Database
Problem: You have a Web application (Nodejs,..) and a database (Mongo, mysql,...).
Requirements: The Web must be able to connect to the DB, Database is not allowed to be exposed to the Internet to ensure safety.

```bash
docker network create my-app-net
```

```bash
docker run -d \
  --name db-server \
  --network my-app-net \
  -e MYSQL_ROOT_PASSWORD=secret \
  mysql:8.0
```

```bash
docker run -d \
  --name web-server \
  --network my-app-net \
  -p 8080:80 \
  nginx:alpine
```

```bash
# Test
docker exec -it web-server ping db-server
```

# Practice: Security Settings
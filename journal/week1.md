# Week 1 — App Containerization
## Docker for Cruddur

Build and run a docker images specifying name and tag (by default tag is `latest` if not specified):
```
docker run -t {name}:{tag} ./backend-flask
```

See all images with their tags:
```
docker images
```

Sets env var in container manually
```
docker run --rm -p 4567:4567 -e FRONTEND_URL='*' -it backend-flask
```

## Docker components
- Docker client (local machine); communicates via REST API with...
- Docker Server, contains
    - Docker deamon
    - Docker registry
        - public/private images

## Managed container services
Scalling with Docker and compose is very cumbersome and disruptive to service (need to bring down container, update, bring it up again). To bridge this, we need a managed container service that takes care of auto-scaling.


# Week 1 - Container Security
Protect software inside containers, like Single Page Applications (SPAs), Microservices, APIs.

## Security perspective on containers
Things to consider
- Docker & host configuration
- security of images (scanning for vulnerabilities)
- secret management
- app security
- data security
- monitoring containers
- compliance framework

## Best practices (TOP 10)
General rule of thumb: a container should only contain as little information and functionality as needed.

1. Keep hopst & Docker updated to latest security patches
2. Docker daemon & containers should run in **non-root user** mode
3. image vulnerability scanning
4. trusting a private vs. public image registry
5. no sensitive data in Focker files or images
6. use secret management services to share secrets
7. read only file system and volume for Docker
8. separate databases for long-term storage
9. use DevSecOps practices while building application security
10. ensure all code is tested for bulnerabilities before production use

## Useful tools
- Snyk -> scan vulnerabilities in repositories and containers
- AWS Secrets Manager (if hosted in cloud environment!) -> paid service
- Hashicorp Vault (free secret manager)
- AWS Amazon Inspector
- clair (open-source, scanning images)

## DevSecOps practices ??
TB Researched

# Week1 - Docker Basics

## Data
Generally, all data stored inside a container (and all other configuration that was added at container runtime) vanishes once the container stops. There are 2 options, however, of persisting data with containers:
1. Volume mount -> dedicates storage within the container to storage (default config)
2. Bind mount -> reserves space on host machine to store data (usually slower but easier to get visibility of files created/edited within the container)

Here's an experiment to illustrate how data disappears (using the `--rm` flag):

```bash
docker run -it --rm ubuntu:22.04

# inside of the container:
mkdir my-data
echo "Hello, container!" > /my-data/hello.txt

# access content
cat /my-data/hello.txt
exit
```

Once the container stops and we bring up a container with the same image (ubuntu), the data will not be available any longer. On the contrary, if the container is named, stopped and started again, the data will remain (until the container is removed from the local environment) - no permanent persistency!

i) Volume Mount

To mount to a volume, we need to create a volume and attach it to the container when we create it:
```bash
docker volume create my-volume
# create container with the volume
docker run -it --rm --mount source=my-volume,destination=/my-data/ ubuntu:22.04
# alternative command
docker run -it --rm -v my-volume:/my-data ubuntu:22.04

# create data
echo "Hello, container!" > /my-data/hello.txt

exit
```
If we create another container mounting the same volume, the data will be accessible still. It is stored in the VM file system inside the defined volume.

Example of how the volume can used to store data of databases at its know path:

```bash
# Create a container from the postgres container image and mount its known storage path into a volume named pgdata
docker run -it --rm -v pgdata:/var/lib/postgresql/data -e POSTGRES_PASSWORD=foobarbaz postgres:15.1-alpine
```

ii) Bind Mount

The bind mount needs to exist locally on host. All changes will be visible/available within that directory on the local host.
```bash
docker run -it --rm --mount type=bind,source=${PWD}/my-data,destination=/my-data ubuntu:22.04
```

**Note**: heavy read/write operations suffer from performance using a bind mount.

## Container use cases
Container images can be used to quickly and easily setup an environment of desired versioning. This is especially useful for prototyping, when those environments are not accessible locally, or when we want to test changes of behavior between two versions of an environment (e.g. breaking change from Python 3.11 to 3.12).


# Week1 - Docker Practice

## Containers for databases

The following Dockerfile spins up a container from a PostgreSQL image (most basic setup):

```Dockerfile
FROM postgres:latest

# Set environment variables for PostgreSQL
# (replace with your database name, user, and password)
ENV POSTGRES_DB=mydatabase
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=password
```

**Note**: the image `latest` will always be automatically updated. It is strongly advised to use images of a specific postgres version to avoid any issues/breaks due to version incompatibilities!

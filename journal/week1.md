# Week 1 — App Containerization


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
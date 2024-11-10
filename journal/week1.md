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
# Docker Best Practices

- Keep containers stateless.
- Use COPY instead of ADD.
- Make COPY last line before CMD or ENTRYPOINT.
  - Each line in the Dockerfile is cached.
  - Separate COPY of requirements.txt from source code.
- CMD vs ENTRYPOINT: ENTRYPOINT is the main command. Treat
CMD as the default flag an entrypoint. Example: 
```
ENTRYPOINT ["s3cmd"]
CMD ["--help"]
```
- Bind to 0.0.0.0. Otherwise you get issue with Kubernetes.
- Make services have healthcheck endpoints.
- Switch to non-root-user
```
RUN groupadd -r myapp && useradd -r -g myapp myapp
USER myapp
```
- Log everything to STDOUT.
- One process per container.
- Limit access from network. Example:
```
docker network create --driver bridge isolated_nw
docker run --network=isolated_nw --name=container busybox
```
- Support SIGTERM, SIGKILL and SIGINT to exit gracefully.
- 1 Process per container.
  - No supervisord, uWSGI 1 worker.


## Resources
- https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/
- https://github.com/FuriKuri/docker-best-practices
- [3 tricks for mastering Docker with Python](https://hackernoon.com/3-tricks-for-mastering-docker-with-python-99876412348d#.aljoaimgx)

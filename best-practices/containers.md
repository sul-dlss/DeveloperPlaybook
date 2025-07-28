# Guide for DLSS Developers working with containers

The infrastructure team has been utilizing docker containers for local developement,
particularly when supporting external dependencies for somet time......

Most of this guide will focus on docker compose...

## Docker

### Installing docker

Install docker desktop for your local operating system:
- [MacOS](https://docs.docker.com/desktop/mac/install/)
- [Linux](https://docs.docker.com/desktop/linux/install/)

### Working with docker compose

### List running services

This is what you should expect when exeuting the `docker compose ps` command when running the argo docker compose file.

```
❯ docker compose ps
NAME                          COMMAND                  SERVICE                STATUS              PORTS
argo-db-1                     "docker-entrypoint.s…"   db                     running             5432/tcp
argo-dor-indexing-app-1       "./docker/invoke.sh"     dor-indexing-app       running             0.0.0.0:3004->3000/tcp
argo-dor-indexing-workers-1   "sh -c 'timeout 30 d…"   dor-indexing-workers   running             
argo-dor-services-app-1       "./docker/invoke.sh"     dor-services-app       running             0.0.0.0:3003->3000/tcp
argo-rabbitmq-1               "docker-entrypoint.s…"   rabbitmq               running             0.0.0.0:5672->5672/tcp
argo-redis-1                  "docker-entrypoint.s…"   redis                  running             0.0.0.0:6379->6379/tcp
argo-sdr-api-1                "./docker/invoke.sh"     sdr-api                running             0.0.0.0:3006->3000/tcp
argo-solr-1                   "docker-entrypoint.s…"   solr                   running             0.0.0.0:8983->8983/tcp
argo-suri-1                   "./docker/invoke.sh"     suri                   running             0.0.0.0:3002->3000/tcp
argo-techmd-1                 "./docker/app/invoke…"   techmd                 running             0.0.0.0:3005->3000/tcp
argo-web-1                    "docker/entrypoint.sh"   web                    running             0.0.0.0:3000->3000/tcp
argo-workflow-1               "./docker/invoke.sh"     workflow               running             0.0.0.0:3001->3000/tcp
```

Note that provides you with the service name and status, but not the container id. In some cases, the service name is used for connecting to running containers, and other times we need the container id (this is unique to the current running instance of the particular image). We can get that information from a list of running containers.

### List running containers

This is what you should expect when executing the `docker ps` command when running the argo docker compose file.

```
❯ docker ps
CONTAINER ID   IMAGE                                       COMMAND                  CREATED          STATUS          PORTS                                                                    NAMES
499bb54d4e92   suldlss/sdr-api:latest                      "./docker/invoke.sh"     13 seconds ago   Up 10 seconds   0.0.0.0:3006->3000/tcp                                                   argo-sdr-api-1
4bb45a7f2fc3   argo_web                                    "docker/entrypoint.sh"   13 seconds ago   Up 10 seconds   0.0.0.0:3000->3000/tcp                                                   argo-web-1
13cd8256e497   suldlss/dor-services-app:latest             "./docker/invoke.sh"     13 seconds ago   Up 10 seconds   0.0.0.0:3003->3000/tcp                                                   argo-dor-services-app-1
5aa11193a9e2   suldlss/dor-indexing-app:latest             "sh -c 'timeout 30 d…"   13 seconds ago   Up 11 seconds                                                                            argo-dor-indexing-workers-1
5e8dde10b243   suldlss/dor-indexing-app:latest             "./docker/invoke.sh"     13 seconds ago   Up 11 seconds   0.0.0.0:3004->3000/tcp                                                   argo-dor-indexing-app-1
55523ad18d44   suldlss/suri-rails:latest                   "./docker/invoke.sh"     13 seconds ago   Up 11 seconds   0.0.0.0:3002->3000/tcp                                                   argo-suri-1
d42fbb887987   suldlss/technical-metadata-service:latest   "./docker/app/invoke…"   13 seconds ago   Up 11 seconds   0.0.0.0:3005->3000/tcp                                                   argo-techmd-1
2446e8d98540   suldlss/workflow-server:latest              "./docker/invoke.sh"     13 seconds ago   Up 11 seconds   0.0.0.0:3001->3000/tcp                                                   argo-workflow-1
04e9f7e8e033   solr:7                                      "docker-entrypoint.s…"   13 seconds ago   Up 12 seconds   0.0.0.0:8983->8983/tcp                                                   argo-solr-1
114379aa9d4d   rabbitmq:3                                  "docker-entrypoint.s…"   13 seconds ago   Up 12 seconds   4369/tcp, 5671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:5672->5672/tcp   argo-rabbitmq-1
7c9814b43a3e   redis                                       "docker-entrypoint.s…"   13 seconds ago   Up 12 seconds   0.0.0.0:6379->6379/tcp                                                   argo-redis-1
2e6089420ebd   postgres:11                                 "docker-entrypoint.s…"   13 seconds ago   Up 12 seconds   5432/tcp                                                                 argo-db-1
```

### Viewing log files of a running container

[This blog post](https://www.papertrail.com/solution/tips/how-to-live-tail-docker-logs/) has some excellent examples and tricks when working with docker based logs.

#### Logs per docker compose service

```
docker compose logs [SERVICE NAME]
```

#### Logs per docker container ID

```
docker logs [CONTAINER ID]
```

Both of the above options support the `--follow` and `--tail N` flags.
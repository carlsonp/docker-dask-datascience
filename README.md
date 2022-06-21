# docker-dask-datascience

Dask, Jupyter Notebook, and other Data Science tooling via Docker Swarm

Uses [dask-docker](https://github.com/dask/dask-docker) as a starting point.

## Requirements

* Docker
* docker-compose
* Existing Docker registry for storing images
* Existing caching layer for .deb packages

## Setup

Edit the `.env` file.

https://docs.docker.com/engine/swarm/swarm-mode/

```shell
docker swarm init --advertise-addr=192.168.1.113 --listen-addr=192.168.1.113:2377
```

Setup across all nodes using provided token and command.  Windows cannot
act as a manager, only a worker.

Nodes:

* 192.168.1.113 - Asus-Blue (manager)
* 192.168.1.145 - Windows WSL2 (worker)
* 192.168.1.105 - Alienware (worker)
* 192.168.1.124 - Laptop (worker)

https://docs.docker.com/engine/swarm/manage-nodes/
Check the status of the swarm cluster

```shell
docker node ls
```

https://docs.docker.com/engine/swarm/stack-deploy/

Use existing Docker image registry

Add the following to your `daemon.json` Docker file:

```file
"insecure-registries": ["192.168.1.226:5000"]
```

```shell
docker service ls
```

Deploy:

```shell
./build-deploy.sh
```

Check the status of the stack

```shel
docker stack ls
docker stack services dask
```

Get the full details

```shell
docker stack ps dask --no-trunc
```

Open up the Dask web-ui

http://localhost:8787

Open up the Jupyter web-ui

http://localhost:8888

## Teardown

Bring everything down

```shell
docker stack rm dask
docker swarm leave --force
```

## Updates

Download newer upstream versions by running `./build-deploy.sh`

## Debugging

* `nload` - for live network usage
* `htop` - for live CPU and RAM usage

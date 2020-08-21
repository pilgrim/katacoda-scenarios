
## Deploying Your First Docker Container

The Docker CLI has a command called run which will start a container based on a Docker Image. The structure is docker run `<options> <image-name>`.

`docker run -d redis'`{{execute}}

##  Finding Running Containers

`docker ps'`{{execute}}

## Persisting Data
`docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis`{{execute}}

## Running A Container In The Foreground

Using `docker run -it ubuntu bash`{{execute}} 


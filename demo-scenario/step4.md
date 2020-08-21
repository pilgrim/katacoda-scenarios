
Communicating Between Containers


## Start Redis

Create a Data Container for storing configuration files using

`docker run -d --name redis-server redis`{{execute}}

##  Create Link
How Links Work

You can output all the environment variables with the env command. For example:

`docker run --link redis-server:redis alpine env`{{execute}}

Secondly, Docker will update the HOSTS file of the container with an entry for our source container with three names, the original, the alias and the hash-id. You can output the containers host entry using cat /etc/hosts


`docker run --link redis-server:redis alpine cat /etc/hosts`{{execute}}

`docker run --link redis-server:redis alpine ping -c 1 redis`{{execute}}
## Connect To App

`docker run -d -p 3000:3000 --link redis-server:redis katacoda/redis-node-docker-example`{{execute}}
`curl docker:3000`{{execute}}

## Connect to Redis CLI

The command below will launch an instance of the Redis-cli tool and connect to the redis server via its alias.

`docker run -it --link redis-server:redis redis redis-cli -h redis`{{execute}} 


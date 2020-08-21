### docker-compose

#### Create package.json

```
{
    "name": "scrapbook-redis-node-docker-example",
    "version": "0.0.0",
    "private": true,
    "scripts": {
      "start": "node server.js"
    },
    "dependencies": {
      "redis": "^0.12.1"
    }
  }
  
```
#### Create server.js

```
// Inspired by https://github.com/mranney/node_redis/blob/master/examples/web_server.js

var http = require("http");
var redis_client = require("redis").createClient(6379, 'redis');

var server = http.createServer(function (request, response) {
  response.writeHead(200, {
    "Content-Type": "text/plain"
  });

  var total_requests;

  redis_client.incr("requests", function (err, reply) {
    total_requests = reply; // stash response in outer scope
  });
  redis_client.hincrby("ip", request.connection.remoteAddress, 1);
  redis_client.hgetall("ip", function (err, reply) {
    // This is the last reply, so all of the previous replies must have completed already
    response.write("This page was generated after talking to redis.\n\n" +
                   "Total requests: " + total_requests + "\n\n" +
                   "IP count: \n");
    Object.keys(reply).forEach(function (ip) {
      response.write("    " + ip + ": " + reply[ip] + "\n");
    });
    response.end();
  });
}).listen(3001);
console.log('Listening on port 3001');

```

#### Create Dockerfile
```
FROM ocelotuproar/alpine-node:5.7.1-onbuild
EXPOSE 3000
```

#### Define docker-compose.yaml

Create with following content

```
web:
  build: .

  links:
    - redis

  ports:
    - "3000"
    - "8000"

redis:
  image: redis:alpine
  volumes:
    - /var/redis/data:/data

```


#### Docker build

Task
Launch your application using `docker-compose up -d`{{execute}}


#### Docker Management
Not only can Docker Compose manage starting containers but it also provides a way manage all the containers using a single command.

For example, to see the details of the launched containers you can use `docker-compose ps`{{execute}}

To access all the logs via a single stream you use `docker-compose logs`{{execute}}
or to acces specific logs by a service  `docker-compose logs web`{{execute}}

Other commands follow the same pattern. Discover them by typing docker-compose.

####  Docker Scale
As Docker Compose understands how to launch your application containers, it can also be used to scale the number of containers running.

The scale option allows you to specify the service and then the number of instances you want. If the number is greater than the instances already running then, it will launch additional containers. If the number is less, then it will stop the unrequired containers.

Task
Scale the number of web containers you're running using the command `docker-compose scale web=3`{{execute}}

You can scale it back down using `docker-compose scale web=1`{{execute}}

#### Docker Stop
As when we launched the application, to stop a set of containers you can use the command `docker-compose stop`{{execute}}.

To remove all the containers use the command `docker-compose rm`{{execute}}.
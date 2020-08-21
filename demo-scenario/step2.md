
## Define  Image

Create index.html

`cat <<EOF >> index.html
Hello Docker From Nginx
EOF`{{execute}}

Create Dockerfile

`cat <<EOF >> Dockerfile
FROM nginx:1.11-alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
EOF`{{execute}}



##  Build Image

`docker build -t my-nginx-image:latest . `{{execute}}

## Launch
`docker run -d -p 80:80 my-nginx-image:latest`{{execute}}

## Testing

Using `curl -i http://docker`{{execute}} 



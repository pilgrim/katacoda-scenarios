
## Data container

Create a Data Container for storing configuration files using

`docker create -v /config --name dataContainer busybox`{{execute}}

##  Copy Files
`cat <<EOF >> config.conf
value=Just a config
EOF`{{execute}}

`docker cp config.conf dataContainer:/config/`{{execute}}

## Step 3 - Mount Volumes From
`docker run --volumes-from dataContainer ubuntu ls /config`{{execute}}

## Step 4 - Export / Import Containers

If we wanted to move the Data Container to another machine then we can export it to a .tar file.

`docker export dataContainer > dataContainer.tar`{{execute}} 

The command `docker import dataContainer.tar`{{execute}}  will import the Data Container back into Docker.

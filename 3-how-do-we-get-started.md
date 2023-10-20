# How do we get started?

## Toolset

Please install Docker Desktop (https://www.docker.com/products/docker-desktop) or Rancher Desktop (https://docs.rancherdesktop.io/getting-started/installation/) before the workshop if you plan to follow along. You can check Docker Desktop license information at https://www.docker.com/pricing/faq/ where they list that use is free for research at non-for-profit institutions (last checked 2023-10-19).

## Hello World httpd example

Run our first container

```sh
docker run --help

docker run --rm --publish 8080:80 httpd
# or equivalently: docker run --rm -p 8080:80 httpd

# Browse to http://localhost:8080/

# Use `Ctrl-C` to stop the process, which also deletes the container.

# Let's check what containers exist
docker ps --all
# or equivalently: docker ps -a

docker run --detach --name webster --publish 8080:80 httpd
# or equivalently: docker run -d --name webster -p 8080:80 httpd

# View the logs
docker logs --follow --tail 100 webster
# or equivalently: docker logs -f -n 100 webster
```

Let's add in mount points. From now on I'll mostly use short hand commands.

```sh
# We can't change anything about an existing container
# We need to start over with a new one

# Stop our existing container
docker stop webster

# Delete it
docker rm webster

# If we wanted to stop and delete it with one command, the following works
docker rm -f webster

# Create some local HTML content
mkdir -p $HOME/Downloads/webdocs
cat << EOF > $HOME/Downloads/webdocs/index.html
<html>
<head>
  <title>My website</title>
</head>
<body>
  <h1>Welcome to my better website!</h1>
</body>
</html>
EOF

# Run our new container with the mount point
docker run -d \
--name webster \
-p 8080:80 \
-v $HOME/Downloads/webdocs:/usr/local/apache2/htdocs \
httpd

# Tail the logs
docker logs -f webster

# Browse to http://localhost:8080/

# Let's update the website
cat << EOF > $HOME/Downloads/webdocs/index.html
<html>
<head>
  <title>My website</title>
</head>
<body>
  <h1>Welcome to my better website!</h1>
  <p>Containers are fun</p>
</body>
</html>
EOF

# Now let's go inside the container
docker exec -it webster bash

# Install VIM
apt update
apt install -y vim

vi htdocs/index.html

# Add the following line below the <p>
# VIM tips
#   - use the arrow keys to navigate to the line with the <p>
#   - press "o" to create a new line and enter insert mode
#   - copy/paste from below
#   - press escape to exit insert mode
#   - press ":" to enter command mode
#   - press "wq" and then press enter to "write" and "quit"
  <div>
    <img src="https://w7.pngwing.com/pngs/627/244/png-transparent-docker-logo-logos-logos-and-brands-icon-thumbnail.png">
  </div>
```

Let's now build an image

```sh
cd $HOME/Downloads/webdocs

cat << EOF > Dockerfile
FROM httpd

RUN apt update \
 && apt install -y vim

COPY . /usr/local/apache2/htdocs
EOF

docker build -t my-website:2023-10-20 .

# Note, if you're on an ARM64 CPU, you sometimes need to use
# "--platform linux/amd64" with your build command
# Example: docker build --platform linux/amd64 -t my-website:2023-10-20 .

docker run -d \
--name webster2 \
-p 8081:80 \
my-website:2023-10-20

# Browse to http://localhost:8081/

docker exec -it webster2 bash

vi htdocs/index.html

# Add the following line below the <div>
# VIM tips
#   - use the arrow keys to navigate to the line with the <p>
#   - press "o" to create a new line and enter insert mode
#   - copy/paste from below
#   - press escape to exit insert mode
#   - press ":" to enter command mode
#   - press "wq" and then press enter to "write" and "quit"
<a href="#">A link to nowhere</a>

exit

docker rm -f webster2

docker run -d \
--name webster2 \
-p 8081:80 \
my-website:2023-10-20

# Browse to http://localhost:8081/
```

What commands have we learned

- docker run
- docker ps
- docker logs
- docker stop
- docker rm
- docker exec
- docker build

Other common ones are

- docker create
- docker start
- pull
- push
- images
- inspect
- cp
- tag

Let's try a docker-compose example

```sh
cd ~/Downloads

wget https://git.dartmouth.edu/research-itc-public/containerization-workshop/-/archive/main/containerization-workshop-main.zip

unzip containerization-workshop-main.zip

rm -f containerization-workshop-main.zip

cat docker-compose.yml

mkdir todo-mysql-data
sudo chown 999:999 todo-mysql-data
sudo chmod 777 todo-mysql-data

docker-compose up -d

# Note, if you're on an ARM64 CPU
# Error: no matching manifest for linux/arm64/v8 in the manifest list entries
# Fix: after line 6, after "image: mysql:5.7", add
# platform: linux/amd64

docker ps

# Browse to http://localhost:3000/

docker-compose down
```

Next: [Examples](4-examples.md)

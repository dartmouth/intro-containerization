# How do we get started?

## Toolset

Please install Podman Desktop (https://podman.io/docs/installation) before the workshop if you plan to follow along.

## Hello World httpd example

Run our first container

```sh
podman run --help

podman run --rm --publish 8080:80 httpd
# or equivalently: podman run --rm -p 8080:80 httpd

# Browse to http://localhost:8080/

# Use `Ctrl-C` to stop the process, which also deletes the container.

# Let's check what containers exist
podman ps --all
# or equivalently: podman ps -a

podman run --detach --name webster --publish 8080:80 httpd
# or equivalently: podman run -d --name webster -p 8080:80 httpd

# View the logs
podman logs --follow --tail 100 webster
# or equivalently: podman logs -f -n 100 webster
```

Let's add in mount points. From now on I'll mostly use short hand commands.

```sh
# We can't change anything about an existing container
# We need to start over with a new one

# Stop our existing container
podman stop webster

# Delete it
podman rm webster

# If we wanted to stop and delete it with one command, the following works
podman rm -f webster

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
podman run -d \
--name webster \
-p 8080:80 \
-v $HOME/Downloads/webdocs:/usr/local/apache2/htdocs \
httpd

# Tail the logs
podman logs -f webster

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
podman exec -it webster bash

# Install VIM
apt update
apt install -y vim nano

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
    <img src="https://cdn.shortpixel.ai/spai/q_lossy+ret_img+to_auto/linuxiac.com/wp-content/uploads/2024/10/podman-timed-releases-1024x576.jpg" />
  </div>
```

Let's now build an image

```sh
cd $HOME/Downloads/webdocs

cat << EOF > Dockerfile
FROM httpd

RUN apt update \
 && apt install -y vim nano

COPY . /usr/local/apache2/htdocs
EOF

podman build -t my-website:latest .

# Note, if you're on an ARM64 CPU, you sometimes need to use
# "--platform linux/amd64" with your build command
# Example: podman build --platform linux/amd64 -t my-website:latest .

podman run -d \
--name webster2 \
-p 8081:80 \
my-website:latest

# Browse to http://localhost:8081/

podman exec -it webster2 bash

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

podman rm -f webster2

podman run -d \
--name webster2 \
-p 8081:80 \
my-website:latest

# Browse to http://localhost:8081/
```

What commands have we learned

- podman run
- podman ps
- podman logs
- podman stop
- podman rm
- podman exec
- podman build

Other common ones are

- podman create
- podman start
- podman pull
- podman push
- podman images
- podman inspect
- podman cp
- podman tag

## Compose

https://betterstack.com/community/guides/scaling-docker/podman-compose/

```sh
mkdir ~/Downloads/compose-demo
cd ~/Downloads/compose-demo

cat << EOF > compose.yaml
services:
  web:
    image: nginx
    ports:
      - "8000:80"
EOF

podman compose up

# Browse to http://localhost:8000/


```




```sh
cd ~/Downloads/containerization-workshop-main

podman compose up -d mysql
podman compose logs -f mysql

podman compose up -d todo-app
podman compose logs -f todo-app

podman compose run --rm --service-ports todo-app /bin/sh

yarn install
yarn run dev


```


Let's try a podman compose example

```sh
cd ~/Downloads

wget -O containerization-workshop.zip https://github.com/dartmouth/containerization-workshop/archive/refs/heads/main.zip

unzip containerization-workshop.zip

rm -f containerization-workshop.zip

cd containerization-workshop-main
cat docker-compose.yml

mkdir todo-mysql-data
sudo chown 999:999 todo-mysql-data
sudo chmod 777 todo-mysql-data

podman compose up -d

# Note, if you're on an ARM64 CPU
# Error: no matching manifest for linux/arm64/v8 in the manifest list entries
# Fix: after line 6, after "image: mysql:5.7", add
# platform: linux/amd64

podman ps

podman compose logs -f

# Browse to http://localhost:3000/

podman compose down
```

Next: [Examples](4-examples.md)

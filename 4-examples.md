# Examples

## Minecraft

```sh
podman run -d -it \
--name minecraft \
-p 25565:25565 \
-e EULA=TRUE \
-e MEMORY=2G \
-e PVP=false \
-e DIFFICULTY=easy \
-e MODE=survival \
-e SERVER_NAME=GagneServer \
-e MOTD="Gagne Docker server" \
-e OPS="ElijahGagne BenjaminGagne" \
-e LEVEL="ForeverWorld" \
-v $HOME/minecraft-data:/data \
itzg/minecraft-server
```

## Let's Encrypt Certbot

```sh
podman run --rm --name certbot -ti ubuntu bash

apt update \
&& apt install -y python3 python3-venv \
&& python3 -m venv /opt/certbot/ \
&& /opt/certbot/bin/pip install --upgrade pip \
&& /opt/certbot/bin/pip install certbot \
&& ln -s /opt/certbot/bin/certbot /usr/bin/certbot

certbot certonly --key-type rsa \
--manual \
--agree-tos \
--preferred-challenges dns-01 \
--server https://acme-v02.api.letsencrypt.org/directory \
-m research.computing@dartmouth.edu \
-d "*.nhcoloregistry.org" -d nhcoloregistry.org
```

## Test SSH

[Linux Auth](https://github.com/dartmouth/linux-auth)

Next: [Where to next?](5-where-to-next.md)

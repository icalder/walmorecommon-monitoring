# Traefik

## Setup
First, run init.sh to create data folders for the containers to mount.

If you want to create the frontend network independently of the dockerfile:

`docker network create -d bridge traefik-proxy_frontend`

## Connect to traefik dashboard
The dashboard is bound to localhost only.
```
ssh -N -L 8080:localhost:8080 user@host
http://localhost:8080/dashboard/#/
```


## Generate a self-signed SSL cert for testing
```
openssl req -x509 -config openssl.cnf -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.crt
chmod 644 cert.crt
chmod 600 cert.key
```
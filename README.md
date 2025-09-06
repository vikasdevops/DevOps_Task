DevOps-Task

Steps: 1

```bash
 redhat@vikas:~/Knovator$ tree
.
├── backend
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── docker-compose.yml
├── frontend
│   ├── Dockerfile
│   ├── package.json
│   ├── public
│   │   └── index.html
│   └── src
│       └── index.js
└── nginx.conf

```
```
redhat@vikas:~/Knovator$ docker compose build --no-cache
WARN[0000] /home/redhat/Knovator/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Building 99.4s (25/25) FINISHED
 => [internal] load local bake definitions                                                                                                                                                               0.0s
 => => reading from stdin 1.00kB                                                                                                                                                                         0.0s
 => [frontend internal] load build definition from Dockerfile                                                                                                                                            0.0s
 => => transferring dockerfile: 269B                                                                                                                                                                     0.0s
 => [backend internal] load build definition from Dockerfile                                                                                                                                             0.0s
 => => transferring dockerfile: 279B                                                                                                                                                                     0.0s
 => [frontend internal] load metadata for docker.io/library/nginx:alpine                                                                                                                                 3.2s
 => [frontend internal] load metadata for docker.io/library/node:18-alpine                                                                                                                               3.6s
 => [backend internal] load .dockerignore                                                                                                                                                                0.0s
 => => transferring context: 2B                                                                                                                                                                          0.0s
 => [frontend internal] load .dockerignore                                                                                                                                                               0.0s
 => => transferring context: 2B                                                                                                                                                                          0.0s
 => [frontend build 1/5] FROM docker.io/library/node:18-alpine@sha256:8d6421d663b4c28fd3ebc498332f249011d118945588d0a35cb9bc4b8ca09d9e                                                                   0.0s
 => [backend internal] load build context                                                                                                                                                                0.0s
 => => transferring context: 93B                                                                                                                                                                         0.0s
 => CACHED [backend build 2/5] WORKDIR /app                                                                                                                                                              0.0s
 => CACHED [frontend stage-1 1/2] FROM docker.io/library/nginx:alpine@sha256:42a516af16b852e33b7682d5ef8acbd5d13fe08fecadc7ed98605ba5e3b26ab8                                                            0.0s
 => [frontend internal] load build context                                                                                                                                                               0.0s
 => => transferring context: 182B                                                                                                                                                                        0.0s
 => [backend build 3/5] COPY package*.json ./                                                                                                                                                            0.0s
 => [frontend build 3/6] COPY package*.json ./                                                                                                                                                           0.1s
 => [backend build 4/5] RUN npm install --production                                                                                                                                                    14.2s
 => [frontend build 4/6] RUN npm install                                                                                                                                                                10.4s
 => [frontend build 5/6] COPY . .                                                                                                                                                                        0.1s
 => [frontend build 6/6] RUN npm run build                                                                                                                                                              84.8s
 => [backend build 5/5] COPY . .                                                                                                                                                                         0.1s
 => [backend stage-1 3/3] COPY --from=build /app .                                                                                                                                                       0.2s
 => [backend] exporting to image                  

 
```
```
redhat@vikas:~/Knovator$ docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                                       NAMES
f7d3240dee35   knovator-frontend   "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp           frontend
13551ed567d6   knovator-backend    "/bin/sh -c 'node se…"   4 seconds ago   Up 3 seconds   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   backend

```
```
 OUTPUT

redhat@vikas:~/Knovator$ curl http://localhost:5000/api
{"message":"Hello from backend!"}redhat@vikas:~/Knovator$

```
```

redhat@vikas:~/Knovator$ curl http://localhost
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>React Frontend</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>

```

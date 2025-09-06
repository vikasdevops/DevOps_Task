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
# ===================================================================================================================#

# Step 2: I have used the jenkins CI/CD:   
```
===============       =================       ===================
│   Developer │       │    Jenkins    │       │  Docker Registry│
│(Push Code/git) │───▶│  (Self-hosted │──────▶│ (DockerHub /   |
================       │   Runner VM)  │       │  Private Repo/jfrog) │
                      =================       =====================
                              │
                              │ (SSH with Key)
                              ▼
                       =================
                       │ Deployment   │
                       │   Server VM  │
                       │ (Docker +    │
                       │ docker-compose) │
                       =====================
                               │
                               ▼
                      ====================
                        │   End User  │
                        │ (Access via │
                        │   Browser)  │
                     =====================





```
# Lets make it more secure  jenkins CI/CD:

```
                        ==================
                        │ Developer   │
                        │ (Push Code) │
                      =====================
                                │ HTTPS (TLS)
                                ▼
                      ==========================
                        │ Git Provider     │
                        │ (GitHub/GitLab) │
                      ======================
                                 │ Webhook (HTTPS + Secret)
                                 ▼
                      ==========================================
                        │   Jenkins Server (Self-hosted VM)  │
                        │────────────────────────────────────│
                        │  - RBAC Enabled (Admin/User roles) │
                        │  - Credentials Vault (SSH, Tokens) │
                        │  - HTTPS Reverse Proxy (Nginx+SSL) │
                        │  - Security Plugins (OWASP, Audit) │
                        │  - Runs Builds in Isolated Agents  │
                    ==============================================
                                        │
                                        │ Docker Build (isolated network)
                                        ▼
                  ==============================================
                              │ Docker Registry   │
                              │ (DockerHub/ECR)  │
                              │ - Private Repo   │
                              │ - Access via     │
                              │   Jenkins Token  │
                  ========================================
                                        │
                                  Encrypted Pull (TLS)
                                        │
                                        ▼
                          ===================================
                             │ Deployment VM (Server)   │
                             │──────────────────────────│
                             │ - Docker + Compose       │
                             │ - Only SSH key access    │
                             │ - Firewall rules enabled │
                             │ - Runs frontend/backend  │
                             │   behind Nginx reverse   │
                             │   proxy (HTTPS, WAF)     │
                             │ - Logs to monitoring     │
                          ===================================
                                         │
                                         ▼
                                 =======================
                                  │   End User  │
                                  │ (Browser)   │
                                =======================

```

# Lets explain above layers:

* Key Security Layers

## Jenkins Security

* Set up  (RBAC) so that admins and developers don’t share the same level of permissions.

* Keep all passwords, tokens, and SSH keys inside Jenkins Credentials Manager instead of hardcoding them in pipelines.

* Put Jenkins behind an Nginx reverse proxy and enable SSL/TLS certificates to secure web access.

## Pipeline Security

* Allow Jenkins to pull code only from trusted Git repositories.

* Make sure the build process runs inside isolated Docker agents so that builds don’t interfere with Jenkins itself.

* Run an image vulnerability scan (using tools like Trivy or Anchore) before pushing or deploying the image.

## Registry Security

* Use a private Docker registry so images aren’t publicly exposed.

* Configure authentication with tokens/credentials stored in Jenkins.

* Enable Docker Content Trust or another method of image signing so you know the images haven’t been tampered with.

## Deployment Security

* Only allow access to the deployment server using SSH keys (no password login).

* Always run containers as a non-root user inside Docker for safety.

* Protect services with Nginx (HTTPS) and set up a firewall (UFW or iptables) to restrict open ports.

* Forward logs to a monitoring stack like ELK or Prometheus for visibility and auditing.


## User Security

* End users should only connect to the application through HTTPS.

* For extra protection, consider adding a Web Application Firewall (WAF) in front of Nginx.

# ===================================================================================================================#

# Steps 3: 

```
                    (Internet)
                         |
                    CDN/WAF (Cloudflare/ALB + WAF)
                          |
                    =================
                    │ Load Balancer│  <-- TLS termination possible here
                    =================
                               |
                               |
                    ================================================
                    │                     │
                    Web / Nginx (reverse)        Web / Nginx (reverse)
                    + Static (cacheable)          + Static (cacheable)
                    │                     │
                  ==============================
                                    │
                                    |
                    PHP/FPM (containers/pods)  <-- horizontal scale(HPA)
                                    │
                    Queue workers (Laravel queue + Horizon)  <-- scale separately
                                │
                    ==============================
                    │   Shared Services      │
                    │ ========  ============     ===========
                    │ │Redis│    │MySQL│       │Elastic │
                    │ │(cache,│  │(primary│    │Search  │
                    │ │session)│ │+ replica)│ │cluster)│
                    │ =========  =============   ============
                  ====================================

                    Logging/Monitoring: ELK / Prometheus+Grafana, centralized
                    Secrets: Vault / Cloud KMS / Jenkins credentials
                    CI/CD: GitHub Actions / Jenkins => build images => deploy to cluster
                    Backups: Managed snapshots for DB, daily/weekly retention

```

# Dockerize Angular application 📦 <!-- omit in toc -->

Dockerize an Angular app with nginx.

❗This configuration is not recommended for Search Engine Optimization(SEO).

## Steps

Add `docker-compose.yml`.

```yml
version: '3.1'

services:
  app:
    image: '<IMAGE_NAME>'
    build: '.'
    ports:
      - 3000:80
```

Start the app with [Docker Compose](https://docs.docker.com/compose/).

```shell
docker compose build
docker compose up -d
```

Add npm scripts in `package.json` to manually build and run docker image.

```shell
"build:docker": "docker build --rm -f Dockerfile -t <IMAGE_NAME>:<TAG_VERSION> .",
"start:docker": "docker run --rm -d  -p 80:80/tcp <IMAGE_NAME>:<TAG_VERSION>",
```

Add `Dockerfile`.

```Dockerfile
# Step 1: Build

FROM node:lts-alpine as build-step

RUN mkdir -p /app

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build:prod

# Stage 2: Serving

FROM nginx:stable-alpine

COPY nginx.conf /etc/nginx/nginx.conf

COPY /resources/* /usr/share/nginx/resources/

WORKDIR /usr/share/nginx/html

RUN rm -rf ./*

COPY --from=build-step /app/dist/<PROJECT_NAME> .
```

⚠️ Replace `<PROJECT_NAME>` with the actual project name.

⚠️ The instruction in line 23 `COPY /resources/* /usr/share/nginx/resources/` will make build process fail if no source folder `resources` exists. This folder cannot be empty.
ℹ️ Line 23 instruction is used to serve static files like preview images. If it's not used, remove this instruction.

Add npm scripts in `package.json` to build static files for production:

```json
"build:prod": "ng build --configuration production",
```

Add nginx configuration file `nginx.conf` at project root level.

```nginx.conf
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    server {
        listen       80;
        listen  [::]:80;
        server_name  <URL> www.<URL>;

        root   /usr/share/nginx/html;
        index  index.html;
        include /etc/nginx/mime.types;

        gzip on;
        gzip_disable "msie6";
        gzip_vary on;
        gzip_proxied any;
        gzip_comp_level 6;
        gzip_buffers 16 8k;
        gzip_http_version 1.1;
        gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;

        # match all HTML files and service worker script (/sw.js).
        # Neither should be cached.
        location ~* (\.html|\/sw\.js)$ {
            expires -1y;
            add_header Pragma "no-cache";
            add_header Cache-Control "public";
        }

        # Caching for all static assets
        location ~* \.(js|css|svg|webp|png|jpg|jpeg|gif|ico|json|otf|)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }

        # Serve static files in /usr/share/nginx/resources/
        location ^~ /resources/  {
            root    /usr/share/nginx;
        }

        # Redirect to index.html for every route that doesn’t match a static file to be handled by SPA Router
        location / {
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

⚠️ Replace `<URL>` with the actual site URL. Ex.: URL = 'mysite.domain'.

Add `.dockerignore` file.

```dockerignore
.git

.editorconfig

/e2e

/gitignore

*.zip

*.md

# compiled output
/dist
/docs
/tmp
/out-tsc
# Only exists if Bazel was run
/bazel-out

# dependencies
/node_modules

# profiling files
chrome-profiler-events*.json
speed-measure-plugin*.json

# IDEs and editors
/.idea
.project
.classpath
.c9/
*.launch
.settings/
*.sublime-workspace

# IDE - VSCode
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json
.history/*

# misc
/.angular/cache
/.sass-cache
/connect.lock
/coverage
/libpeerconnection.log
npm-debug.log
yarn-error.log
testem.log
/typings

# System Files
.DS_Store
Thumbs.db
```

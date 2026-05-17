# nginx-http-starter

A minimal NGINX static server starter for local development and Docker deployment.

## Features

- Static file hosting from `public/`
- Native NGINX workflow for local development
- Docker and Docker Compose support
- Gzip enabled in HTTP config
- Cache headers for common static assets
- Clean, small starter structure

## Project Structure

```text
nginx-http-server-starter/
├── .dockerignore
├── .editorconfig
├── .gitignore
├── LICENSE
├── README.md
├── conf.d/
│   └── server.conf
├── docker-compose.yaml
├── dockerFile
├── mime.types
├── nginx.conf
└── public/
    └── index.html
```

## Quick Start

Use one of the two options below.

## Option A: Native NGINX (Local)

### Start

From the project root:

```bash
nginx -c $(pwd)/nginx.conf -p $(pwd)
```

Open: http://localhost:8080

### Validate Config

```bash
nginx -t -c $(pwd)/nginx.conf -p $(pwd)
```

### Reload After Config Changes

```bash
nginx -s reload -c $(pwd)/nginx.conf -p $(pwd)
```

### Stop

```bash
nginx -s stop -c $(pwd)/nginx.conf -p $(pwd)
```

## Option B: Docker

### Run With Compose

```bash
docker compose -f docker-compose.yaml up --build
```

Open: http://localhost:8080

### Stop Compose

```bash
docker compose -f docker-compose.yaml down
```

### Build and Run Manually

```bash
docker build -f dockerFile -t nginx-http-starter .
docker run --rm -p 8080:80 nginx-http-starter
```

## Configuration Notes

- Main config: `nginx.conf`
- Server block: `conf.d/server.conf`
- MIME definitions: `mime.types`
- Static content: `public/index.html`

Native mode serves from `./public` and listens on port `8080` via `conf.d/server.conf`.

Docker mode currently maps host `8080` to container `80` (`docker-compose.yaml`) and uses the image configuration copied by `dockerFile`.

## Development Flow

```bash
# 1) Start locally
nginx -c $(pwd)/nginx.conf -p $(pwd)

# 2) Edit files
#    - public/index.html
#    - conf.d/server.conf
#    - nginx.conf

# 3) Validate and reload
nginx -t -c $(pwd)/nginx.conf -p $(pwd)
nginx -s reload -c $(pwd)/nginx.conf -p $(pwd)

# 4) Stop
nginx -s stop -c $(pwd)/nginx.conf -p $(pwd)
```